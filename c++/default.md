# default

default 是c++11引入的一个关键字，用于让编译器自动生成以下函数：
* 构造函数
* 析构函数
* 复制构造
* 移动构造
* 复制赋值
* 移动赋值

用法:
```
#include <iostream>

class A  {
public:
    A(int n) : m(n) {}

    A() = default; //构造函数
    ~A() = default; //析构函数

    A(const A&) = default; //复制构造函数
    A(A&&) = default; //移动构造函数

    A& operator=(const A&) = default; //复制赋值函数
    A& operator=(A&&) = default; //移动赋值函数
private:
    int m;
};

int main() {
    A a;
    A b(a);
    A c(std::move(b));
    A d = c;
    A e = std::move(d);
}
```
