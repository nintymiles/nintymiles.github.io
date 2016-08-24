---
layout: post
title:  Learning Notes - Swift Version check in XCode
---

## How to check Swift version in XCode?

- 查看Swift版本
`$ xcrun swift -version`

```
localhost:~ seanren$ xcrun swift -version
Apple Swift version 2.2 (swiftlang-703.0.18.8 clang-703.0.31)
Target: x86_64-apple-macosx10.9
```

- 查看 Swift utility program 在 Xcode 中的位置
`$ xcrun --find swift`

```
$ xcrun --find swift
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swift
$ xcrun --find clang
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang
```

- xcrun command depiction

	xcrun  provides  a  means  to **locate** or **invoke** developer tools from the
command-line, without requiring users to modify Makefiles or  otherwisetake inconvenient measures to support multiple Xcode tool chains.

	The tool xcode-select(1) is used to set a system default for the active
developer directory, and may be overridden by the  DEVELOPER_DIR  environment variable.

