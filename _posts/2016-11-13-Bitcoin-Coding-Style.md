
---
layout: post
title: BitCoin的代码风格设定
---
## Bitcoin 基础代码风格
 - Basic rules specified in [src/.clang-format].
  Use a recent clang-format to format automatically using one of the \[dev scripts]
  (/contrib/devtools/README.md#clang-formatpy).
  - Braces on new lines for namespaces, classes, functions, methods.
  - Braces on the same line for everything else.
  - 4 space indentation (no tabs) for every block except namespaces.
  - No indentation for public/protected/private or for namespaces.
  - No extra spaces inside parenthesis; don't do ( this )
  - No space after function names; one space after if, for and while.
  - `++i` is preferred over `i++`.


### .clang-format内容

```
Language:        Cpp
AccessModifierOffset: -4
AlignAfterOpenBracket: false
AlignEscapedNewlinesLeft: true
AlignTrailingComments: true
AllowAllParametersOfDeclarationOnNextLine: false
AllowShortBlocksOnASingleLine: false
AllowShortFunctionsOnASingleLine: All
AllowShortIfStatementsOnASingleLine: false
AllowShortLoopsOnASingleLine: false
AlwaysBreakBeforeMultilineStrings: false
AlwaysBreakTemplateDeclarations: true
BinPackParameters: false
BreakBeforeBinaryOperators: false
BreakBeforeBraces: Linux
BreakBeforeTernaryOperators: false
BreakConstructorInitializersBeforeComma: false
ColumnLimit:     0
CommentPragmas:  '^ IWYU pragma:'
ConstructorInitializerAllOnOneLineOrOnePerLine: false
ConstructorInitializerIndentWidth: 4
ContinuationIndentWidth: 4
Cpp11BracedListStyle: true
DerivePointerAlignment: false
DisableFormat:   false
ForEachMacros:   [ foreach, Q_FOREACH, BOOST_FOREACH, BOOST_REVERSE_FOREACH ]
IndentCaseLabels: false
IndentFunctionDeclarationAfterType: false
IndentWidth:     4
KeepEmptyLinesAtTheStartOfBlocks: false
MaxEmptyLinesToKeep: 2
NamespaceIndentation: None
ObjCSpaceAfterProperty: false
ObjCSpaceBeforeProtocolList: false
PenaltyBreakBeforeFirstCallParameter: 1
PenaltyBreakComment: 300
PenaltyBreakFirstLessLess: 120
PenaltyBreakString: 1000
PenaltyExcessCharacter: 1000000
PenaltyReturnTypeOnItsOwnLine: 200
PointerAlignment: Left
SpaceBeforeAssignmentOperators: true
SpaceBeforeParens: ControlStatements
SpaceInEmptyParentheses: false
SpacesBeforeTrailingComments: 1
SpacesInAngles:  false
SpacesInContainerLiterals: true
SpacesInCStyleCastParentheses: false
SpacesInParentheses: false
Standard:        Cpp03
TabWidth:        8
UseTab:          Never 
```

## 代码注释风格
To facilitate（使...便利，加快） the generation of documentation, use doxygen-compatible comment blocks for functions, methods and fields.

## ？
Coins.h 中有很多实现代码
Coins.cpp 中全是类相关的实现？？


