```cpp
auto x = 27;
const auto cx = x;
const auto &rx = cx;
//编译器的行为看起来就像是认为这里每个声明都有一个模板，然后使用合适的初始化表达式进行调用
template<typename T>
void func_for_x(T param);
func_for_x(27);

template<typename T>
void func_for_cx(const T param);
func_For_cx(x);

template<typename T>
void func_for_rx(const T& param);
func_for_rx(x);

//除了一个情况外，auto和模板推导是一样的
auto x = 27;            //情景三
const auto cx = x;      //情景三
const auto& rx = cx;    //情景一

auto&& ref1 = x;        //情景二 int&
auto&& ref2 = cx;       //情景二 const int&
auto&& ref3 = 27;       //情景二 int&&

const char name[] = "R. N. Briggs";
auto arr1 = name;       //const char*
auto& arr2 = name;      //const char (&)[13]

void someFunc(int, double);
auto func1 = someFunc;  //void(*)(int, double)
auto& func2 = someFunc; //void(&)(int, double)

//auto与模板类型推导不同点
int x1 = 27;
int x2(27);
int x3 = {27};
int x4{27};

auto x1 = 27;       //int
auto x2(27);        //int
auto x3 = {27};     //std::initializer_list<int>
auto x4{27};        //std::initializer_list<int>
//当用auto声明的变量使用花括号进行初始化时，auto类型推导推出的类型则为std::initializer_list
auto x5 = {1, 2, 3.0};      //类型不一致，编译报错
//这个过程中进行了两种类型推导：auto+花括号被推导为std::initializer_list<T>，花括号中的变量用来推导T
//对于花括号的处理是auto类型推导和模板类型推导唯一不同的地方，当使用auto声明的变量使用花括号进行初始化时，会推导出initializer_list<T>，但是对于模板类型推导行不通
template<typename T>
void f(T param);
f({11, 23, 9});
//auto类型推导假定花括号表示initializer_list而模板类型推导不知道怎么处理花括号
//在c++14中，auto可用于函数参数和返回值、lambda中，但是此种情况下，auto是使用模板类型推导规则工作
auto CreateInitList(){
    return {1,2,3}; //ERROR
}

auto resetV = [](const auto& newValue){};
resetV({1, 2, 3});  //ERROR

```
1. auto类型推导和模板类型推导相同，但是auto类型推导假定花括号初始化代表std::initializer_list，而模板类型推导不这样做
2. 在C++14中auto允许出现在函数返回值或者lambda函数形参中，但是它的工作机制是模板类型推导那一套方案，而不是auto类型推导
