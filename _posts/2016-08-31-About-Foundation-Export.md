---
layout: post
title:  Learning Notes - Inside FOUNDATION_EXPORT
---
## Inside FOUNDATION_EXPORT
If you look in NSObjCRuntime.h (in Foundation) you will see that FOUNDATION_EXPORT compiles to extern in C, extern "C" in C++, and other things in Win32. So, it's a bit more compatible. For most projects, this won't make any difference.

`FOUNDATION_EXPORT ==== extern` in Objective-C

```
#if defined(__cplusplus)
#define FOUNDATION_EXTERN extern "C"
#else
#define FOUNDATION_EXTERN extern
#endif

#if TARGET_OS_WIN32

    #if defined(NSBUILDINGFOUNDATION)
        #define FOUNDATION_EXPORT FOUNDATION_EXTERN __declspec(dllexport)
    #else
        #define FOUNDATION_EXPORT FOUNDATION_EXTERN __declspec(dllimport)
    #endif

    #define FOUNDATION_IMPORT FOUNDATION_EXTERN __declspec(dllimport)

#else
    #define FOUNDATION_EXPORT  FOUNDATION_EXTERN
    #define FOUNDATION_IMPORT FOUNDATION_EXTERN
#endif

#if !defined(NS_INLINE)
    #if defined(__GNUC__)
        #define NS_INLINE static __inline__ __attribute__((always_inline))
    #elif defined(__MWERKS__) || defined(__cplusplus)
        #define NS_INLINE static inline
    #elif defined(_MSC_VER)
        #define NS_INLINE static __inline
    #elif TARGET_OS_WIN32
        #define NS_INLINE static __inline__
    #endif
#endif 
```

