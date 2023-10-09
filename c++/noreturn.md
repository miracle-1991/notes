# [[noreturn]]

[[noreturn]]是c++11新引入的注解标签，在c11中注解标签可以用来修饰类型、函数或者enum,在c17中扩展到能修饰明明空间和enum class.

[[noreturn]]告诉编译器某个函数不会返回到调用者，也就是在返回之前会通过其他的方式终止程序，比如：
* 异常
* exit

使用方法:
```
#include <iostream>

[[noreturn]] void terminate() {
    std::cout << "terminate" << std::endl;
    std::exit(1);  //实测，即使没有调用exit，只要有[[noreturn]]标签，end也不会输出
}

int main() {
    terminate();
    std::cout << "end" << std::endl; //end不会输出
}
```
结果：
```
terminate
```