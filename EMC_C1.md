# template type deduction, auto independent type deduction, decltype works with rules made by itself
## template type deduction
```cpp
template<typename T>
void f(ParamType param);

//invoke
f(expr);

//compiler uses expr to deduce the type of T and ParamType
//they usually different cause ParamType contains const or reference
template<typename T>
void f(const T& param); 
int x = 0;
f(x); // T is int, ParamType is const int&
//期望T的类型与expr的类型相同
//T的类型推导不仅取决于expr，也取决于ParamType的类型
```
## 三种情况
### ParamType是一个指针或引用，但不是通用引用
- 推导规则
  1. 如果expr的类型是一个引用，忽略引用部分
  2. 然后expr的类型与ParamType进行模式匹配来决定T
```cpp
//case 1
template<typename T>
void f(T& param); 
int x = 28;
const int cx = x;
const int& rx = x;
f(x);   //T is int, ParamType is const int&
f(cx);  //T is const int, ParamType is const int&
f(rx);  //T is const int, ParamType is const int&


//case 2
template<typename T>
void f(const T& param);
int x = 28;
const int cx = x;
const int& rx = x;
f(x);   //T is int, ParamType is const int&
f(cx);  //T is int, ParamType is const int&
f(rx);  //T is int, ParamType is const int&


//case 3
template<typename T>
void f(T* param);
int x = 27;
const int *px = &x;
f(&x);  //T is int, ParamType is int*
f(px);  //T is const int, ParamType is const int*
```
### ParamType是一个通用引用
- 推导规则
  1. 如果expr是左值，T和ParamType都会被推导为左值引用。这是模板推导类型中唯一一种T被推导为引用的情况
  2. 如果expr是右值，使用正常（第一种情况）的推导规则
```cpp
//case 1
template<typename T>
void f(T&& param);
int x = 27;
const int cx = x;
const int &rx = cx;
f(x);   //T is int&, ParamType is int&
f(cx);  //T is const int&, ParamType is const int&
f(rx);  //T is const int&, ParamType is const int&
f(27);  //T is int, ParamType is int&&
```
### ParamType既不是指针也不是引用
- 推导规则
  1. 如果expr是一个引用，忽略这个引用部分
  2. 如果忽略引用性之后，expr是一个const，再忽略const，如果是volatile，忽略volatile
```cpp
template<typename T>
void f(T param);
int x = 27;
const int cx = x;
const int &rx = cx;
f(x);   //T is int, ParamType is int
f(cx);  //T is int, ParamType is int
f(rx);  //T is int, ParamType is int
```

### 数组实参
```cpp
// 数组类型不同于指针类型，有时候可以互换
const char name[] = "YLW";
const char *p_name = name;  //数组退化为指针
template<typename T>
void f(T param);
f(name);    // ？？？

//以下两个函数等价
void mf(int param[]); 
void mf(int *param);

//数组形参会被视作指针形参，传值给模板的一个数组类型会被推导为一个指针类型
f(name);    //T被推导为const char*

template<typename T>
void f(T& param);
f(name);    //T被推导为const char[13], ParamType is const char (&) [13]

//可声明指向数组的引用的能力，使得我们可以创建一个模板函数来推导数组的大小
template<typename T, std::size_t N>
constexpr std::size_t arraySize(T (&t) [N]) noexcept{
    return N;
}
```
### 函数实参
```cpp
//不只是数组会退化为指针，函数类型也会退化为一个函数指针
void sf(int, double);
template<typename T>
void f1(T param);

template<typename T>
void f2(T &param);

f1(sf); //param被推导为指向函数的指针：void(*)(int, double)
f2(sf); //param被推导为指向函数的引用：void(&)(int, double)
```

## 总结
- 模板类型推导时，有引用的实参会被视为无引用，它们的引用会被忽略
- 对于通用引用的推导，左值实参会被特殊对待
- 对于传值类型推导，const，volatile会被忽略
- 在模板类型推导时，数组名或者函数名会退化为指针，除非它们被用于初始化引用




## auto type deduction
### auto类型推导与模板类型推导有一个直接的映射关系
```cpp
auto x = 27;        //x is int
const auto cx = x;  //cx is const int
const auto &rx = cx;//rx is const int&

auto&& uref1 = x;   //uref1 is int&
auto&& uref2 = cx;  //uref2 is const int&
auto&& uref3 = 27;  //uref3 is int&&

const char name[] = "R. N. Briggs";
auto arr1 = name;   //arr1 is const char*
auto& arr2 = name;  //arr2 is const char(&)[13]


void someFunc(int, double);
auto func1 = someFunc;  //func1 is void(*)(int, double)
auto& func2 = someFunc; //func2 is void(&)(int, double)
```

### auto与模板推导不一致的地方
```cpp
//c++98
int x1 = 27;
int x2(27);

//c++11 add: uniform initialization
int x3 = {27};
int x4{27};

auto x1 = 27;   //x1 is int
auto x2(27);    //x2 is int
auto x3 = {27}; //x3 is std::initializer_list<int>
auto x4{27};    //x4 is std::initializer_list<int>
auto x5 = {1, 2, 3.0};  //ERROR! 
/*step: 
    1. 由于auto的使用x5会被推导
    2. x5使用花括号的方式进行初始化，x5必须被推导为std::initializer_list
    3. initializer_list是一个模板，initializer_list<T>中的T也会被推导
*/
```
- 对于花括号的处理是auto类型推导与模板类型推导唯一不同的地方
- 真正区别在于：auto类型推导假定花括号表示std::initializer_list而模板类型推导不会（不知道如何处理）
```cpp
auto x = {11, 23, 8};   //x is std::initializer_list<int>

template<typename T>
void f(T param);

f({11, 23, 8}); //ERROR! 不能推导出T

template<typename T>
void f(std::initializer_list<T> initList);
f({11, 23, 9});     //T is int, initList is std::initializer_list<int>

```
- c++14允许auto用于函数返回值并会被推导，而且c++14的lambda函数也允许在形参中声明中使用auto，但是在这些情况下auto实际上使用模板类型推导的那一套规则在工作，而不是auto类型推导
```cpp
//c++14
auto createInitList(){
    return {1,2,3};     //ERROR! 
}
std::vector<int> v;
auto resetV = [&v](const auto& newValue){v = newValue;};
resetV({1,2,3});        //ERROR!

```

# 10.8
1. 引用或者指针
  - 去除引用属性
  - 模式匹配
2. 通用引用
  - 实参为左值，T和param均为左值引用
  - 实参为右值，模式匹配
3. 既不是指针也不是引用
  - 去除引用属性
  - 去除const属性
  - 去除volatile属性
4. 数组实参
  - 形参为普通参数时，数组名退化为普通指针
  - 形参为引用时，数组名表示数组
```cpp
template<typename T, std::size_t N>
constexpr std::size_t AS(T(&)[N]){
  return N;
}
int vals[] = {1, 2, 3, 4};
int newVals[AS(vals)];

```
