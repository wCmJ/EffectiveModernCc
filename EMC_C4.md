## move & forward
move on rvalue references, forward on universal references
```cpp
class Widget{
public:
  Widget(Widget&& rhs);
};

// 传递这样的对象给其他函数，允许那些函数利用对象的右值性，将绑定到此类对象的形参转换为右值
class Widget{
public:
  Widget(Widget&& rhs): name(std::move(rhs.name)), p(std::move(rhs.p)){}
private:
  std::string name;
  std::shared_ptr<SomeDataStructure> p;
};

// 通用引用可能绑定到有资格移动的对象上，通用引用使用右值初始化时，才将其强制转换为右值
class Widget{
public:
  template<typename T>
  void setName(T&& newName){
    name = std::forward<T>(newName);
  }
};

// 总而言之：把右值引用转发给其他函数时，右值引用应该被无条件转换为右值（std::move），因为他们总是绑定到右值；当转发通用引用时，通用引用应该有条件地转换为右值（std::forward），因为它们只是有时绑定到右值


```
