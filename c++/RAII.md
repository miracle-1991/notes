# RAII

资源获取就是初始化(Resource Acquisition Is Initialization): 
* 资源在我们拿到时就已经初始化
* 一旦不需要该资源，就可以自动释放该资源

主要用处:
* socket fd
* lock
* 申请的内存
* ...

使用示例:
```
#include <mutex>
#include <iostream>
#include <thread>
#include <future>

class SomeLockGuard {
public:
    explicit SomeLockGuard(std::mutex& lock) : m_mutex(lock) {
        std::cout << "lock" << std::endl;
        m_mutex.lock();  //获取资源时进行初始化
    }
    ~SomeLockGuard() {
        std::cout << "unlock" << std::endl;
        m_mutex.unlock(); //一旦不需要该资源，自动进行释放
    }
private:
    std::mutex& m_mutex;
};

int main() {
    int globalCount = 0;
    std::mutex globalLock;
    auto printFunc = [&globalLock, &globalCount]() {
        for (int i = 0; i < 3; i++) {
            {
                auto guard = SomeLockGuard(globalLock);
                globalCount++;
                std::cout << "count: " << globalCount << std::endl;
            }
        }
    };
    auto f1 = std::async(std::launch::async, printFunc);
    auto f2 = std::async(std::launch::async, printFunc);
    f1.get();
    f2.get();
}
```
输出:
```
lock
count: 1
unlock
lock
count: 2
unlock
lock
count: 3
unlock
lock
count: 4
unlock
lock
count: 5
unlock
lock
count: 6
unlock
```
golang中使用RAII实现同样的输出:
```
package main

import (
	"fmt"
	"sync"
)

type RAIIItem struct {
	sync.Mutex
	Count int
}

func inc(item *RAIIItem) {
	item.Lock()
	fmt.Println("lock")

	defer func() {
		fmt.Println("unlock")
		item.Unlock()
	}()

	item.Count++
	fmt.Printf("count: %v\n", item.Count)
}

func addAndPrint(item *RAIIItem) {
	for i := 0; i < 3; i++ {
		inc(item)
	}
}

func main() {
	item := new(RAIIItem)
	runWG := sync.WaitGroup{}
	for i := 0; i < 3; i++ {
		runWG.Add(1)
		go func() {
			defer runWG.Done()
			addAndPrint(item)
		}()
	}
	runWG.Wait()
}
```