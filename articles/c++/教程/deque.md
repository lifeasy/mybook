# deque

## 功能

* 双端数组，可对头部进行插入删除操作
* 头部操作比vector快
* vector随机访问比deque快

## 底层原理

* 中控器 + 缓冲区

* 内部有一个中控器，中控器维护了n段连续的存储缓冲区，使得deque看上去像一个连续的存储空间

## 构造

```c++
// 参数const 防止 d被重新复制
void showDeque(const deque<int>& d) {
    // 常量迭代器 防止 *it被修改
    for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    deque<int> d;
    for (int i = 0; i < 10; i++) {
        // 头插
        d.push_front(i);
    }
    showDeque(d);
    // 区间构造
    deque<int> d1(d.begin()+1, d.end());
    showDeque(d1);
    // n和elem构造
    deque<int> d2(5, 66);
    showDeque(d2);
    // 拷贝构造
    deque<int> d3(d2);
    showDeque(d3);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 赋值

```c++
// 参数const 防止 d被重新复制
void showDeque(const deque<int>& d) {
    // 常量迭代器 防止 *it被修改
    for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    deque<int> d;
    for (int i = 0; i < 10; i++) {
        // 头插
        d.push_front(i);
    }
    showDeque(d);
    // = 赋值
    deque<int> d1 = d;
    showDeque(d1);
    
    // assign 区间
    deque<int> d2;
    d2.assign(d1.begin(), d1.end());
    showDeque(d2);
    
    // assign n 个 elem
    deque<int> d3;
    d3.assign(4, 44);
    showDeque(d3);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 大小操作

```c++
// 参数const 防止 d被重新复制
void showDeque(const deque<int>& d) {
    // 常量迭代器 防止 *it被修改
    for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    deque<int> d;
    for (int i = 0; i < 5; i++) {
        // 头插
        d.push_front(i);
    }
    showDeque(d);
    // 判空
    if (d.empty()) {
        cout << "deque is empty" << endl;
    } else {
        cout << "deque is not empty" << endl;
    }
    // 大小
    cout << d.size() << endl;
    // resize 超出空间默认补0
    d.resize(8);
    showDeque(d);
    // resize 超出空间补指定的值
    d.resize(10, -1);
    showDeque(d);
    
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 插入和删除

```c++
// 参数const 防止 d被重新复制
void showDeque(const deque<int>& d) {
    // 常量迭代器 防止 *it被修改
    for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    deque<int> d;
    // 头尾操作
    d.push_back(9);
    d.push_back(8);
    showDeque(d);
    d.push_front(-1);
    d.push_front(-2);
    showDeque(d);
    d.pop_back();
    d.pop_front();
    showDeque(d);
    // 指定位置操作
    // 指定位置插入
    d.insert(d.begin(), 66);
    showDeque(d);
    // 指定位置插入 n 和 elem
    d.insert(d.begin(), 2, 99);
    showDeque(d);
    // 指定位置插入 区间数据
    d.insert(d.begin(), d.begin(), d.end());
    showDeque(d);
    // 删除数据
    // 删除指定位置
    d.erase(d.begin());
    showDeque(d);
    // 删除指定区间
    d.erase(d.begin() + 2, d.end() - 2);
    showDeque(d);
    // 清空
    d.clear();
    showDeque(d);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 数据存取

```c++
// 参数const 防止 d被重新复制
void showDeque(const deque<int>& d) {
    // 常量迭代器 防止 *it被修改
    for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    deque<int> d;
    for (int i = 0; i < 5; i++) {
        d.push_back(i);
    }
    showDeque(d);
    // at 访问
    cout << d.at(1) << endl;
    d.at(1) = 99;
    cout << d.at(1) << endl;
    // [] 访问
    cout << d[1] << endl;
    d[1] = 88;
    cout << d[1] << endl;
    // front访问
    cout << d.front() << endl;
    // back访问
    cout << d.back() << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 排序

```c++
// 参数const 防止 d被重新复制
void showDeque(const deque<int>& d) {
    // 常量迭代器 防止 *it被修改
    for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    deque<int> d;
    for (int i = 9; i > 3; i--) {
        d.push_back(i);
    }
    showDeque(d);
    // 支持随机访问迭代器的容器，都可以使用sort进行排序
    sort(d.begin(), d.end());
    showDeque(d);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

