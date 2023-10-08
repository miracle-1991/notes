# PIMPL
pimpl(Pointer to Implementation)是一种常用的封装方法，用来在对外的接口中隐藏自己的实现，一般会定义为内部类使用

这种封装方法有如下优点:
* 核心数据成员被隐藏，对使用者透明，使用者不容易推测出内部的实现
* 头文件变得干净，依赖的类型减少，降低了编译依赖，提高了编译速度
* 对外头文件的内容不变的情况下可以随意更改impl的成员与方法

实现参考:
* 头文件
```
#include <memory>

class SocketClient
{
public:
    SocketClient();
    ~SocketClient();
    bool Init();
private:
    class Impl;
    std::unique_ptr<Impl> m_pImpl;
};
```
* 具体实现
```
#include "SocketClient.h"
#include <iostream>

class SocketClient::Impl {
public:
    Impl(): n(0) {}
    bool Init() {
        std::cout << "impl init" << std::endl;
        return true;
    }
private:
    int n;  
};

SocketClient::SocketClient(): m_pImpl(std::make_unique<Impl>()) {}
SocketClient::~SocketClient() {}

bool SocketClient::Init() { return m_pImpl->Init(); }
```
* 测试代码
```
#include "SocketClient.h"

int main() {
    SocketClient cli;
    cli.Init();
}
```
* 输出结果
```
impl init
```