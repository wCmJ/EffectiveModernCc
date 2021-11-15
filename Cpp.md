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

### Virtual
1. 为什么构造函数不能是虚函数？
- 虚函数表vtable是在编译期建立，但是指向虚函数表的指针vptr是在运行阶段实例化对象才产生的
- 编译器会在构造函数中添加代码来创建vptr
- 如果构造函数是虚函数，那么需要vptr来访问vtable，这个时候vptr还没有产生，所以构造函数不能为虚函数

2. 静态函数可以声明为虚函数吗？不可以
- 静态函数不能声明为虚函数，同时也不能被const和volatile关键性修饰
- static成员函数不属于任何对象或实例，所以即使给静态函数加上virtual也没有任何意义
- 静态函数没有this指针，无法调用vptr，不能使用虚函数

3. 析构函数可以是虚函数吗？ 可以
- 需要删除一个指向派生类的基类指针时，应该把析构函数声明为虚函数
- 一个类可能作为基类时，应该声明为虚析构函数

4. 虚函数可以为私有函数吗？
- 可以，需要使用友元功能才能被访问，仍然保持虚函数特性，友元功能增加了访问权限

```cpp
class Base {
    private: 
        virtual void fun() { cout << "Base Fun1"<<std::endl; }
        friend int main();
};

class Derived: public Base {
    private:
        void fun() { cout << "Derived Fun1"<<std::endl; }
        void fun22() { cout << "Derived Fun"<<std::endl; }
};

int main() {
    Base *ptr = new Derived;
    ptr->fun();
}
```

5. 虚函数可以被内联吗？ 可以给虚函数加上inline，但是是否使用由编译器决定
- 虚函数可以是内联函数，内联是可以修饰虚函数的，但是当虚函数表现多态性时不能内联
- 内联是在编译期建议编译器内联，而虚函数的多态性是在运行期，编译器无法知道运行期调用哪个代码


6. RTTI和dynamic_cast
- RTTI通过运行时类型信息程序能够使用基类的指针或引用来检查这些指针或引用所指的对象的实际派生类型
- dynamic_cast查询一个对象是否能作为某种多态类型使用，相比于C的强制类型转换和C++的reinterpret_cast提供了类型安全检查
- 一般用来向下转型，即基类的指针转换为派生类的指针，如果可以转换成功，则返回派生类指针，如果不能，返回NULL
- typeid用于引用或者指针解引用时，得到的是运行期类型
- typeid用于指针或者非引用时，得到的是静态类型


















