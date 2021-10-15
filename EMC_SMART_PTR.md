## auto_ptr
- 唯一使用场景：C++98编译器。使用unique_ptr替换auto_ptr

## unique_ptr
- 可移动不可复制
- 默认情况下，销毁使用delete来实现
- 常见用法：作为继承层次结构中对象的工厂函数返回类型
```cpp
class Investment {
public:
  virtual ~Investment();  // Important
};
class Stock: public Investment {};
class Bond: public Investment {};
class RealEstate: public Investment {};

template<typename... Ts>
std::unique_ptr<Investment> makeInvestment(Ts&&... params);
{
  //...
  auto pInvestment = makeInvestment(arguments);
  //...
} // destroy *pInvestment

// 自定义删除器：使用lambda比常规函数更方便有效
auto delInvmt = [](Investment* pInvestment){
  makeLogEntry(pInvestment);
  delete pInvestment;
}
template<typename... Ts>
std::unique_ptr<Investment, decltype(delInvmt)>
makeInvestment(Ts&&... params){
  std::unique_ptr<Investment, decltype(delInvmt)> pInv(nullptr, delInvmt);
  if(/*Stock obj*/){
    pInv.reset(new Stock(std::forward<Ts>(params)...));
  }
  else if(/*Bond obj*/){
    pInv.reset(new Bond(std::forward<Ts>(params)...));
  }
  else if(/*RealEstate obj*/){
    pInv.reset(new RealEstate(std::forward<Ts>(params)...);
  }
  return pInv;
}

// 可以轻易转换为shared_ptr
std::shared_ptr<Investment> sp = makeInvestment(arguments);

```
- 如果一个异常传播到线程的基本函数外，或者违反noexcept说明，局部变量可能不会被销毁
- 如果std::abort或者退出函数（std::_Exit, std::exit, std::quick_exit）被调用，局部变量一定没被销毁

- unique_ptr是轻量级、快速的、只可移动的管理转悠所有权语义资源的智能指针
- 默认情况，资源销毁通过delete实现，支持自定义删除器，有状态的删除器和函数指针会增加unique_ptr对象的大小
- 将unique_ptr转换为shared_ptr非常简单

## shared_ptr


## weak_ptr
