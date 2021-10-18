## Rvalues References, Move Semantics and Perfect Forwarding
- 移动语义：使编译器有可能用廉价的移动操作来代替昂贵的拷贝操作。拷贝构造函数和拷贝赋值运算符提供拷贝语义，移动构造函数和移动赋值运算符提供移动语义的能力。移动语义允许创建只可移动的类型，例如std::unique_ptr, std::future, std::thread
- 完美转发：使接收任意数量实参的函数模板成为可能，它可以将实参转发到其他的函数，使目标函数接收到的实参与被传递给转发函数的实参保持一致
- 右值引用：连接移动语义和完美转发的胶合剂。使移动语义和完美转发变得可能的基础语义机制
- std::move并不移动任何东西
- 移动操作并不永远比复制操作更廉价
- 形参永远是左值，即使它的类型是一个右值引用


### std::move and std::forward
为了了解std::move和std::forward，从它们不做什么开始
- std::move不移动任何东西
- std::forward也不转发任何东西
- 在运行时，它们不做任何事情，不产生任何可执行代码
- std::move和std::forward仅仅是转换的函数（函数模板）。std::move无条件将它的实参转换为右值，而std::forward只在特定的情况满足时进行转换
- c++11的std::move的示例实现
```cpp
template<typename T>
typename remove_reference<T>::type&& move(T&& param){
  using ReturnType = typename remove_reference<T>::type&&;
  return static_cast<ReturnType>(param);
}
// std::move接受一个对象的引用（通用引用），返回一个指向同对象的引用
```
- c++14中更简单的实现
```cpp
template<typename T>
decltype(auto) move(T&& param){
  using ReturnType = remove_reference_t<T>&&;
  return static_cast<ReturnType>(param);
}
```
- std::move除了转换它的实参到右值以外什么也不做。它只进行转换，不移动任何东西，表示更容易指定可以被移动的对象
- 右值经常是移动操作的候选者
```cpp
class Annotation{
public:
  explicit Annotation(std::string text);
};
// 假如Annotation类的构造函数解决需要读取text的值，并不需要修改它，修改为如下
class Annotation{
public:
  explicit Annotation(const std::string text);
};
// 为了避免一次复制操作的代价，把std::move应用到text上，因此产生一个右值
class Annotation{
public:
  explicit Annotation(const std::string text): value(std::move(text)){}
private:
  std::string value;
};
// 上述实现中，text并不是被移动到value，而是被拷贝。
/*
  text通过std::move被转换为右值，text声明是const std::string，转换之前text是一个左值的const std::string，转换的结果是一个右值的const std::string，整个过程中，const属性一直保留
  class string{
  public:
    string(const string& rhs);
    string(string&& rhs);
  };
  关键点：
    std::move(text)是一个const std::string的右值，这个右值不能被传递给std::string的移动构造函数，因为移动构造函数只接受一个执行non-const的std::string的右值引用
    然而，该右值却可以被传递给std::string的拷贝构造函数，因为lvalue-reference-to-const允许绑定到一个const右值上
    因此，std::string在成员初始化的过程中调用了拷贝构造函数，即使text已经被转换成了右值
    从一个对象中移动出某个值通常代表着修改该对象，所以语言不允许const对象被传递给可以修改它们的函数（例如移动构造函数）
*/
```
- 不要再希望能移动对象的时候，声明它们为const。对const对象的移动请求会悄无声息的被转化为拷贝操作
- std::move不仅不移动任何对象，而且也不保证它执行转换的对象可以被移动
- std::move能确保的唯一一件事就是将它应用到一个对象上，你能够得到一个右值
- std::forward只有在满足一定条件的情况下才执行转换，std::forward是有条件的转换
最常见的使用场景
```cpp
void process(const Widget& lvalArg);  // 处理左值
void process(Widget&& rvalArg);      // 处理右值

template<typename T>
void logAndProcess(T&& param){
  auto now = std::chrono::system_clock::now();
  makeLogEntry("calling process", now);
  process(std::forward<T>(param));
}

Widget w;
logAndProcess(w);             // 用左值调用
logAndProcess(std::move(w));  // 用右值调用
// param作为一个形参，是一个左值。每次在函数logAndProcess内部对process的调用，都会调用process的左值重载版本
/*
   我们需要一种机制：当且仅当传递给函数logAndProcess的用以初始化param的实参是一个右值时，param会被转换为一个右值
   这即是std::forward做的事情，它的实参用右值初始化时，转换为一个右值
   在上述代码中，std::forward是如何分辨param是被一个左值还是右值初始化的？简短的说，该信息藏在模板参数T中，该参数传递给了函数std::forward，它解开了含在其中的信息

*/
```
- std::move总是执行转换，而std::forward偶尔为之
- 从纯技术的角度看，可以用std::forward替代std::move
```cpp
class Widget{
public:
  Widget(Widget&& rhs): s(std::move(rhs.s)){
    ++moveCtorCalls;
  }
private:
  static std::size_t moveCtorCalls;
  std::string s;
};

class Widget{
public:
  Widget(Widget&& rhs): s(std::forward<std::string>(rhs.s)){    // 不自然，不合理的实现
    ++moveCtorCalls;
  }
};
```
## 总结
- std::move执行到右值的无条件的转换，它不移动任何东西
- std::forward只有当它的参数被绑定到一个右值时，才将参数转换为右值
- std::move和std::forward在运行期什么也不做










