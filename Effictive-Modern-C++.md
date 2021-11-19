### nullptr
```cpp
#ifndef __cplusplus
#define NULL ((void*)0)
#else
#define NULL 0
#endif

// c语言中将NULL定义为空指针，c++定义为0，这是因为c++是强类型的，void*不能隐式转换为其他指针类型
void f(int);
void f(bool);
void f(void*);

f(0);
f(NULL); // g++ v4.8.5中，编译失败，NULL类型为long，long转换为int，long转换为bool，0L转换为void*都同样好

f(nullptr); // c++11

```
- nullptr可以认为是所有类型的指针，真正的类型是std::nullptr_t，在一个完美的循环定义后，std::nullptr_t又被定义为nullptr

- 优先考虑nullptr而非0和NULL
- 避免重载指针和整型
- NULL没有规定具体实现，可能为0，或者0L
