---
layout: post
title: Window上在MSYS2环境中安装ruby以及gem的使用
---

要在windows上设置jekyll服务，需要安装ruby环境。查看jekyll文档后，发现需要依赖msys2或mingw环境。由于已经
有msys2 ucrt64环境，故而直接使用`pacman -S ruby`安装ruby。但是这个命令安装的ruby版本，编译方式非正常。导致gem无法正常使用，安装jekyll时出现open ssl错误。

```
... ...% gem install jekyll
ERROR:  While executing gem ... (Gem::Exception)
    OpenSSL is not available. Install OpenSSL and rebuild Ruby or use non-HTTPS sources (Gem::Exception)
        /usr/lib/ruby/3.3.0/rubygems/request.rb:53:in `configure_connection_for_https'
        /usr/lib/ruby/3.3.0/rubygems/request/https_pool.rb:7:in `setup_connection'
        /usr/lib/ruby/3.3.0/rubygems/request/http_pool.rb:40:in `make_connection'
        /usr/lib/ruby/3.3.0/rubygems/request/http_pool.rb:21:in `checkout'
        ...
        /usr/lib/ruby/3.3.0/rubygems/command_manager.rb:152:in `run'
        /usr/lib/ruby/3.3.0/rubygems/gem_runner.rb:56:in `run'
        /bin/gem:12:in `<main>'
```
问题在于msys2包仓库中基础的ruby版本编译时没有配置openssl。而gem对openssl有依赖，这样安装的ruby环境其实不完整。要正常使用gem，需要安装ucrt64等具体环境下(包含具体的gcc等编译工具链)编译的ruby包。这种ruby的编译才完整，gem可以正常使用。

在msys2[包仓库站点](https://packages.msys2.org/repos)查找ucrt64环境下的对应ruby版本，找到了对应的ruby包名为`mingw-w64-ucrt-x86_64-ruby`。执行安装命令`pacman -S mingw-w64-ucrt-x86_64-ruby`。


```
pacman -S mingw-w64-ucrt-x86_64-ruby
正在解析依赖关系...
正在查找软件包冲突...

软件包 (3) mingw-w64-ucrt-x86_64-gdbm-1.19-3  mingw-w64-ucrt-x86_64-libyaml-0.2.5-2  mingw-w64-ucrt-x86_64-ruby-3.3.6-1

下载大小：       8.12 MiB
全部安装大小：  33.84 MiB

:: 进行安装吗？ [Y/n] y
...
```

此ruby版本安装后，gem命令正常。可以下载对应的jekyll包使用。

> msys2可能需要预先安装编译工具链，以ucrt64环境为例(`pacman -S mingw-w64-ucrt-x86_64-toolchain`)。

