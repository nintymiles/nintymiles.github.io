---
layout: post
title:  Learning Notes - Core Animation Concepts
---

## Some important concepts
- CGImageRef is **a pointer** to a CGImage **struct**. UIImage has a CGImage property that returns the underlying CGImageRef
- Why do we need a **bridge**?

    Although **Core Foundation types behave like Cocoa objects at runtime** (known as toll-free bridging), they are **not type compatible with id** unless you use ***a bridged cast***. To assign the image of a layer, you actually need to do the following:

```
layer.contents = (__bridge id)image.CGImage;
```
- what is contentsGravity?
    
    The visual properties of UIView— such as contentMode,The equivalent property of CALayer is called contentsGravity, and it is an NSString rather than an enum like its UIKit counterpart. The contentsGravity string should be set to one of the following constant values:

列1 | 列2  
---|---
kCAGravityCenter | kCAGravityTopRight
kCAGravityTop | kCAGravityBottomLeft
kCAGravityBottom | kCAGravityBottomRight
kCAGravityLeft | kCAGravityResize
kCAGravityRight | kCAGravityResizeAspect
kCAGravityTopLeft | kCAGravityResizeAspectFill

```
self.layerView.layer.contentsGravity = kCAGravityResizeAspect;
```
- what is contentsScale?

    If contentsScale is set to 1.0, drawing will be done at a resolution of 1 pixel per point. If it is set to 2.0, drawing will be done at 2 pixels per point, a.k.a. Retina resolution.
    
    This doesn’t actually make any difference when using kCAGravityResizeAspect because it scales the image to fit the layer, regardless of its resolution. But if we switch our contentsGravity to **kCAGravityCenter** instead (*which doesn’t scale the image*), the difference will be more apparent.
    
- what is contentsRect?

    The contentsRect property of CALayer allows us to specify a **subrectangle** of the backing image to be displayed inside the layer frame. This allows for much greater flexibility than the contentsGravity property in terms of how the image is cropped and stretched.
    
- what is contentCenter?

    The name is misleading,The contentsCenter is actually a CGRect that defines a stretchable region inside the layer and a fixed border around the edge. Changing the contentsCenter makes no difference to how the backing image is displayed, until the layer is resized, and then its purpose becomes clear.
    
    By default, the contentsCenter is set to {0, 0, 1, 1}, which means that the backing image will stretch uniformly when the layer is resized (depending on the contentsGravity). But if we increase the origin values and reduce the size, we can create a border around the image. 
    
- Custom Drawing

    Setting the layer contents with a CGImage is not the only way to populate the backing image. It is also possible to draw directly into the backing image using Core Graphics. The -drawRect: method can be implemented in a UIView subclass to implement **custom drawing**.
    
    The -drawRect: method has* no default implementation* because a UIView does not require a custom backing image if it is just filled with a solid color or if the underlying layer’s contents property contains an existing image instance. If UIView detects that
the -drawRect: method is present, it allocates a new backing image for the view, with pixel dimensions equal to the view size multiplied by the contentsScale.

    **The -drawRect: method is executed automatically when the view first appears onscreen.**
    > Apple recommends that you don’t leave an empty -drawRect: method in your layer subclasses if you don’t intend to do any custom drawing.


## iOS coordinate types 
- **Points**—The *most commonly* used coordinate type on iOS and Mac OS. Points are **virtual pixels**, also known as **logical pixels**. On standard-definition devices, 1 point equates to 1 pixel, but on Retina devices, a point equates to 2×2 physical pixels. iOS uses points for all screen coordinate measurements so that layouts work seamlessly on both Retina and non-Retina devices.
- **Pixels**—*Physical pixel coordinates* are ***not used for screen layout***, but they are often still relevant when working with images. UIImage is screen-resolution aware, and specifies its size in points, but some lower-level image representations such as **CGImage use pixel dimensions**, so you should keep in mind that their stated size will not match their display size on a Retina device.
- **Unit**—Unit coordinates are a convenient way to specify measurements that are relative to the size of an image or a layer’s bounds, and so do not need to be adjusted if that size changes. Unit coordinates are used a lot in OpenGL for things like texture coordinates, and they are also used frequently in Core Animation.

## Coordinate systems
Layers, like views, are positioned hierarchically, with each placed relative to its parent in the layer tree. **The position of a layer is relative to the bounds of its superlayer**. If the superlayer moves, so do all of its sublayers.

Sometimes you need to know the absolute position of a layer or (more commonly) its position relative to a layer other than its immediate parent.
CALayer provides some utility methods for converting between different layers’ coordinate systems:
-  \- (CGPoint)convertPoint:(CGPoint)point fromLayer:(CALayer *)layer; 
-  \- (CGPoint)convertPoint:(CGPoint)point toLayer:(CALayer *)layer; 
-  \- (CGRect)convertRect:(CGRect)rect fromLayer:(CALayer *)layer;
-  \- (CGRect)convertRect:(CGRect)rect toLayer:(CALayer *)layer;

## Flipped Geometry
Conventionally, on iOS the position of a layer is specified relative to the **top-left** corner of its superlayer’s bounds. On Mac OS, the convention is that the position is relative to
the **bottom-left** corner. Core Animation can support **either** of these conventions by virtue of the **geometryFlipped** property. This is a BOOL value that determines whether the geometry of a layer is **vertically flipped** with respect to its superlayer. 

## The Z Axis
Unlike UIView,which is strictly two-dimensional,CALayer exists in three-dimensional space,CALayer also has two additional properties,zPosition and anchorPointZ,both of which are floating-point values describing the layer's postion on the Z axis.