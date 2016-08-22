##Waht is CAKeyframeAnimation?

CAKeyframeAnimation, however, is considerably more powerful and has no equivalent interface exposed in UIKit. CAKeyframeAnimation is, like CABasicAnimation, a subclass of CAPropertyAnimation. It still operates on a single property, but unlike CABasicAnimation it is not limited to just a single start and end value, and instead can be given an **arbitrary** sequence of values to animate between.
The term keyframe **originates** from traditional animation, where a **lead**(主要的，头条的) animator would draw only the frames where something significant happens (*the key frames*), and then the less highly skilled artists would draw the frames in between (which could be easily inferred from the keyframes). The same principle applies with CAKeyframeAnimation: You provide the significant frames, and Core Animation fills in the gaps using a process called interpolation（插入，插值）.
We can demonstrate this using our colored layer from the earlier example. We’ll set up an array of colors and play them back with a single command using a keyframe animation (see Listing 8.5).
Listing 8.5 Applying a Sequence of Colors Using CAKeyframeAnimation

```
- (IBAction)changeColor {//create a keyframe animationCAKeyframeAnimation *animation = [CAKeyframeAnimation animation]; animation.keyPath = @"backgroundColor";animation.duration = 2.0;animation.values = @[(__bridge id)[UIColor blueColor].CGColor, (__bridge id)[UIColor redColor].CGColor, (__bridge id)[UIColor greenColor].CGColor, (__bridge id)[UIColor blueColor].CGColor ];//apply animation to layer[self.colorLayer addAnimation:animation forKey:nil]; } 
```

