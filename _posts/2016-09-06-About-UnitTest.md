---
layout: post
title:  Learning Notes - About Unit Test
---
##Advantages and Disadvantages of Unit TestsWhen it comes to testing, there’s good news, and bad news. The bad news is that there can be disadvantages to unit tests, like the following: 
- **More code**: In projects with high test coverage it’s possible to have more test code than functional code.- **More to maintain**: When there is more code, there is more to maintain.- **No silver bullet**: Unit tests don’t (and can’t) ensure that your code is free of bugs.- **Takes longer**: Writing tests takes time 

Although there is no silver bullet, there is a silver lining — testing has the following advantages: 

- **Confidence**: You can demonstrate that your code works.- **Quick feedback**: You can use unit tests to quickly validate code that is buried many layers deep in your app navigation — things that are cumbersome to test manually.- **Modularity**: Unit tests help keep you focused on writing more modular code.- **Focus**: Writing tests for micro features keep you focused on the small details.- **Regression**: Be sure that the bugs you fixed stay fixed — and aren’t broken by subsequent fixes.- **Refactoring**: Until Xcode gets smart enough to refactor your code on its own, you’ll need unit tests to validate your refactoring.- **Documentation**: Unit tests describe what you think the code should do; they serve as another way to document your code. 

