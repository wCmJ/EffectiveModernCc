## decltype
```cpp
// V1
template<typename Container, typename Index>
auto authAndAccess(Container& c, Index i) -> decltype(c[i]){
  return c[i];
}
// vector<bool>的operator[]不会返回bool&，它会返回一个全新的对象
// 对一个容器进行operator[]操作返回的类型取决于容器本身
// auto不会做任何类型推导工作，相反的，它只是暗示使用了C++11的尾置返回类型语法，即在函数形参列表后面使用一个 ”->“ 符号指出函数的返回类型
// 这么做的原因是可以在函数返回类型中使用函数形参相关的信息，如果按照传统语法把函数返回类型放在函数名称之前，由于未被声明所以不能使用
// C++11
// 允许自动推导单一语句的lambda表达式的返回类型
// C++14
// 扩展到允许自动推导所有的lambda表达式和函数，甚至它们内含多条语句

// V2
template<typename Container, typename Index>
auto authAndAccess(Container& c, Index i){
  return c[i];
}
// 函数返回类型中使用auto，编译器实际上是使用的模板类型推导的规则
// 在模板类型推导期间，表达式的引用型会被忽略，下面语句会导致编译错误
std::deque<int> d;
authAndAccess(d, 5) = 10;

// 要想让authAndAccess像我们期待的那样工作，需要使用decltype类型推导来推导它的返回值
// decltype(auto)：auto说明符表示这个类型将会被推导，decltype说明decltype的规则将会被用到这个推导过程中
// V3
template<typename Container, typename Index>
decltype(auto) authAndAccess(Container& c, Index i){
  return c[i];
}
// decltype(auto)的使用不仅仅局限于函数返回类型，当你想对初始化表达式使用decltype推导的规则，可以如下使用：
Widget w;
const Widget& cw = w;
auto my_widget1 = cw;             // my_widget1类型为Widget
decltype(auto) my_widget2 = cw;   // my_widget2类型为const Widget&
// 容器通过传引用的方式传递非常量左值引用，因为返回一个引用运行用户可以修改容器
// 这意味着不能给这个函数传递右值容器，右值不能被绑定到左值引用上，除非是一个const左值引用
// 为了支持左值和右值，可以使用：1. 重载，需要同时维护两个函数；2. 通用引用
// V4 最终C++14版本
template<typename Container, typename Index>
decltype(auto) authAndAccess(Container&& c, Index i){
  return std::forward<Container>(c)[i];
}
// V5 最终C++11版本
template<typename Container, typename Index>
auto authAndAccess(Container&& c, Index i) -> decltype(std::forward<Container>(c)[i]){
  return std::forward<Container>(c)[i];
}

// 将decltype应用于变量名会产生该变量名的声明类型
// 对于比单纯的变量名更复杂的左值表达式，decltype可以确保报告的类型始终是左值引用
// 也就是说，如果一个不是单纯变量名的左值表达式的类型是T，那么decltype会把这个表达式的类型报告为T&

int x = 0;
decltype(x) == int
decltype((x)) == int&

decltype(auto) f1(){
  int x = 0;
  return x;             // decltype(x) is int
}

decltype(auto) f2(){
  int x = 0;
  return (x);           // decltype((x)) is int&
}
```

- decltype总是不加修改的产生变量或者表达式的类型
- 对于T类型的不是单纯的变量名的左值表达式，decltype总是产出T的引用即T&
- C++14支持decltype(auto)，就像auto引用，推导出类型，但是它使用decltype的规则进行推导

