### what is c++?
1. C
2. class
3. template
4. STL


### try to replace #define with const, enum, inline
1. #define can not be seen by compiler
2. const variable in class
  - should be constant
  - should be defined in class
  - should be static for class only
```cpp
// the sentence in class is a declaration not a define
// but for static const int/char/bool, we can do something special
// we can only provide declaration if we do not get its address. you should have a defination if you want to get address.
const int GamePlayer::SIZE; // not value
class GamePlayer{
private:
  static const int SIZE = 1;
  int arr[SIZE];
};
// if compiler do not support initialize in class, use enum
class GP{
private:
  enum {NT = 5};
  int arr[NT];
};

// use #define as MACRO
#define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b))
// every variable should be in paranthesises
int a = 5, b = 0;
CALL_WITH_MAX(++a, b);  // ++ once
CALL_WITH_MAX(++a, b + 10); // ++ twice

// use template inline to replace CALL_WITH_MAX
template<typename T>
inline void callWithMax(const T& a, const T& b){
  f(a > b ? a : b);
}
```
- for constant variable, use const or enums to replace #defines
- for macro like function, use inline function to replace #defines

### Use const whenever possible
- declare iterator as const is same with T* const
- if you want to get a const T*, use const_iterator
```cpp
std::vector<int> vec;
//...
const std::vector<int>::iterator iter = vec.begin();
*iter = 10;
std::vector<int>::const_iterator cIter = vec.begin();
*cIter = 10; // error
++cIter;
```
- const used in function
- const for member functions: const and non-const can be overloaded functions
```cpp



```

### Make sure that objects are initialized before they're used
- initialize objects with build-in type by yourself since c++ do not promise
- constructors should use member initialization list and do not use assignment in constructor body, ensure the order of initialize is same with the order of declare
- to avoid the wrong order of initialize, use local static replace non-local static


### Know what functions C++ silently writes and calls
- compiler will make default constructor, copy constructor, copy assignment operator and destructor for class
















