# final

final是一个c++11引入的关键字，用于修饰类，表示该类无法被继承

用法:
```
#include <iostream>

class A final {

};

class B : public A {

};

int main() {
    B b;
}
```
编译器会报错:
```
main.cpp:7:18: error: base 'A' is marked 'final'
class B : public A {
                 ^
main.cpp:3:7: note: 'A' declared here
class A final {
      ^ ~~~~~
1 error generated.
```