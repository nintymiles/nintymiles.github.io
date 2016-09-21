---
layout: post
title:  Learning Notes - CoreStoreError 代码解读
---
##`CoreStoreError`代码解读
Swift CoreStore 框架的Error设计，所有的错误都用`CoreStoreError`表达。`CoreStoreError`主体由2个enum组成，分别为`CoreStoreError`，`CoreStoreErrorCode`，Error的编码由`CoreStoreError`使用computed property基于`CoreStoreErrorCode`来实现。`CoreStoreError`另有一个Computed Property用来指定domain。Domain固定返回一个全局变量（`CoreStoreErrorDomain`）的值。

重载 `==` 函数，判断enum是否相等。 


```
// MARK: - CoreStoreError

/**
 All errors thrown from CoreStore are expressed in `CoreStoreError` enum values.
 */
public enum CoreStoreError: ErrorType, Hashable {
    
    /**
     A failure occured because of an unknown error.
     */
    case Unknown
    
    /**
     The `NSPersistentStore` could not be initialized because another store existed at the specified `NSURL`.
     */
    case DifferentStorageExistsAtURL(existingPersistentStoreURL: NSURL)
    
    /**
     An `NSMappingModel` could not be found for a specific source and destination model versions.
     */
    case MappingModelNotFound(localStoreURL: NSURL, targetModel: NSManagedObjectModel, targetModelVersion: String)
    
    /**
     Progressive migrations are disabled for a store, but an `NSMappingModel` could not be found for a specific source and destination model versions.
     */
    case ProgressiveMigrationRequired(localStoreURL: NSURL)
    
    /**
     An internal SDK call failed with the specified `NSError`.
     */
    case InternalError(NSError: NSError)
    
    
    // MARK: ErrorType
    
    public var _domain: String {
        
        return CoreStoreErrorDomain
    }
    
    public var _code: Int {
    
        switch self {
            
        case .Unknown:
            return CoreStoreErrorCode.UnknownError.rawValue
            
        case .DifferentStorageExistsAtURL:
            return CoreStoreErrorCode.DifferentPersistentStoreExistsAtURL.rawValue
            
        case .MappingModelNotFound:
            return CoreStoreErrorCode.MappingModelNotFound.rawValue
            
        case .ProgressiveMigrationRequired:
            return CoreStoreErrorCode.ProgressiveMigrationRequired.rawValue
            
        case .InternalError:
            return CoreStoreErrorCode.InternalError.rawValue
        }
    }
    
    
    // MARK: Hashable
    
    public var hashValue: Int {
        
        let code = self._code
        switch self {
            
        case .Unknown:
            return code.hashValue
            
        case .DifferentStorageExistsAtURL(let existingPersistentStoreURL):
            return code.hashValue ^ existingPersistentStoreURL.hashValue
            
        case .MappingModelNotFound(let localStoreURL, let targetModel, let targetModelVersion):
            return code.hashValue ^ localStoreURL.hashValue ^ targetModel.hashValue ^ targetModelVersion.hashValue
            
        case .ProgressiveMigrationRequired(let localStoreURL):
            return code.hashValue ^ localStoreURL.hashValue
            
        case .InternalError(let NSError):
            return code.hashValue ^ NSError.hashValue
        }
    }
    
    
    // MARK: Internal
    
    internal init(_ error: ErrorType?) {
        
        self = error.flatMap { $0.bridgeToSwift } ?? .Unknown
    }
}


// MARK: - CoreStoreError: Equatable

@warn_unused_result
public func == (lhs: CoreStoreError, rhs: CoreStoreError) -> Bool {
    
    switch (lhs, rhs) {
        
    case (.Unknown, .Unknown):
        return true
        
    case (.DifferentStorageExistsAtURL(let url1), .DifferentStorageExistsAtURL(let url2)):
        return url1 == url2
        
    case (.MappingModelNotFound(let url1, let model1, let version1), .MappingModelNotFound(let url2, let model2, let version2)):
        return url1 == url2 && model1 == model2 && version1 == version2
        
    case (.ProgressiveMigrationRequired(let url1), .ProgressiveMigrationRequired(let url2)):
        return url1 == url2
        
    case (.InternalError(let NSError1), .InternalError(let NSError2)):
        return NSError1 == NSError2
        
    default:
        return false
    }
}


// MARK: - CoreStoreErrorDomain

/**
 The `NSError` error domain string for `CSError`.
 */
@nonobjc
public let CoreStoreErrorDomain = "com.corestore.error"


// MARK: - CoreStoreErrorCode

/**
 The `NSError` error codes for `CoreStoreErrorDomain`.
 */
public enum CoreStoreErrorCode: Int {
    
    /**
     A failure occured because of an unknown error.
     */
    case UnknownError
    
    /**
     The `NSPersistentStore` could note be initialized because another store existed at the specified `NSURL`.
     */
    case DifferentPersistentStoreExistsAtURL
    
    /**
     An `NSMappingModel` could not be found for a specific source and destination model versions.
     */
    case MappingModelNotFound
    
    /**
     Progressive migrations are disabled for a store, but an `NSMappingModel` could not be found for a specific source and destination model versions.
     */
    case ProgressiveMigrationRequired
    
    /**
     An internal SDK call failed with the specified "NSError" userInfo key.
     */
    case InternalError
}


// MARK: - NSError

public extension NSError {
    
    // MARK: Internal
    
    internal var isCoreDataMigrationError: Bool {
        
        let code = self.code
        return (code == NSPersistentStoreIncompatibleVersionHashError
            || code == NSMigrationMissingSourceModelError
            || code == NSMigrationError)
            && self.domain == NSCocoaErrorDomain
    }
    
    
    // MARK: Deprecated

    /**
     Deprecated. Use `CoreStoreError` enum values instead.
     
     If the error's domain is equal to `CoreStoreErrorDomain`, returns the associated `CoreStoreErrorCode`. For other domains, returns `nil`.
     */
    @available(*, deprecated=2.0.0, message="Use CoreStoreError enum values instead.")
    public var coreStoreErrorCode: CoreStoreErrorCode? {
        
        return (self.domain == CoreStoreErrorDomain
            ? CoreStoreErrorCode(rawValue: self.code)
            : nil)
    }
}
```


