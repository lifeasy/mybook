# STL

## 基本概念

* STL广义上分为容器、算法和迭代器
* 容器和算法通过迭代器无缝连接

## STL六大组件

* 容器：vector、list、map...
* 算法：sort、find、for_each...
* 迭代器：容器和算法之间的胶合剂
* 仿函数：行为类似函数，可所谓算法的某种策略。（重载（）运算符）
* 适配器：一种用来修饰容器、迭代器或者仿函数接口的东西
* 空间配置器：负责空间的配置与管理。

## 容器初识

1. 基本使用 & 迭代

```c++
#include <vector>
#include <algorithm>
using namespace std;

void myPrint(int val) {
    cout << val << endl;
}

void test01() {
    vector<int> v;
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);
    v.push_back(40);
    // 第一种遍历
    /*
    vector<int>::iterator itBegin = v.begin();
    vector<int>::iterator itEnd = v.end();
    while (itBegin != itEnd) {
        cout << *itBegin << endl;
        itBegin++;
    }
     */
    // 第二种遍历
    /*
    for (vector<int>::iterator itBegin = v.begin(); itBegin != v.end(); itBegin++) {
        cout << *itBegin << endl;
    }
     */
    // 第三种遍历
    for_each(v.begin(), v.end(), myPrint);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

2. 自定义类型

```c++
#include <vector>
#include <algorithm>

// 自定义数据类型
class Person {
public:
    Person(string name, int age) {
        this->name = name;
        this->age = age;
    }
    string name;
    int age;
};
void test01() {
    Person p1("lisi", 18);
    Person p2("zhaoliu", 29);
    Person p3("wangwu", 37);
    Person p4("zhangsan", 12);
    vector<Person> v;
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);
    for (vector<Person>::iterator it = v.begin(); it != v.end(); it++) {
        cout << "name: " << (*it).name << ", age: " << (*it).age << endl;
    }
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
} 
```

3. vector嵌套

```c++
#include <vector>
#include <algorithm>

// 嵌套
void test01() {
    vector<vector<int>> v;
    for (int i = 0; i < 4; i++) {
        vector<int> v1;
        for (int j = 0; j < 4; j++) {
            v1.push_back(i + j);
        }
        v.push_back(v1);
    }
    for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++) {
        for (vector<int>::iterator it2 = (*it).begin(); it2 != (*it).end(); it2++) {
            cout << *it2 << " ";
        }
        cout << endl;
    }
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## string容器

### 构造函数

```c++
// string 构造
void test01() {
    // 无参构造 创建一个空串
    string s1;
    cout << s1 << endl;
    // 使用 c string 初始化
    const char* cstr = "Hello World";
    string s2(cstr);
    cout << s2 << endl;
    // 拷贝构造
    string s3(s2);
    cout << s3 << endl;
    // 创建n个相同字符的字符串
    string s4(4, '3');
    cout << s4 << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

### 赋值操作

```c++
// string 赋值
void test01() {
    // = 赋值
    string s1;
    s1 = "Hello World";
    cout << s1 << endl;
    
    string s2;
    s2 = s1;
    cout << s2 << endl;
    
    string s3;
    s3 = 'b';
    cout << s3 << endl;
    
    // assign 赋值
    string s4;
    s4.assign("Hello World");
    cout << s4 << endl;
    
    string s5;
    s5.assign("Hello World", 3); // 把第一个参数的前3个赋值给s5
    cout << s5 << endl;
    
    string s6;
    s6.assign(s5);
    cout << s6 << endl;

    string s7;
    s7.assign(7, 'C');
    cout << s7 << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

### 拼接

```c++
void myPrint(string s) {
    cout << s << endl;
}
// string 拼接
void test01() {
    // += 拼接
    string s1;
    s1 += "Hello";
    myPrint(s1);
    
    s1 += 'W';
    myPrint(s1);
    
    s1 += s1;
    myPrint(s1);
    
    // append拼接
    s1.append(s1);
    myPrint(s1);
    
    s1.append("WANGWU");
    myPrint(s1);
    
    s1.append("LIST", 2); // 拼接字符串的前两个
    myPrint(s1);
    
    s1.append("123412341234", 2, 3); // 拼接从 2 号位置开始的 3 个字符
    myPrint(s1);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

### 查找和替换

```c++
void test01() {
    string s1 = "Hello Hello World";
    // 查找str 从第一个位置开始(包含这个位置) 没找到返回-1
    long ret = s1.find("Hello", 1); // 6
    cout << ret << endl;
    // 查找 char 从第一个位置开始 没找到返回-1
    ret = s1.find('e', 1); // 1
    cout << ret << endl;
    // 从s1的第二个位置开始，查找目标字符串中前4个字符第一次出现的位置
    ret = s1.find("ello23", 2, 4); // 7
    cout << ret << endl;
    // rfind是从右往左开始查找 从s1.size()-1开始往左查找第一个出现的位置
    ret = s1.rfind("o", s1.size()-1); // 13
    cout << ret << endl;
    // 从s1的s1.size()-1开始往右查找 目标字符串的前4个字符第一次出现的位置
    ret = s1.rfind("ello23", s1.size()-1, 4); // 7
    cout << ret << endl;
    // 替换
    // 将s1中从3开始连续两个字符替换为目标字符
    string rep = s1.replace(3, 2, "123123123132"); // Hel123123123132 Hello World
    cout << s1 << endl;
    cout << rep << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

### 比较

* 比较字符串的ASCII码

```c++
void test01() {
    string s1 = "Dello";
    string s2 = "Dello";
    string s3 = "Fello";
    string s4 = "Aello";

    int ret = s1.compare(s2); // 0 相等
    cout << ret << endl;
    ret = s1.compare(s3); // -2 D的ASCII小F两个值
    cout << ret << endl;
    ret = s1.compare(s4); // 3 D的ASCII大A三个值
    cout << ret << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

### 访问或者修改单个字符串中的单个字符

```c++
void test01() {
    string s1 = "Hello";
    cout << s1[2] << endl; // l
    cout << s1.at(3) << endl; // l
    s1[0] = 'X'; // Xello
    cout << s1 << endl;
    s1.at(4) = 'Y'; // XellY
    cout << s1 << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

### 插入和删除

```c++
void test01() {
    string s1 = "Hello World";
    // 在第一个位置插入 "AA"
    s1.insert(1, "AA"); // HAAello World
    cout << s1 << endl;
    // 在第四个位置插入4个'C'
    s1.insert(4, 4, 'C'); // HAAeCCCCllo World
    cout << s1 << endl;
    // 删除从1开始的两个字符
    s1.erase(1, 2); // HeCCCCllo World
    cout << s1 << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

### 截取子串

```c++
void test01() {
    string s1 = "Hello World";
    // s1中从1开始，4个长度的子串
    string sub = s1.substr(1,4); // ello
    cout << s1 << endl;
    cout << sub << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## vector

