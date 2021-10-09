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
   lvalue         xvalue         prvalue        
glvalue: a generalized lvalue
prvalue: a pure rvalue
xvalue: an expiring lvalue










