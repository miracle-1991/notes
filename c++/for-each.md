# for-each

for-each语法是c++引入的，可以方便进行遍历，在遍历的过程中可以自由控制按值传递或者按引用传递：
```
#include <iostream>
#include <string>
#include <vector>

int main() {
    std::vector<std::string> v;
    v.push_back("zhangsan");
    v.push_back("lisi");
    v.push_back("wangwu");
    std::cout << "before change" << std::endl;
    for (auto iter : v) { //按值传递
        std::cout << iter << std::endl;
    }

    for (auto& iter : v) { //按引用传递
        iter = "hello";
    }
    std::cout << "after change" << std::endl;
    for (auto iter : v) { //按值传递
        std::cout << iter << std::endl;
    }
}
```
输出：
```
before change
zhangsan
lisi
wangwu
change
after change
hello
hello
hello
```
golang中也有for-each语法，但是只能按值传递：
```
package main

import "fmt"

func main() {
	v := []string{"zhangsan", "lisi", "wangwu"}
	fmt.Printf("before change\n")
	for _, item := range v {
		fmt.Printf("%v\n", item)
	}

	for _, item := range v {
		item = "hello"
		fmt.Printf("change to %v\t", item)
	}

	fmt.Println("\nafter change\n")
	for _, item := range v {
		fmt.Printf("%v\n", item)
	}
}
```
输出:
```
before change
zhangsan
lisi
wangwu
change to hello change to hello change to hello 
after change

zhangsan
lisi
wangwu
```