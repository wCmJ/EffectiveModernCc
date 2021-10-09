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
// 容器通过







```
