---
layout: post
title:  Learning Notes - TTF字体轮廓矢量路径相关
---
## TTF字体轮廓
iOS中可以使用函数从TTF字体中获取单个字体的轮廓（glyph）path，通过技术手段，可以从path中获取具体的每个点，但是每个笔画的path不是分离的，整个字体是一个单一的path，需要通过各种方式分离出笔画路径。

```
+(NSArray *)getTTFPointsWithShapeLayers:(NSArray *)shapeLyaers{
    NSMutableArray *bezierPoints = [NSMutableArray array];
    
    for(CAShapeLayer *shapeLayer in shapeLyaers){
        NSMutableArray *pathPoints = [NSMutableArray array];
        
        CGPathApply(shapeLayer.path, (__bridge void * _Nullable)(pathPoints), MyCGPathApplierFunc);
        
        [bezierPoints addObjectsFromArray:pathPoints];
    }
    
    return bezierPoints;
}

//从CGPath还原组成的各个点
void MyCGPathApplierFunc (void *info, const CGPathElement *element) {
    NSMutableArray *bezierPoints = (__bridge NSMutableArray *)info;
    
    CGPoint *points = element->points;
    CGPathElementType type = element->type;
    
    switch(type) {
        case kCGPathElementMoveToPoint: // contains 1 point
            [bezierPoints addObject:[NSValue valueWithCGPoint:points[0]]];
            break;
            
        case kCGPathElementAddLineToPoint: // contains 1 point
            [bezierPoints addObject:[NSValue valueWithCGPoint:points[0]]];
            break;
            
        case kCGPathElementAddQuadCurveToPoint: // contains 2 points
            [bezierPoints addObject:[NSValue valueWithCGPoint:points[0]]];
            [bezierPoints addObject:[NSValue valueWithCGPoint:points[1]]];
            break;
            
        case kCGPathElementAddCurveToPoint: // contains 3 points
            [bezierPoints addObject:[NSValue valueWithCGPoint:points[0]]];
            [bezierPoints addObject:[NSValue valueWithCGPoint:points[1]]];
            [bezierPoints addObject:[NSValue valueWithCGPoint:points[2]]];
            break;
            
        case kCGPathElementCloseSubpath: // contains no point
            break;
    }
} 

```


