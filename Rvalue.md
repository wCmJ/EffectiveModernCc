## Rvalue References
Rvalue references bind only to rvalues
```cpp
int n = 10;
int &&ri = n;   //error: n is an lvalue
int const &&rj = n;   //error: n is an lvalue
```

Modern C++ uses rvalue references to implement move operations that can avoid unnecessary copying
```cpp
class string{
public:
  // copy operations
  string(string const&);
  string& operator=(const string&);
  
  // move operations
  string(string&&) noexcept;
  string& operator=(string&&) noexcept;
};
```
  
                expression
         glvalue          rvalue
   lvalue         xvalue        prvalue        


glvalue: a generalized lvalue
prvalue: a pure rvalue
xvalue: an expiring lvalue


右值引用的意义通常解释为两大作用：移动语义和完美转发
移动语义：各种情形下对象的资源所有权转移的问题
左值对应变量的存储位置
右值对应变量的值本身

右值可以被赋值给左值或者绑定到引用
类的右值是一个临时对象，如果没有被绑定都引用，在表达式结束时就会被丢弃。于是我们可以在右值被废弃之前，移走它的资源进行废物利用，从而避免无意义的复制。被移走的资源的右值在废弃时已经成为空壳，析构的开销也会降低
对于左值，如果我们明确放弃对其资源的所有权，则可以通过std::move()来将其转为右值引用
std::move is static_cast<T&&>()
std::move的功能是把左值强制转化为右值，让右值引用可以指向左值，单纯的std::move()不会有性能提升。
```cpp
class People{
public:
  People(string name): name_(move(name)){}
  string name_;
};
People a("Alice");
string bn = "Bob";
People b(bn);

vector<string> str_splite(const string& s){
  vector<string> v;
  // ...
  return v;   // v是左值，但优先移动，不支持移动时仍可复制
}

```
unique_ptr是非常轻量的封装，存储空间等价于裸指针，但安全性强了一个世纪

### 被声明出来的左、右值引用都是左值
```cpp
void change(int&& val){
  val = 8;
}
int a = 5;
int &left = a;
int &&right = std::move(a);

change(a);
change(left);
change(right);

change(std::move(a));
change(std::move(left));
change(std::move(right));

change(5);
```

1. 从性能上来讲，左右值引用没有区别，传参使用左右值引用都可以避免拷贝
2. 右值引用可以直接指向右值，也可以通过std::move指向左值；而左值引用只能指向左值（const左值引用可以指向右值）
3. 作为函数形参时，右值引用更灵活，虽然const左值引用也可以做到左右值都接收，但它无法修改，有一点局限性


## 右值引用和std::move的应用场景
1. 实现移动语义
  - 右值引用和std::move被广泛用于STL和自定义类中实现移动语义，避免拷贝，从而提升程序性能
```cpp
std::string str1 = "AXE";
std::vector<std::string> vec;
vec.push_back(str1);
vec.push_back(std::move(str1)); // str1 will be empty
vec.push_back("AXE2");
// 在vector和string这个场景，加上std::move会调用移动语义函数，避免了深拷贝
```

编译器会默认在用户自定义的class和struct中生成移动语义函数，该函数什么也不做
std::move本身只做类型转换，对性能无影响。我们可以在自己的类中实现移动语义，避免深拷贝，充分利用右值引用和std::move的语言特性

2. 完美转发
std::forward同样是做类型转换
std::move只能转出来右值
forward都可以
std::forward<T>(u)有两个参数：T和u，当T为左值引用类型时，u将被转换为T类型的左值；否则u将被转换为T类型右值
```cpp
void B(int&& ref_r){
  ref_r = 1;  
}

void A(int&& ref_r){
  B(ref_r);
  B(std::move(ref_r);
  B(std::forward<int>(ref_r));  
}  
  
  
  
```












