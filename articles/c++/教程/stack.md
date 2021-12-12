# stack

## 功能

* 先进后出
* 没有迭代器，不支持遍历，随机访问

## 构造

```c++
void test01() {
    // 默认构造
    stack<int> s;
    s.push(10);
    s.push(11);
    cout << s.size() << endl;

    // 拷贝构造
    stack<int> s1(s);
    cout << s1.size() << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 赋值

```c++
void test01() {
    // 默认构造
    stack<int> s;
    s.push(10);
    s.push(11);
    cout << s.size() << endl;
    // = 赋值
    stack<int> s1;
    s1 = s;
    cout << s1.size() << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 数据存取

```c++
void test01() {
    // 默认构造
    stack<int> s;
    // 入栈
    s.push(10);
    s.push(11);
    s.push(12);
    // 弹栈
    s.pop();
    // 获取栈顶元素
    cout << s.top() << endl;
    
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 判断大小

```c++
void test01() {
    // 默认构造
    stack<int> s;
    // 入栈
    s.push(10);
    s.push(11);
    s.push(12);
    cout << (s.empty() ? "empty" : "not empty") << endl;
    cout << s.size() << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

