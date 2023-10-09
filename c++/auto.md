# auto
auto是c++11中重新定义了用法的关键字。主要用于让编译器自动推导一些复杂的类型，以简化语法。

例如：
```
#include <iostream>
#include <map>
#include <unordered_map>
#include <string>

int main() {
    std::map<std::string, std::string> seasons;
    seasons["spring"] = "123";
    seasons["summer"] = "456";
    seasons["autumn"] = "789";
    seasons["winter"] = "101112";
    // auto is std::map<std::string, std::string>::iterator
    for (auto iter = seasons.begin(); iter != seasons.end();iter++) {
        std::cout << iter->first << " : " << iter->second << std::endl;
    }

    std::cout << "------------------" << std::endl;

    std::unordered_map<std::string, std::string> unorderSeasons;
    unorderSeasons["spring"] = "123";
    unorderSeasons["summer"] = "456";
    unorderSeasons["autumn"] = "789";
    unorderSeasons["winter"] = "101112";
    // auto is std::unordered_map<std::string, std::string>::iterator
    for (auto iter = unorderSeasons.begin(); iter != unorderSeasons.end(); iter++) {
        std::cout << iter->first << " : " << iter->second << std::endl;
    }
}
```
输出：
```
autumn : 789
spring : 123
summer : 456
winter : 101112
------------------
winter : 101112
autumn : 789
summer : 456
spring : 123
```