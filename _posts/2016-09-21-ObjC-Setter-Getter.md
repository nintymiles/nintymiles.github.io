---
layout: post
title:  Learning Notes - ObjC属性以及初始化时机
---
##ViewController中的属性以及如何初始化
**所有的属性都使用getter和setter**

不要在viewDidLoad里面初始化你的view然后再add，这样代码就很难看。viewDidload里面只做addSubview的事情，然后在viewWillAppear里面做布局的事情（改变位置可以放在viewWilllayoutSubview或者didLayoutSubview里），最后在viewDidAppear里面做Notification的监听之类的事情。至于属性的**初始化**，则**交给getter**去做。 


早在2003年，Allen Holub就发了篇文章《Why getter and setter methods are evil》，自此之后，业界就对此产生了各种争议，虽然是从Java开始说的，但是发展到后面各种语言也参与了进来。然后虽然现在关于这个问题讨论得少了，但是依旧属于没有定论的状态。setter的情况比较复杂，也不是我这一节的重点，我这边还是主要说getter。我们从objc的设计来看，苹果的设计者更加倾向于getter is not evil。

认为getter is evil的原因有非常之多，或大或小，随着争论的进行，大家慢慢就聚焦到这样的一个原因：Getter和Setter提供了一个能让外部修改对象内部数据的方式，这是evil的，正常情况下，一个对象自己私有的变量应该是只有自己关心。

然后我们回到iOS领域来，objc也同样面临了这样的问题，甚至更加严重：objc并没有像Java那么严格的私有概念。但在实际工作中，我们不太会去操作头文件里面没有的变量，这是从规范上就被禁止的。

认为getter is not evil的原因也可以聚焦到一个：高度的封装性。getter事实上是工厂方法，有了getter之后，业务逻辑可以更加专注于调用，而不必担心当前变量是否可用。我们可以想一下，假设一个ViewController有20个subview要加入view中，这20个subview的初始化代码是肯定逃不掉的，放在哪里比较好？放在哪里都比放在addsubview的地方好，我个人认为最好的地方还是放在getter里面，结合单例模式之后，代码会非常整齐，生产的地方和使用的地方得到了很好的区分。

所以放到iOS来说，我还是觉得使用getter会比较好，因为evil的地方在iOS这边基本都避免了，not evil的地方都能享受到，还是不错的。

