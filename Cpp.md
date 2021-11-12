1. NULL and nullptr
- in c: 
```c
#define NULL ((void*)0)
// int *p = NULL;
// int *p = ((void*)0);  //right
```
- in c++:
```cpp
#define NULL 0
// int *p = ((void*)0);  //wrong，不允许隐式转换，所以NULL变更为上述方式
```
- nullptr并非整形类型，甚至也不是指针类型，但是能转换为任意指针类型，nullptr的实际类型是std::nullptr_t

