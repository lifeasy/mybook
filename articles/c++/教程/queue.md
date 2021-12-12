# queue

## 功能

* 先进先出
* 没有迭代器，不支持遍历，随机访问

## 构造

```c++
void test01() {
    // 默认构造
    queue<int> q;
    q.push(10);
    q.push(11);
    cout << q.size() << endl;

    // 拷贝构造
    queue<int> q1(q);
    cout << q1.size() << endl;
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
    queue<int> q;
    q.push(10);
    q.push(11);
    cout << q.size() << endl;

    // = 赋值
    queue<int> q1;
    q1 = q;
    cout << q1.size() << endl;
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
    queue<int> q;
    // 入队
    q.push(11);
    q.push(12);
    // 出队
    q.pop();
    // 获取队头
    cout << q.front() << endl;
    // 获取队尾
    cout << q.back() << endl;
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
    queue<int> q;
    // 入栈
    q.push(10);
    q.push(11);
    q.push(12);
    cout << (q.empty() ? "empty" : "not empty") << endl;
    cout << q.size() << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

