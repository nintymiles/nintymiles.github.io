---
layout: post
title:  Learning Notes - How to draw a Mi-Character Plaid Backgournd in view
---
## How to draw a Mi-Character Plaid Backgournd in view?
在View中实现drawRect，使用CoreGraphic绘制图形时，使用CGContext... 方法是正途。


```
@implementation MiDashLineBackgroundRenderView

// Only override drawRect: if you perform custom drawing.
// An empty implementation adversely affects performance during animation.
- (void)drawRect:(CGRect)rect {
    // Drawing code
    
    //更新后台颜色属于UI更新，需要在主线程中。在此处更新无法刷新后台意味着drawRect的执行不在主线程中。？？？
//    self.backgroundColor=[UIColor whiteColor];
    
    [self drawMiDashLineBackground];
}


- (void)drawMiDashLineBackground{
    self.layer.backgroundColor=[UIColor clearColor].CGColor;
    
    CGFloat maxX = CGRectGetMaxX(self.bounds);
    CGFloat maxY = CGRectGetMaxY(self.bounds);
    CGFloat minX = CGRectGetMinX(self.bounds);
    CGFloat minY = CGRectGetMinY(self.bounds);
    CGFloat centX = CGRectGetMidX(self.bounds);
    CGFloat centY = CGRectGetMidY(self.bounds);
    
//    UIBezierPath *path = [UIBezierPath bezierPath];
//    [path moveToPoint:CGPointMake(minX, minY)];
//    
//    [path addLineToPoint:CGPointMake(maxX, minY)];
    
    CGFloat thickness = 2.0;
    
    CGContextRef cx = UIGraphicsGetCurrentContext();
    CGContextSetLineWidth(cx, thickness);
    CGContextSetStrokeColorWithColor(cx, [UIColor redColor].CGColor);
    
    CGFloat ra_dash[] = {4,2};  //第二个数组位置参数表示空线长度
    CGContextSetLineDash(cx, minX, ra_dash, 2); //  "2" == ra count
    
    CGContextMoveToPoint(cx, minX,thickness*0.5);
    CGContextAddLineToPoint(cx, maxX, maxY);
    
    CGContextMoveToPoint(cx, minX, maxY);
    CGContextAddLineToPoint(cx, maxX, minY);
    
    CGContextMoveToPoint(cx, centX, minY);
    CGContextAddLineToPoint(cx, centX, maxY);
    
    CGContextMoveToPoint(cx, minX, centY);
    CGContextAddLineToPoint(cx, maxX, centY);
    
    CGContextStrokePath(cx);
    
    CGFloat ra_line[] = {1,0};
    CGContextSetLineDash(cx, minX, ra_line, 2); //第二个数组位置参数表示空线长度，若为0则表示没有空线

    CGContextMoveToPoint(cx, minX+thickness*0.5,minY+thickness*0.5);
    CGContextAddLineToPoint(cx, maxX-thickness*0.5, minY+thickness*0.5);
    CGContextAddLineToPoint(cx, maxX-thickness*0.5, maxY-thickness*0.5);
    CGContextAddLineToPoint(cx, minX+thickness*0.5, maxY-thickness*0.5);
    CGContextClosePath(cx);
    
    CGContextStrokePath(cx);
    CGContextSetFillColorWithColor(cx, [UIColor clearColor].CGColor);
    
    
}


@end
```

思路来自于下面的点线绘制实现。

```
@interface UIDashedLine : UIView
@end

// Extremely easy way to draw a dashed line:
// Simply place an empty UIView in the storyboard, where you want the line.
// Change the class of the UIView, to UIDashedLine.
// At runtime it will draw a perfect dashed line,
// wherever you placed the view in storyboard.

@implementation UIDashedLine

-(void)drawRect:(CGRect)rect
    {
    CGFloat thickness = 4.0;

    CGContextRef cx = UIGraphicsGetCurrentContext();
    CGContextSetLineWidth(cx, thickness);
    CGContextSetStrokeColorWithColor(cx, [UIColor blackColor].CGColor);

    CGFloat ra[] = {4,2};
    CGContextSetLineDash(cx, 0.0, ra, 2); // nb "2" == ra count

    CGContextMoveToPoint(cx, 0,thickness*0.5);
    CGContextAddLineToPoint(cx, self.bounds.size.width, thickness*0.5);
    CGContextStrokePath(cx);
    }

@end 
```

