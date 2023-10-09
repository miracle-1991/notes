# override

override是一个c++11引入的关键字，告知编译器此函数用于重写父类的同名方法，
强制让编译器在编译期间检查父类是否存在相同的函数签名，主要用于避免在子类中不小心写错了函数签名

用法：
```
#include <iostream>

class A  {
public:
    virtual void func(int a, int b) {
        std::cout << "A::func" << std::endl;
    }
};

class B : public A {
public:
    virtual void func(int a, int b) override {
        std::cout << "B::func" << std::endl;
    }
};

int main() {
    B b;
    b.func(1, 0);
}
```