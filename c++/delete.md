# delete

delete 关键字起到default关键字相反的操作，用于禁止编译器自动生成某些函数，比如，如果想让一个类不能被拷贝，可以
使用delete禁止生成拷贝构造和拷贝赋值函数：
```
#include <iostream>

class A  {
public:
    A(int n) : m(n) {}

    A() = default; //构造函数
    ~A() = default; //析构函数

    A(const A&) = delete; //复制构造函数
    A(A&&) = default; //移动构造函数

    A& operator=(const A&) = delete; //复制赋值函数
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
这段测试代码会遇到编译错误:
```
main.cpp:21:7: error: call to deleted constructor of 'A'
    A b(a);
      ^ ~
main.cpp:10:5: note: 'A' has been explicitly marked deleted here
    A(const A&) = delete; //复制构造函数
    ^
main.cpp:23:7: error: call to deleted constructor of 'A'
    A d = c;
      ^   ~
main.cpp:10:5: note: 'A' has been explicitly marked deleted here
    A(const A&) = delete; //复制构造函数
    ^
2 errors generated.
```