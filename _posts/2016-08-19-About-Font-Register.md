---
layout: post
title:  Notes - About Fonts Register
---

## iOS Font 注册的一些过程
ttf等正常字体的注册过程

```
+ (BOOL)registerFontWithURL:(NSURL *)url
{
    NSAssert([[NSFileManager defaultManager] fileExistsAtPath:[url path]], @"Font file doesn't exist");
    
    CFErrorRef cfError = NULL;
    BOOL success = CTFontManagerRegisterFontsForURL((__bridge CFURLRef)url, kCTFontManagerScopeNone, &cfError);
    if (!success) {
        NSError *error = (__bridge_transfer NSError*)cfError;
        NSLog(@"%@", [error localizedDescription]);
    }
    
    return success;
    
}

```

涉及iconfont等的字体注册过程

```
- (id)initWithFontName:(NSString *)aName ofType:(NSString *)aType {
    self = [super init];
    if (self) {
        NSString *fontPath = [[NSBundle mainBundle] pathForResource:aName ofType:aType];
        NSString *plistPath = [[NSBundle mainBundle] pathForResource:aName ofType:@"plist"];
        
        //NSURL * url = [[NSBundle mainBundle] URLForResource:aName withExtension:aType];
		//CGDataProviderRef fontProvider = CGDataProviderCreateWithURL((__bridge CFURLRef)url);
        
        NSData *data = [[NSData alloc] initWithContentsOfFile:fontPath];
        CGDataProviderRef fontProvider = CGDataProviderCreateWithCFData((__bridge CFDataRef)data);
        
        _glyphLookup = [[NSDictionary alloc] initWithContentsOfFile: plistPath];
        NSAssert(_glyphLookup!=nil, @"plist for glyph font %@ not found", aName);
        
        CGFontRef cgFont = CGFontCreateWithDataProvider(fontProvider);
        CGDataProviderRelease(fontProvider);

        
        //图形字体注册使用的方法
        /* register to use as uifont
		CFErrorRef error;
        CTFontManagerRegisterGraphicsFont(cgFont, &error);
         */
        
        CTFontDescriptorRef fontDescriptor = CTFontDescriptorCreateWithAttributes((__bridge CFDictionaryRef)@{});
        _font = CTFontCreateWithGraphicsFont(cgFont, 0, NULL, fontDescriptor);
        CFRelease(fontDescriptor);
        CGFontRelease(cgFont);
        
        self.baseline = 0.2;
        
        //self.insets = UIEdgeInsetsMake(8, 8, 8, 8);
        

    }
    return self;
} 
```


```
enum {
   kCTFontManagerScopeNone = 0,
   kCTFontManagerScopeProcess = 1,
   kCTFontManagerScopeUser = 2,
   kCTFontManagerScopeSession = 3
};
typedef uint32_t CTFontManagerScope;

Constants
kCTFontManagerScopeNone
No scope is defined.

kCTFontManagerScopeProcess
The font is available to the current process for the duration of the process unless directly unregistered.

kCTFontManagerScopeUser
The font is available to all processes for the current user session and will be available in subsequent sessions unless unregistered.

kCTFontManagerScopeSession
The font is available to the current user session but will not be available in subsequent sessions.

```


> 以下为字体注册及字体列表等相关代码

精简加载自定义字体

核心源码:

UIFont+WDCustomLoader.m 与 UIFont+WDCustomLoader.h


```
//
//  UIFont+WDCustomLoader.h
//
//  Created by Walter Da Col on 10/17/13.
//  Copyright (c) 2013 Walter Da Col (walter.dacol<at>gmail.com)
//

#import <UIKit/UIKit.h>

/**
 You can use `UIFont+WDCustomLoader` category to load custom fonts for your
 application without worring about plist or real font names.
 */
@interface UIFont (WDCustomLoader)

/// @name Implicit registration and font loading

/**
 Get `UIFont` object for the selected font file.
 
 This method calls `+customFontWithURL:size`.
 
 @deprecated
 @see +customFontWithURL:size: method
 @param size Font size
 @param name Font filename without extension
 @param extension Font filename extension (@"ttf" and @"otf" are supported)
 @return `UIFont` object or `nil` on errors
 */
+ (UIFont *) customFontOfSize:(CGFloat)size withName:(NSString *)name withExtension:(NSString *)extension;

/**
 Get `UIFont` object for the selected font file (*.ttf or *.otf files).
 
 The first call of this method will register the font using 
 `+registerFontFromURL:` method.
 
 @see +registerFontFromURL: method
 @param fontURL Font file absolute url
 @param size Font size
 @return `UIFont` object or `nil` on errors
 */
+ (UIFont *) customFontWithURL:(NSURL *)fontURL size:(CGFloat)size;

/// @name Explicit registration

/**
 Allow custom fonts registration.
 
 With this method you can load all supported font file: ttf, otf, ttc and otc.
 Font that are already registered, with this library or by system, will not be 
 registered and you will see a warning log.
 
 @param fontURL Font file absolute url
 @return An array of postscript name which represent the file's font(s) or `nil`
 on errors. (With iOS < 7 as target you will see an empty array for collections)
 */
+ (NSArray *) registerFontFromURL:(NSURL *)fontURL;



@end
```


```
//
//  UIFont+WDCustomLoader.m
//
//  Created by Walter Da Col on 10/17/13.
//  Copyright (c) 2013 Walter Da Col (walter.dacol<at>gmail.com)
//

#import "UIFont+WDCustomLoader.h"
#import <CoreText/CoreText.h>

// Feature and deployment target check
#if  ! __has_feature(objc_arc)
#error This file must be compiled with ARC.
#endif

#if __IPHONE_OS_VERSION_MIN_REQUIRED < 40100
#error This file must be compiled with Deployment Target greater or equal to 4.1
#endif

// Activate Xcode only logging
#ifdef DEBUG
#define UIFontWDCustomLoaderDLog(fmt, ...) NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__);
#else
#define UIFontWDCustomLoaderDLog(...)
#endif

@implementation UIFont (Custom)
static CGFloat const kSizePlaceholder = 1.0f;
static NSMutableDictionary *appRegisteredCustomFonts = nil;

/**
 Check features for full font collections support
 
 @return YES if all features are supported
 */
+ (BOOL) deviceHasFullSupportForFontCollections {
    
    return (CTFontManagerCreateFontDescriptorsFromURL != NULL); // 10.6 or 7.0
    
}

/**
 Inner method for font(s) registration from a file
 
 @param fontURL A font URL
 
 @return Registration result
 */
+ (BOOL) registerFromURL:(NSURL *)fontURL {
    
    CFErrorRef error;
    BOOL registrationResult = YES;
    
    registrationResult = CTFontManagerRegisterFontsForURL((__bridge CFURLRef)fontURL, kCTFontManagerScopeProcess, &error);
    
    if (!registrationResult) {
        UIFontWDCustomLoaderDLog(@"Error with font registration: %@", error);
        CFRelease(error);
        return NO;
    }
    
    return YES;
}

/**
 Inner method for font registration from a graphic font.
 
 @param fontRef A CGFontRef
 
 @return Registration result
 */
+ (BOOL) registerFromCGFont:(CGFontRef)fontRef {

    CFErrorRef error;
    BOOL registrationResult = YES;
    
    registrationResult = CTFontManagerRegisterGraphicsFont(fontRef, &error);
    
    if (!registrationResult) {
        UIFontWDCustomLoaderDLog(@"Error with font registration: %@", error);
        CFRelease(error);
        return NO;
    }
    
    return YES;
    
}

+ (NSArray *) registerFontFromURL:(NSURL *)fontURL {
    // Dictionary creation
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        appRegisteredCustomFonts = [NSMutableDictionary new];
    });
    
    // Result
    NSArray *fontPSNames = nil;
    
    
    // Critical section
    @synchronized(appRegisteredCustomFonts) {
        
        // Check if this library knows this url
        fontPSNames = [[appRegisteredCustomFonts objectForKey:fontURL] copy];
        
        if (fontPSNames == nil) {
            
            // Check features
            if ([UIFont deviceHasFullSupportForFontCollections]) {
                
                // Retrieve font descriptors from ttf, otf, ttc and otc files
                NSArray *fontDescriptors = (__bridge_transfer NSArray *)(CTFontManagerCreateFontDescriptorsFromURL((__bridge CFURLRef)fontURL));
                
                // Check errors
                if (fontDescriptors) {
                    
                    // Check how many fonts are already registered (or have the
                    // same name of another font)
                    NSMutableArray *verifiedFontPSNames = [NSMutableArray new];
                    
                    for (NSDictionary *fontDescriptor in fontDescriptors) {
                        NSString *fontPSName = [fontDescriptor objectForKey:@"NSFontNameAttribute"];
                        
                        if (fontPSName) {
                            if ([UIFont fontWithName:fontPSName size:kSizePlaceholder]) {
                                UIFontWDCustomLoaderDLog(@"Warning with font registration: Font '%@' already registered",fontPSName);
                            }
                            [verifiedFontPSNames addObject:fontPSName];
                        }
                    }
                    
                    fontPSNames = [NSArray arrayWithArray:verifiedFontPSNames];
                    
                    // At least one
                    if ([fontPSNames count] > 0) {
                        
                        // If registration went ok
                        if ([UIFont registerFromURL:fontURL]) {
                            // Add url to this library
                            [appRegisteredCustomFonts setObject:fontPSNames
                                                         forKey:fontURL];
                            
                        } else {
                            fontPSNames = nil;
                        }
                        
                    } else { // [fontPSNames count] <= 0
                        UIFontWDCustomLoaderDLog(@"Warning with font registration: All fonts in '%@' are already registered", fontURL);
                    }
                    
                } else { // CTFontManagerCreateFontDescriptorsFromURL fail
                    UIFontWDCustomLoaderDLog(@"Error with font registration: File '%@' is not a Font", fontURL);
                    fontPSNames = nil;
                }
            } else { // [UIFont deviceHasFullSupportForFontCollections] fail
                
                // Read data
                NSError *error;
                NSData *fontData = [NSData dataWithContentsOfURL:fontURL
                                                         options:NSDataReadingUncached
                                                           error:&error];
                
                // Check data creation
                if (fontData) {
                    
                    // Load font
                    CGDataProviderRef fontDataProvider = CGDataProviderCreateWithCFData((CFDataRef)fontData);
                    CGFontRef loadedFont = CGFontCreateWithDataProvider(fontDataProvider);
                    
                    // Check font
                    if (loadedFont != NULL) {
                        
                        // Prior to iOS7 is not easy to retrieve names from font collections
                        // But is possible to register collections
                        NSSet *singleFontValidExtensions = [NSSet setWithArray:@[@"ttf", @"otf"]];
                        
                        if ([singleFontValidExtensions containsObject:[fontURL pathExtension]]) {
                            // Read name
                            fontPSNames = @[(__bridge_transfer NSString *)(CGFontCopyPostScriptName(loadedFont))];
                            
                            // Check if registration is required
                            if ([UIFont fontWithName:fontPSNames[0] size:kSizePlaceholder] == nil) {
                                
                                // If registration went ok
                                if ([UIFont registerFromCGFont:loadedFont]) {
                                    // Add url to this library
                                    [appRegisteredCustomFonts setObject:fontPSNames
                                                                 forKey:fontURL];
                                    
                                } else {
                                    fontPSNames = nil;
                                }
                            } else {
                                UIFontWDCustomLoaderDLog(@"Warning with font registration: All fonts in '%@' are already registered", fontURL);
                            }
                            
                        } else {
                            // Is a collection
                            
                            //TODO find a way to read names
                            fontPSNames = @[];
                            
                            // Revert to url registration which allow collections
                            // If registration went ok
                            if ([UIFont registerFromURL:fontURL]) {
                                // Add url to this library
                                [appRegisteredCustomFonts setObject:fontPSNames
                                                             forKey:fontURL];
                                
                            } else {
                                fontPSNames = nil;
                            }
                        }
                        
                    } else { // CGFontCreateWithDataProvider fail
                        UIFontWDCustomLoaderDLog(@"Error with font registration: File '%@' is not a Font", fontURL);
                        fontPSNames = nil;
                    }
                    
                    // Release
                    CGFontRelease(loadedFont);
                    CGDataProviderRelease(fontDataProvider);
                } else {
                    UIFontWDCustomLoaderDLog(@"Error with font registration: URL '%@' cannot be read with error: %@", fontURL, error);
                    fontPSNames = nil;
                }

            }
        
        }
        
    }

    return fontPSNames;
}

+ (UIFont *) customFontWithURL:(NSURL *)fontURL size:(CGFloat)size {
    
    // Only single font with this method
    NSSet *singleFontValidExtensions = [NSSet setWithArray:@[@"ttf", @"otf"]];
    
    if (![singleFontValidExtensions containsObject:[fontURL pathExtension]]) {
        UIFontWDCustomLoaderDLog(@"Only ttf or otf files are supported by this method");
        return nil;
    }
    
    NSArray *fontPSNames = [UIFont registerFontFromURL:fontURL];

    if (fontPSNames == nil) {
        UIFontWDCustomLoaderDLog(@"Invalid Font URL: %@", fontURL);
        return nil;
    }
    if ([fontPSNames count] != 1) {
        UIFontWDCustomLoaderDLog(@"Font collections not supported by this method");
        return nil;
    }
    return [UIFont fontWithName:fontPSNames[0] size:size];
}

+ (UIFont *) customFontOfSize:(CGFloat)size withName:(NSString *)name withExtension:(NSString *)extension {
    // Get url for font resource
    NSURL *fontURL = [[[NSBundle mainBundle] URLForResource:name withExtension:extension] absoluteURL];
    
    return [UIFont customFontWithURL:fontURL size:size];
}

@end
```


以下源码都是在上述源码基础上封装的:

FontInfo.h 与 FontInfo.m

```
//
//  FontInfo.h
//  GCD
//
//  Created by YouXianMing on 14-9-21.
//  Copyright (c) 2014年 YouXianMing. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface FontInfo : NSObject

+ (NSDictionary *)customFontNameList;
+ (NSDictionary *)systomFontNameList;
+ (void)registerFont:(NSString *)fontPath withName:(NSString *)name;

@end
```


```
//
//  FontInfo.m
//  GCD
//
//  Created by YouXianMing on 14-9-21.
//  Copyright (c) 2014年 YouXianMing. All rights reserved.
//

#import "FontInfo.h"
#import "UIFont+WDCustomLoader.h"

static NSMutableDictionary   *customFontDictionary = nil; // 自己加载的字体信息
static NSMutableDictionary   *systemFontDictionary = nil; // 系统字体信息

@implementation FontInfo

+ (void)initialize
{
    if (self == [FontInfo class])
    {
        customFontDictionary = [[NSMutableDictionary alloc] init];
        systemFontDictionary = [[NSMutableDictionary alloc] init];
        
        // 获取系统字体族
        [FontInfo getSystemFontList];
    }
}

+ (void)getSystemFontList
{
    NSArray *familyNames = [UIFont familyNames];
    for( NSString *familyName in familyNames)
    {
        NSArray *fontNames = [UIFont fontNamesForFamilyName:familyName];
        [systemFontDictionary setObject:fontNames forKey:familyName];
    }
}

+ (void)registerFont:(NSString *)fontPath withName:(NSString *)name
{
    NSArray *nameArray = [UIFont registerFontFromURL:[NSURL fileURLWithPath:fontPath]];
    [customFontDictionary setObject:nameArray forKey:name];
}

+ (NSDictionary *)customFontNameList
{
    return [NSDictionary dictionaryWithDictionary:customFontDictionary];
}

+ (NSDictionary *)systomFontNameList
{
    return [NSDictionary dictionaryWithDictionary:systemFontDictionary];
}

@end
```

UIFont+Custom.h 与 UIFont+Custom.m

```
//
//  UIFont+Custom.h
//  GCD
//
//  Created by YouXianMing on 14-9-21.
//  Copyright (c) 2014年 YouXianMing. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface UIFont (CustomFont)

+ (void)registerFontFamily:(NSString *)fontPath withName:(NSString *)name;
+ (NSDictionary *)systomAllFontsFamilyInfo;

@end
```


```
//
//  UIFont+Custom.m
//  GCD
//
//  Created by YouXianMing on 14-9-21.
//  Copyright (c) 2014年 YouXianMing. All rights reserved.
//

#import "UIFont+CustomFont.h"
#import "FontInfo.h"

#ifdef DEBUG
#define UIFontCustomFontDLog(fmt, ...) NSLog((@"%@[%d]:%s:" fmt),[[NSString stringWithUTF8String:__FILE__] lastPathComponent], __LINE__, __func__, ##__VA_ARGS__);
#else
#define UIFontCustomFontDLog(...)
#endif

@implementation UIFont (CustomFont)

+ (void)registerFontFamily:(NSString *)fontPath withName:(NSString *)name
{
    if (fontPath == nil) {
        UIFontCustomFontDLog(@"[警告]fontPath为空");
        return;
    }
    
    [FontInfo registerFont:fontPath withName:name];
}

+ (NSDictionary *)systomAllFontsFamilyInfo
{
    return [FontInfo systomFontNameList];
}

@end
```


NSString+CustomFont.h 与 NSString+CustomFont.m


```
//
//  NSString+CustomFont.h
//  GCD
//
//  Created by YouXianMing on 14-9-21.
//  Copyright (c) 2014年 YouXianMing. All rights reserved.
//

#import <Foundation/Foundation.h>

/*
 NSString值都为字体族名字
 */

/*
 经典字体家族
 "Helvetica Neue" =     (
 "HelveticaNeue-BoldItalic",
 "HelveticaNeue-Light",
 "HelveticaNeue-Italic",
 "HelveticaNeue-UltraLightItalic",
 "HelveticaNeue-CondensedBold",
 "HelveticaNeue-MediumItalic",
 "HelveticaNeue-Thin",
 "HelveticaNeue-Medium",
 "HelveticaNeue-ThinItalic",
 "HelveticaNeue-LightItalic",
 "HelveticaNeue-UltraLight",
 "HelveticaNeue-Bold",
 HelveticaNeue,
 "HelveticaNeue-CondensedBlack"
 );
 */

@interface NSString (CustomFont)

- (NSString *)customFontFamilyName;
- (NSString *)customFontFamilyNameAtIndex:(NSInteger)index;
- (NSArray *)customFontInfo;

- (NSString *)systemFontFamilyName;
- (NSString *)systemFontFamilyNameIndex:(NSInteger)index;
- (NSArray *)systemFontInfo;

@end
```


```
//
//  NSString+CustomFont.m
//  GCD
//
//  Created by YouXianMing on 14-9-21.
//  Copyright (c) 2014年 YouXianMing. All rights reserved.
//

#import "NSString+CustomFont.h"
#import "FontInfo.h"

@implementation NSString (CustomFont)

#pragma public
- (NSString *)customFontFamilyName
{
    return [self fontFamilyName:self customFontIndex:0];
}

- (NSString *)customFontFamilyNameAtIndex:(NSInteger)index
{
    return [self fontFamilyName:self customFontIndex:index];
}

- (NSArray *)customFontInfo
{
    NSDictionary *fontDic = [FontInfo customFontNameList];
    if (self) {
        return fontDic[self];
    } else {
        return nil;
    }
}

- (NSString *)systemFontFamilyName
{
    return [self fontFamilyName:self systemFontIndex:0];
}

- (NSString *)systemFontFamilyNameIndex:(NSInteger)index
{
    return [self fontFamilyName:self systemFontIndex:index];
}

- (NSArray *)systemFontInfo
{
    NSDictionary *fontDic = [FontInfo systomFontNameList];
    if (self) {
        return fontDic[self];
    } else {
        return nil;
    }
}

#pragma private
- (NSString *)fontFamilyName:(NSString *)fontName customFontIndex:(NSInteger)index
{
    if (fontName == nil) {
        return nil;
    }
    
    NSDictionary *fontDic = [FontInfo customFontNameList];
    NSArray *fontIndex = fontDic[fontName];
    
    if (fontIndex) {
        return fontIndex[index];
    } else {
        return nil;
    }
}

- (NSString *)fontFamilyName:(NSString *)fontName systemFontIndex:(NSInteger)index
{
    if (fontName == nil) {
        return nil;
    }
    
    NSDictionary *fontDic = [FontInfo systomFontNameList];
    NSArray *fontIndex = fontDic[fontName];
    
    if (fontIndex) {
        return fontIndex[index];
    } else {
        return nil;
    }
}

@end
```



以下是使用源码:


```
//
//  RootViewController.m
//  GCD
//
//  Created by YouXianMing on 14-9-21.
//  Copyright (c) 2014年 YouXianMing. All rights reserved.
//

#import "RootViewController.h"
#import "NSString+CustomFont.h"
#import "UIFont+CustomFont.h"

@interface RootViewController ()

@end

@implementation RootViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor blackColor];
    
    // 注册自定义字体
    [UIFont registerFontFamily:[@"新蒂小丸子小学版.ttf" bundleFile]
                      withName:@"新蒂小丸子小学版"];
    
    // 显示
    UILabel *label_1      = [[UILabel alloc] initWithFrame:CGRectMake(50, 50, 200, 40)];
    label_1.text          = @"游贤明";
    label_1.textColor     = [UIColor redColor];
    label_1.textAlignment = NSTextAlignmentCenter;
    label_1.font          = [UIFont fontWithName:[@"新蒂小丸子小学版" customFontFamilyName] size:30.f];
    [self.view addSubview:label_1];
    
    UILabel *label_2      = [[UILabel alloc] initWithFrame:CGRectMake(50, 100, 200, 40)];
    label_2.text          = @"YouXianMing";
    label_2.textColor     = [UIColor whiteColor];
    label_2.textAlignment = NSTextAlignmentCenter;
    label_2.font          = [UIFont fontWithName:[@"Helvetica Neue" systemFontFamilyNameIndex:1] size:18.f];
    [self.view addSubview:label_2];
    
    // 打印字体族信息
    NSLog(@"%@", [@"新蒂小丸子小学版" customFontInfo]);
    NSLog(@"%@", [@"Helvetica Neue" systemFontFamilyNameIndex:1]);
}

@end

```

