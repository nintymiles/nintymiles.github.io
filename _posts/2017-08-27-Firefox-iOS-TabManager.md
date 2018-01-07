---
layout: post
title: Firefox-iOS 源代码学习
---

## TabManager.swift

- TabManager通过对subscript(index:int) subscript(webView:WKWebView)对下标进行了重新定义，第一个可以通过index查找到tab，并且即便index溢出也不crash。第二个通过webView实例查找其是否是tabs元素。


```    
//重载定义了下标函数，如果index越界则返回nil，而不是抛出异常。
    subscript(index: Int) -> Tab? {
        assert(Thread.isMainThread)

        if index >= tabs.count {
            return nil
        }
        return tabs[index]
    }

    //重载定义了下标，支持以元素类型查找元素
    subscript(webView: WKWebView) -> Tab? {
        assert(Thread.isMainThread)

        for tab in tabs where tab.webView === webView {
            return tab
        }

        return nil
    }
```
- 通过Swift的where（循环中）和filter（数组）语法，可以快速查找对应元素

```
//通过tab使用where语句反向查找index
    func getIndex(_ tab: Tab) -> Int? {
        assert(Thread.isMainThread)

        for i in 0..<count where tabs[i] === tab {
            return i
        }

        assertionFailure("Tab not in tabs list")
        return nil
    }

    func getTabForURL(_ url: URL) -> Tab? {
        assert(Thread.isMainThread)

        //用filter通过tab的属性反向查找
        return tabs.filter { $0.webView?.url == url } .first
    }
```

## Logger.swift
### log 文件路径地址维护
    
```Swift
//log file saves at user sanbox domain dir
static func logFileDirectoryPath() -> String? {
        //Logfile saves at Library/Caches directory
        if let cacheDir = NSSearchPathForDirectoriesInDomains(.cachesDirectory, .userDomainMask, true).first {
            let logDir = "\(cacheDir)/Logs"
            if !FileManager.default.fileExists(atPath: logDir) {
                do {
                    try FileManager.default.createDirectory(atPath: logDir, withIntermediateDirectories: false, attributes: nil)
                    return logDir
                } catch _ as NSError {
                    return nil
                }
            } else {
                return logDir
            }
        }

        return nil
    }
```

### 绑定文件到logger

```
    static private func fileLoggerWithName(_ name: String) -> XCGLogger {
        let log = XCGLogger()
        //binding filepath to logger seems redudunt
        if let logFileURL = urlForLogNamed(name) {
            let fileDestination = FileDestination(
                owner: log,
                writeToFile: logFileURL.absoluteString,
                identifier: "com.mozilla.firefox.filelogger.\(name)"
            )
            log.add(destination: fileDestination)
        }
        return log
    }
```

### 读取已经写入磁盘的log文件
这个funct
ion实现很能体现swift的语法特征。

```
    static func diskLogFilenamesAndData() throws -> [(String, Data?)] {
        var filenamesAndURLs = [(String, URL)]()
        filenamesAndURLs.append(("browser", urlForLogNamed("browser")!))
        filenamesAndURLs.append(("keychain", urlForLogNamed("keychain")!))

			//mutable collection可以直接用重载的+=操作符合并
        // Grab all sync log files
        do {
            filenamesAndURLs += try syncLogger.logFilenamesAndURLs()
            filenamesAndURLs += try corruptLogger.logFilenamesAndURLs()
            filenamesAndURLs += try browserLogger.logFilenamesAndURLs()
        } catch _ {
        }

			//然后使用collection的Functional Programming函数对list内容进行重整
        return filenamesAndURLs.map { ($0, try? Data(contentsOf: URL(fileURLWithPath: $1.absoluteString))) }
    }
```

### 使用struct构建logger的sigletion实例，struct本体留空不知什么用意？（只是一种风格）

```
public struct Logger {}

// MARK: - Singleton Logger Instances
public extension Logger {
    static let logPII = false

    /// Logger used for recording happenings with Sync, Accounts, Providers, Storage, and Profiles
    static let syncLogger = RollingFileLogger(filenameRoot: "sync", logDirectoryPath: Logger.logFileDirectoryPath())

    /// Logger used for recording frontend/browser happenings
    static let browserLogger = RollingFileLogger(filenameRoot: "browser", logDirectoryPath: Logger.logFileDirectoryPath())

    /// Logger used for recording interactions with the keychain
    static let keychainLogger: XCGLogger = Logger.fileLoggerWithName("keychain")

    /// Logger used for logging database errors such as corruption
    static let corruptLogger: RollingFileLogger = {
        let logger = RollingFileLogger(filenameRoot: "corruptLogger", logDirectoryPath: Logger.logFileDirectoryPath())
        logger.newLogWithDate(Date())
        return logger
    }()
    ...
}
```


## WebServer.Swift
###对特定的http method和path应用handler

```
    /// Convenience method to register a dynamic handler. Will be mounted at $base/$module/$resource
    func registerHandlerForMethod(_ method: String, module: String, resource: String, handler: @escaping (_ request: GCDWebServerRequest?) -> GCDWebServerResponse!) {
        //对closure的再包装，可以对其应用前置或后置处理，比如参数范围限制。
        // Prevent serving content if the requested host isn't a whitelisted local host.
        let wrappedHandler = {(request: GCDWebServerRequest?) -> GCDWebServerResponse? in
            guard let request = request, request.url.isLocal else {
                return GCDWebServerResponse(statusCode: 403)
            }

            return handler(request)
        }
        //GCDWebServer will apply hander to the method and path
        server.addHandler(forMethod: method, path: "/\(module)/\(resource)", request: GCDWebServerRequest.self, processBlock: wrappedHandler)
    }
```

### 本地Local WebServer的启动
 
```
    //启动本地的GCDWebServer，Swift特性丰富（丢弃结果，try语句）
    @discardableResult func start() throws -> Bool {
        if !server.isRunning {
            try server.start(options: [
                GCDWebServerOption_Port: 6571,
                GCDWebServerOption_BindToLocalhost: true,
                GCDWebServerOption_AutomaticallySuspendInBackground: true,
                GCDWebServerOption_AuthenticationMethod: GCDWebServerAuthenticationMethod_Basic,
                GCDWebServerOption_AuthenticationAccounts: [sessionToken: ""]
            ])
        }
        return server.isRunning
    }
```

### Swift中Singleton方式多样？

```
class WebServer {
	 //static const variable,static前面加上private更佳
    static let WebServerSharedInstance = WebServer()
	// class computed method
    class var sharedInstance: WebServer {
        return WebServerSharedInstance
    }
    。。。
 }
```

### 将MainBundle中的文件或特定类型的多个文件指定为可供getHandler处理的资源

```
    /// Convenience method to register a resource in the main bundle. Will be mounted at $base/$module/$resource
    func registerMainBundleResource(_ resource: String, module: String) {
        if let path = Bundle.main.path(forResource: resource, ofType: nil) {
            server.addGETHandler(forPath: "/\(module)/\(resource)", filePath: path, isAttachment: false, cacheAge: UInt.max, allowRangeRequests: true)
        }
    }

    /// Convenience method to register all resources in the main bundle of a specific type. Will be mounted at $base/$module/$resource
    func registerMainBundleResourcesOfType(_ type: String, module: String) {
        for path: String in Bundle.paths(forResourcesOfType: type, inDirectory: Bundle.main.bundlePath) {
            if let resource = NSURL(string: path)?.lastPathComponent {
                server.addGETHandler(forPath: "/\(module)/\(resource)", filePath: path as String, isAttachment: false, cacheAge: UInt.max, allowRangeRequests: true)
            } else {
                log.warning("Unable to locate resource at path: '\(path)'")
            }
        }
    }
```

## main.swift


```
private var appDelegate: AppDelegate.Type

//auto run for this code block？run when loading into memory？
if AppConstants.IsRunningTest {
    appDelegate = TestAppDelegate.self
} else {
    appDelegate = AppDelegate.self
}

//CommandLine为Swift命令行参数封装对象，argc为字符(int)参数，argv为数组参数？
private let pointer = UnsafeMutableRawPointer(CommandLine.unsafeArgv).bindMemory(to: UnsafeMutablePointer<Int8>.self, capacity: Int(CommandLine.argc))
//firefox程序的主入口main函数
UIApplicationMain(CommandLine.argc, pointer, NSStringFromClass(UIApplication.self), NSStringFromClass(appDelegate))
```

