---
layout: post
title: Window上在MSYS2环境中安装ruby以及使用gem
---


直接使用`pacman -S ruby`安装的是基础版本的ruby，没有关联具体的gcc等工具链。安装完成后出现open ssl错误。需要安装具体环境下的ruby工具包。这种ruby的编译才完整。gem可以正常使用。

```
ultraman@Administrator ultraman % gem install jekyll
ERROR:  While executing gem ... (Gem::Exception)
    OpenSSL is not available. Install OpenSSL and rebuild Ruby or use non-HTTPS sources (Gem::Exception)
        /usr/lib/ruby/3.3.0/rubygems/request.rb:53:in `configure_connection_for_https'
        /usr/lib/ruby/3.3.0/rubygems/request/https_pool.rb:7:in `setup_connection'
        /usr/lib/ruby/3.3.0/rubygems/request/http_pool.rb:40:in `make_connection'
        /usr/lib/ruby/3.3.0/rubygems/request/http_pool.rb:21:in `checkout'
        /usr/lib/ruby/3.3.0/rubygems/request.rb:135:in `connection_for'
        /usr/lib/ruby/3.3.0/rubygems/request.rb:193:in `perform_request'
        /usr/lib/ruby/3.3.0/rubygems/request.rb:160:in `fetch'
        /usr/lib/ruby/3.3.0/rubygems/remote_fetcher.rb:317:in `request'
        /usr/lib/ruby/3.3.0/rubygems/remote_fetcher.rb:213:in `fetch_http'
        /usr/lib/ruby/3.3.0/rubygems/remote_fetcher.rb:252:in `fetch_path'
        /usr/lib/ruby/3.3.0/rubygems/source.rb:86:in `dependency_resolver_set'
        /usr/lib/ruby/3.3.0/rubygems/resolver/best_set.rb:24:in `block in pick_sets'
        /usr/lib/ruby/3.3.0/rubygems/source_list.rb:94:in `each'
        /usr/lib/ruby/3.3.0/rubygems/source_list.rb:94:in `each_source'
        /usr/lib/ruby/3.3.0/rubygems/resolver/best_set.rb:23:in `pick_sets'
        /usr/lib/ruby/3.3.0/rubygems/resolver/best_set.rb:29:in `find_all'
        /usr/lib/ruby/3.3.0/rubygems/resolver/installer_set.rb:174:in `find_all'
        /usr/lib/ruby/3.3.0/rubygems/resolver/installer_set.rb:62:in `add_always_install'
        /usr/lib/ruby/3.3.0/rubygems/dependency_installer.rb:318:in `resolve_dependencies'
        /usr/lib/ruby/3.3.0/rubygems/commands/install_command.rb:198:in `install_gem'
        /usr/lib/ruby/3.3.0/rubygems/commands/install_command.rb:223:in `block in install_gems'
        /usr/lib/ruby/3.3.0/rubygems/commands/install_command.rb:216:in `each'
        /usr/lib/ruby/3.3.0/rubygems/commands/install_command.rb:216:in `install_gems'
        /usr/lib/ruby/3.3.0/rubygems/commands/install_command.rb:162:in `execute'
        /usr/lib/ruby/3.3.0/rubygems/command.rb:326:in `invoke_with_build_args'
        /usr/lib/ruby/3.3.0/rubygems/command_manager.rb:253:in `invoke_command'
        /usr/lib/ruby/3.3.0/rubygems/command_manager.rb:194:in `process_args'
        /usr/lib/ruby/3.3.0/rubygems/command_manager.rb:152:in `run'
        /usr/lib/ruby/3.3.0/rubygems/gem_runner.rb:56:in `run'
        /bin/gem:12:in `<main>'
```

https://packages.msys2.org/repos

```
pacman -S mingw-w64-ucrt-x86_64-ruby
正在解析依赖关系...
正在查找软件包冲突...

软件包 (3) mingw-w64-ucrt-x86_64-gdbm-1.19-3  mingw-w64-ucrt-x86_64-libyaml-0.2.5-2  mingw-w64-ucrt-x86_64-ruby-3.3.6-1

下载大小：       8.12 MiB
全部安装大小：  33.84 MiB

:: 进行安装吗？ [Y/n] y
:: 正在获取软件包......
 mingw-w64-ucrt-x86_64-libyaml-0.2.5-2-any    85.1 KiB   249 KiB/s 00:00 [#######################################] 100%
 mingw-w64-ucrt-x86_64-gdbm-1.19-3-any       223.3 KiB   417 KiB/s 00:01 [#######################################] 100%
 mingw-w64-ucrt-x86_64-ruby-3.3.6-1-any        7.8 MiB  3.67 MiB/s 00:02 [#######################################] 100%
 全部 (3/3)                                    8.1 MiB  3.77 MiB/s 00:02 [#######################################] 100%
(3/3) 正在检查密钥环里的密钥                                             [#######################################] 100%
(3/3) 正在检查软件包完整性                                               [#######################################] 100%
(3/3) 正在加载软件包文件                                                 [#######################################] 100%
(3/3) 正在检查文件冲突                                                   [#######################################] 100%
(3/3) 正在检查可用存储空间                                               [#######################################] 100%
:: 正在处理软件包的变化...
(1/3) 正在安装 mingw-w64-ucrt-x86_64-gdbm                                [#######################################] 100%
(2/3) 正在安装 mingw-w64-ucrt-x86_64-libyaml                             [#######################################] 100%
(3/3) 正在安装 mingw-w64-ucrt-x86_64-ruby                                [#######################################] 100%
```

