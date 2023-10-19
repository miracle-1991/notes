# 多态与运行时识别

c++与golang都有自己的多态，但是实现方式却大相径庭，这主要与语言的思想有关。
在c++中多态基于virtual进行实现：
* 父类定义virtual函数
* 子类继承后进行重写  

这样会导致：
* 编译器为包含虚函数的类添加一个虚函数表，记录各个虚函数的实现，这个虚函数表保存在代码段
* 子类继承父类并进行重写时，会将虚函数表中的实现进行覆盖

在运行时：
* 每个类生成的对象中都会多一个指针，也就是虚函数表指针，指向虚函数表
* 然后在虚函数表中寻找具体实现进行调用
```
#include <iostream>
#include <map>
#include <memory>

class Base {
public:
    virtual void Speak() { std::cout << "human language!" << std::endl; }
};

class EN : public Base{
public:
    virtual void Speak() override { std::cout << "english" << std::endl; }
    int n;
    int m;
};

class CN {
public:
    void Speak() { std::cout << "chinese" << std::endl; }
    int n;
    int m;
};

int main() {
    std::unique_ptr<Base> sp = std::make_unique<EN>();
    sp->Speak();

    int* a = new int;
    std::cout << "size of normal pointer: " << sizeof(a) << ", sizeof sp: " << sizeof(sp) << std::endl;

    EN e = EN();
    CN c = CN();
    std::cout << "sizeof en: " << sizeof(e) << ", sizeof cn: " << sizeof(c) << std::endl;
}
```
输出:
```
english
size of normal pointer: 8, sizeof sp: 8
sizeof en: 16, sizeof cn: 8
```
golang中也有自己的多态，但是实现更简单。
比如：
```
package main

import "fmt"

type Base interface {
	Speak()
}

type EN struct{}

func (e *EN) Speak() {
	fmt.Printf("Speak English\n")
}

func main() {
	var sp Base
	sp = new(EN)
	sp.Speak()
}
```
在golang中，interface其实是一个数据结构，分为空空接口结构和非空接口结构，在此处是非空接口结构
在非空接口结构可以表示如下：
```
type itab struct {
	fun [1]unsafe.Pointer
}

type iface struct {
	tab  *itab
	data unsafe.Pointer
}
```
在这个非空接口结构中，fun指向了EN的具体实现，从而实现了多态。



