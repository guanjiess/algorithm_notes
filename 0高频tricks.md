##字符串相关trick

###判断回文

```c++
bool isPalindrome(const string& s, int start, int end) {
     for (int i = start, j = end; i < j; i++, j--) {
         if (s[i] != s[j]) {
             return false;
         }
     }
     return true;
}
```

###大小写切换

C++

```C++
x ^= ' ';
x = x ^ 32
```

python

```python
x.lower()
x.upper()
```

==是否为字母、数字==

C++

```C++
isalnum(c)
isdigit(c)
```

Python

```python
c.isalnum()
c.isdigit()
```



### 整数转字符

1. to_string()
2. stoi()，字符串转整型数
3. stoll()，字符串转long long型
4. 转1-26的数字：  x  & 31

###ASCII相关

1. https://www.ascii-code.com/
2. 共有128个字符
3. 数字范围：48-57 
4. 大写字母范围：65-90
5. 小写字母范围：97-122。小写字母、大写字母ascii字符只有低6位不同，小写第六位是1，大写第六位是0

##库函数

1. lower_bound：二分查找第一个大于等于x的下标
2. upper_bound：二分查找第一个大于x的下标

```C++
int main() {
    std::vector<int> vec = {1, 2, 3, 4, 4, 5, 6, 7};
    
    // 使用lower_bound找到第一个大于等于4的元素的位置
    auto lower = std::lower_bound(vec.begin(), vec.end(), 4);
    
    // 使用upper_bound找到第一个大于4的元素的位置
    auto upper = std::upper_bound(vec.begin(), vec.end(), 4);
}
```

1. ranges::max()

2. ranges::min()

3. ranges::sort()排序

4. accumulate()

5. isalpha() 判断字符

6. substr()：取子串

7. lcm(a, b)，求ab的最小公倍数，在<numeric>头中

8. gcd(a, b)，求ab的最大公约数，在<numeric>头中

9. accumulate()，求和函数，左闭右开区间。

   ```C++
   int sum = accumulate(arr+startIdx, arr+endIdx, init)
   ```

   



##Lambda表达式



```C++
		auto check = [&](long long t) -> bool {
            long long cnt = 0;
            for (int period: time) {
                cnt += t / period;
            }
            return cnt >= totalTrips;
        };
```

在 C++ 中，lambda 表达式是一种匿名函数，可以在需要时定义并使用，类似于 Python 中的 lambda 函数。Lambda 表达式可以方便地在函数内部定义一个函数对象，用于简化代码和提高可读性。

==语法==

Lambda 表达式的一般形式如下：
```cpp
[capture list](parameters) -> return_type {
    // 函数体
};
```

在上述代码中的 lambda 表达式 `auto check = [&](long long t) -> bool { ... };` 中：
- `auto`：用于自动推断 lambda 表达式的类型。
- `&`：表示引用捕获，即 lambda 表达式可以访问函数外部作用域的变量，并且可以修改这些变量。
- `(long long t)`：lambda 表达式的参数列表，这里接受一个 `long long` 类型的参数 `t`。
- `-> bool`：指定 lambda 表达式的返回类型为 `bool` 类型。
- `{ ... }`：lambda 表达式的函数体，包含了计算逻辑和返回语句。

==优点==

- 简洁：可以在需要时定义一个匿名函数，避免了额外的函数定义。
- 方便：可以直接在函数内部定义函数对象，减少了代码量和提高了可读性。
- 灵活性：可以方便地捕获外部作用域的变量，使得 lambda 表达式更加灵活和功能强大。

通过 lambda 表达式，可以在 C++ 中实现类似于函数式编程的特性，简化代码逻辑并提高代码的可维护性。



==capture list==

在 C++ 中，lambda 表达式的捕获列表 `[capture list]` 用于指定 lambda 函数体内可以使用哪些外部作用域中的变量，以及如何使用这些变量（通过值捕获、通过引用捕获等）。下面通过几个例子来详细说明不同的捕获方式。

1. 值捕获 `[=]`

通过值捕获，lambda 表达式可以使用其外部作用域中所有变量的副本，但不能修改这些变量。

```cpp
#include <iostream>
int main() {
    int x = 10;
    auto copy_x = [=]() { return x; }; // x 通过值被捕获
    std::cout << copy_x() << std::endl; // 输出: 10
    // x 在 lambda 表达式中是只读的，不能修改
}
```

2. 引用捕获 `[&]`

通过引用捕获，lambda 表达式可以直接访问并修改其外部作用域中的所有变量。

```cpp
#include <iostream>
int main() {
    int x = 10;
    auto increase_x = [&]() { x++; }; // x 通过引用被捕获
    increase_x(); // x 的值被修改
    std::cout << x << std::endl; // 输出: 11
}
```

3. 显示捕获单个变量

你可以明确指定捕获列表中的变量，无论是通过值还是通过引用。

- 通过值捕获单个变量 `[x]`：

```cpp
#include <iostream>
int main() {
    int x = 10;
    int y = 20;
    auto copy_x = [x]() { return x; }; // 只有 x 被通过值捕获
    std::cout << copy_x() << std::endl; // 输出: 10
    // y 没有被捕获，因此在 lambda 中不可用
}
```

- 通过引用捕获单个变量 `[&x]`：

```cpp
#include <iostream>
int main() {
    int x = 10;
    auto increase_x = [&x]() { x++; }; // 只有 x 被通过引用捕获
    increase_x(); 
    std::cout << x << std::endl; // 输出: 11
}
```

4. 混合捕获

你可以在捕获列表中同时使用值捕获和引用捕获。

```cpp
#include <iostream>
int main() {
    int x = 10, y = 20;
    auto mix = [x, &y]() { 
        std::cout << "x: " << x << ", y: " << y << std::endl; // x 通过值捕获, y 通过引用捕获
        y++; // 可以修改 y
    };
    mix();
    std::cout << "y after lambda: " << y << std::endl; // 输出: 21
    // 注意 x 在 lambda 中是只读的，y 被修改
}
```

5. 默认引用捕获，单独值捕获

```cpp
#include <iostream>
int main() {
    int x = 10, y = 20;
    auto lambda = [&, x]() { // 默认通过引用捕获，但 x 通过值捕获
        // x 是只读的，不能修改
        y++; // y 通过引用捕获，可以修改
    };
    lambda();
    std::cout << "y: " << y << std::endl; // 输出: 21
}
```

通过这些例子，你应该能够更好地理解 C++ 中 lambda 表达式的捕获列表 `[capture list]` 的工作原理及其用法了。