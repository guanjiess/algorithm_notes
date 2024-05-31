## 题目

1. 统计连续子串长度

```python
groups = defaultdict(int)
cnt = 0
for i,c in enumerate(s):
    cnt += 1
    if i + 1 == len(s) or c != s[i+1]:
        group[c] = cnt
        cnt = 0
```







## python库

1. count()
2. min()
3. ord()





## C++库

1. 



##C++中的string

在C++中，`std::string` 类型表示一个字符串，可以包含任意字符，包括字母、数字、特殊字符等。`std::string` 类型是一个动态大小的字符序列，因此可以存储任意字符。

以下是一些常见的字符类型，`std::string` 可以包含但不限于以下字符：

1. **字母**：包括大写字母（A-Z）和小写字母（a-z）。
2. **数字**：包括数字字符（0-9）。
3. **特殊字符**：例如标点符号（.,!?）、空格、制表符等。
4. **扩展字符集**：包括Unicode字符、特殊符号、表情符号等。

由于 `std::string` 是一个通用的字符串类型，它可以包含几乎任何字符。在处理字符串时，可以使用 `std::string` 来存储和操作各种类型的字符数据。



### 简单string类

```C++
#include <iostream>
#include <vector>

#include <string.h>
#include <stdio.h>

using namespace std;

class MyString {
public:
    MyString() : _data(nullptr), _len(0) {}

    MyString(const char* str) {
        if (str) {
            _len = strlen(str);
            _data = new char[_len + 1];
            strcpy(_data, str);
        } else {
            _len = 0;
            _data = nullptr;
        }
    }

    MyString(const MyString& other) {
        _len = other._len;
        _data = new char[_len + 1];
        strcpy(_data, other._data);
    }

    MyString& operator=(const MyString& other) {
        if (this != &other) {
            delete[] _data;
            _len = other._len;
            _data = new char[_len + 1];
            strcpy(_data, other._data);  // deep copy
        }

        return *this;
    }

    MyString& operator+=(const MyString& other) {
        int new_len = _len + other._len;
        char* new_data = new char[new_len + 1];
        strcpy(new_data, _data);
        strcat(new_data, other._data);

        delete[] _data;

        _len = new_len;
        _data = new_data;

        return *this;
    }

    bool operator==(const MyString& other) {
        if (_len != other._len) {
            return false;
        }

        return strcmp(_data, other._data);
    }

    ~MyString() {
        delete[] _data;
        _len = 0;
    }

private:
    char* _data;
    int _len;
};

```

