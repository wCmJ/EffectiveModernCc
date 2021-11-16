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
```
