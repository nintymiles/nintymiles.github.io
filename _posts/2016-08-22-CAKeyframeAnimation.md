##Waht is CAKeyframeAnimation?

CAKeyframeAnimation, however, is considerably more powerful and has no equivalent interface exposed in UIKit. CAKeyframeAnimation is, like CABasicAnimation, a subclass of CAPropertyAnimation. It still operates on a single property, but unlike CABasicAnimation it is not limited to just a single start and end value, and instead can be given an **arbitrary** sequence of values to animate between.




```
- (IBAction)changeColor {
```
