```c
#include <stdio.h>
#include <stdarg.h>

/**
 * @brief 测试变长参数的使用
 * 
 * @param n 头参
 * @param ... 剩余参数，基本数据类型
 */
static void Mytest1(int n, ...) {
    va_list ap;

    va_start(ap, n);

    int a = va_arg(ap, int);

    unsigned b = va_arg(ap, unsigned);

    double d = va_arg(ap, double);

    va_end(ap);

    printf("result = %f\n", n + a + b + d);

}

/**
 * @brief 变长参数类型va_list作为形参的测试函数
 * 
 * @param n 头参
 * @param ap 变长参数列表
 * @return double 所有形参和
 */
static double MyFunc(int n, va_list ap) {
    int a = va_arg(ap, int);

    unsigned b = va_arg(ap, unsigned);

    double d = va_arg(ap, double);

    return n + a + b + d;
}

/**
 * @brief 测试变长形参函数2
 * 
 * @param n 头参
 * @param ... 变长参数，传入的变长形参列表作为MyFunc的变长实参
 */
static void MyTest2(int n, ...) {
    va_list ap;

    va_start(ap, n);

    double result = MyFunc(n, ap);

    va_end(ap);

    printf("result = %f\n", result);
}

struct MyStruct {
    int a;
    int b;
};

union MyUnion {
    char c;
    short s;
};

/**
 * @brief 测试变长参数的函数3
 * 
 * @param a 头参
 * @param ... 变长参数，复杂数据类型 
 */
static void MyTest3(int a, ...) {
    va_list ap;

    va_start(ap, a);

    struct MyStruct s = va_arg(ap, struct MyStruct);

    union MyUnion un = va_arg(ap, union MyUnion);

    printf("size of un: %zu\n",sizeof(un));

    va_end(ap);

    int result = a + s.a + s.b - un.s;

    printf("result = %d\n", result);
}

int main(int argc, const char** argv) {
    __int8_t a = 10;

    __uint16_t b = 20;

    Mytest1(3, a, b, 10.5f);

    MyTest2(5, a, b, 10.5f);

    MyTest3(10, (struct MyStruct) {1, 2}, (union MyUnion) {.s = 3});

    return 0;
}
```

