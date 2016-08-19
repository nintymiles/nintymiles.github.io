## Objective-C __unused __used 等关键字
- __unused 表示变量和函数可能不会被用到，阻止编译器对遍变量和函数等没有使用而发出的警告。
- __used 表示强制变量和函数要被包含进来，即便在编译器看来他们没有被使用（并且就这样被弃置）

	> __used forces variables and functions to be included even if it appears to the compiler that they are not used (and would thust be discarded). （后半句是难懂的English）
- __deprecated 表示当使用废弃功能时，会导致编译器产生警告信息。

```
/* __unused denotes variables and functions that may not be used, preventing
 * the compiler from warning about it if not used.
 */
#define __unused	__attribute__((unused))

/* __used forces variables and functions to be included even if it appears
 * to the compiler that they are not used (and would thust be discarded).
 */
#define __used		__attribute__((used))

/* __deprecated causes the compiler to produce a warning when encountering
 * code using the deprecated functionality.
 * __deprecated_msg() does the same, and compilers that support it will print
 * a message along with the deprecation warning.
 * This may require turning on such warning with the -Wdeprecated flag.
 * __deprecated_enum_msg() should be used on enums, and compilers that support
 * it will print the deprecation warning.
 */
#define __deprecated	__attribute__((deprecated)) 
```

