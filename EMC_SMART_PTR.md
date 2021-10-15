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
- shared_ptr大小是原始指针的两倍，内部包含一个指向资源的原始指针，还包含一个指向资源的引用计数的原始指针（所有标准库都这样实现）
- 引用计数的内存必须动态分配
- 递增递减引用计数必须是原子性的


- 从另一个shared_ptr移动构造新shared_ptr会将原来的shared_ptr设置为null，意味着老的shared_ptr不再指向资源，同时新的shared_ptr指向资源
- 这样的结果就是不需要修改引用计数值，因此移动比拷贝要快，拷贝要求递增引用计数值，移动不需要
- 移动赋值同理，移动构造比拷贝构造快，移动赋值比拷贝赋值快

```cpp
// 自定义删除器
// shared_ptr删除器不是shared_ptr的一部分
auto loggingDel = [](Widget *pw){
  makeLogEntry(pw);
  delete pw;
}

std::unique_ptr<Widget, decltype(loggingDel)> upw(new Widget, loggingDel);  // 是指针类型的一部分
std::shared_ptr<Widget> spw(new Widget, loggingDel);  // 不是指针类型的一部分

```

std::shared_ptr<T>
ptr to T                ->  T object
ptr to Control block    ->  Control block
                            Reference Count
                            Weak Count
                            Other data(e.g., custom deleter, allocator, etc.)


shared_from_this
- 首先需要创建一个当前类型的shared_ptr对象，然后该函数会查找当前对象对应的控制块，创建一个新的shared_ptr
- 必须在shared_ptr中对象调用该方法  

### 开销
- 动态分配控制块，任意大小的删除器和分配器，虚函数机制（确保指向的对象被正确销毁），原子性的引用计数修改  
- 通常情况下，使用默认删除器和默认分配器，使用make_shared创建shared_ptr，产生的控制块只需三个word大小，分配基本上是无开销的
  

  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  





## weak_ptr
