# map

c++11中的map是STL中模板容器类型，内部使用红黑树进行实现，因此其插入、删除和查找的效率都是O(log n)。map在声明
时会自动初始化，并且可以直接使用，不用像golang一样必须使用make申请内存。

比如：
```
#include <iostream>
#include <map>
#include <unordered_map>
#include <string>

int main() {
    // c++中的map在声明时初始化
    std::map<std::string, std::string> seasons;

    // 如果键不存在，会自动创建
    seasons["spring"] = "123";
    seasons["summer"] = "456";
    seasons["autumn"] = "789";
    seasons["winter"] = "101112";


    // 用fist表示key, second表示value
    // c++中的map由红黑树实现，内部有序，排序依据是key的operator <
    for (auto iter = seasons.begin(); iter != seasons.end();iter++) {
        std::cout << iter->first << " : " << iter->second << std::endl;
    }

    // 查找, 由于内部是红黑树实现的，所以查找效率是O(log(n))
    if (seasons.find("no") != seasons.end()) {
        std::cout << "found" << std::endl;
    } else {
        std::cout << "not found" << std::endl;
    }
}
```
而在golang中，map是语言自带的内建类型，内部使用散列表进行实现，平均情况下的插入、删除、查找的复杂度都是O(1)
golang中map类型的零值是nil，直接在nil上操作会引发panic，所以必须使用make或者字面量语法初始化。

比如:
```
package main

import "fmt"

func main() {
	// golang中map需要使用make初始化，否则是nil
	seasons := make(map[string]string)

	seasons["spring"] = "123"
	seasons["summer"] = "456"
	seasons["autumn"] = "789"
	seasons["winter"] = "101112"

	// 遍历时，第一个值是key, 第二个值是val
	// 由于内部是散列表实现的，所以无序
	for k, v := range seasons {
		fmt.Printf("%v :  %v\n", k, v)
	}

	// 查找, 通过额外的ok布尔值判断键是否存在
	if v, ok := seasons["no"]; ok {
		fmt.Printf("found %v\n", v)
	} else {
		fmt.Printf("not found\n")
	}
}
```
c++中的unordered_map也是用散列表实现的，从用法和执行效率上最接近golang中的map，但是有一个差异的地方：
* c++中通过operator[]对不存在的键进行访问，会创建一个新的元素
* golang中则不会
比如:
* c++ 
```
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::unordered_map<std::string, std::string> seasons;
    auto val = seasons["winter"];
    std::cout << val << std::endl;

    if (seasons.find("winter") != seasons.end()) {
        std::cout << "found" << std::endl;
    } else{
        std::cout << "not found" << std::endl;
    }
}
```
输出:
```

found
```
* golang
```
package main

import "fmt"

func main() {
	seasons := make(map[string]string)
	val := seasons["winter"]
	fmt.Printf("%v\n", val)

	if _, ok := seasons["winter"]; ok {
		fmt.Println("found")
	} else {
		fmt.Println("not found")
	}
}
```
输出:
```

not found
```