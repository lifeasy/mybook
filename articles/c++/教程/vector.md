# STL

## vector

* 动态扩展

## 构造函数

```c++
void printVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    vector<int> v;
    for (int i = 0; i < 10; i++) {
        v.push_back(i);
    }
    printVector(v); // 0 1 2 3 4 5 6 7 8 9
    
    // 区间构造
    vector<int> v1(v.begin(), v.end());
    printVector(v1); // 0 1 2 3 4 5 6 7 8 9
    
    // n个element 构造
    vector<int> v2(5,100);
    printVector(v2); // 100 100 100 100 100

    // 拷贝构造
    vector<int> v3(v2);
    printVector(v3); // 100 100 100 100 100
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 赋值

```c++
void printVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    vector<int> v;
    for (int i = 0; i < 5; i++) {
        v.push_back(i);
    }
    printVector(v);
    
    // = 赋值
    vector<int> v1;
    v1 = v;
    printVector(v1);
    
    // assign 区间赋值
    vector<int> v2;
    v2.assign(v.begin(), v.end());
    printVector(v2);
    
    // assign n个elem
    vector<int> v3;
    v3.assign(5, 1);
    printVector(v3);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 容量和大小

* resize不改变容量

```c++
void printVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    vector<int> v;
    for (int i = 0; i < 5; i++) {
        v.push_back(i+3);
    }
    printVector(v);
    
    // 判断空
    if (v.empty()) {
        cout << "v is empty" << endl;
    } else {
        cout << "v is not empty" << endl;
    }
    
    // 容量
    cout << "容量：" << v.capacity() << endl;
    
    // 元素个数
    cout << "元素个数：" << v.size() << endl;
    
    // 重新设定容器长度 如果变长，以默认值填充(默认0)，如果变短，删除超出部分
    v.resize(6);
    printVector(v);
    
    // 重新设定容器长度 如果变长，以给定值填充，如果变短，删除超出部分
    v.resize(9, 66);
    printVector(v);
    
    v.resize(3);
    printVector(v);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 插入和删除

```c++
void printVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    vector<int> v;
    for (int i = 0; i < 5; i++) {
        v.push_back(i+3);
    }
    printVector(v);
    
    // 尾插入
    v.push_back(30);
    printVector(v);
    // 尾删除
    v.pop_back();
    printVector(v);
    // 指定位置插入
    v.insert(v.begin() + 2, 3);
    printVector(v);
    // 指定位置插入n个elem
    v.insert(v.begin() + 3, 5, -1);
    printVector(v);
    // 指定位置删除
    v.erase(v.end() - 3);
    printVector(v);
    // 删除指定区间
    v.erase(v.begin() + 2, v.begin() + 6);
    printVector(v);
    // 清空
    v.clear();
    printVector(v);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 数据存取

```c++
void printVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void printInt(int a) {
    cout << a << endl;
}

void test01() {
    vector<int> v;
    for (int i = 0; i < 5; i++) {
        v.push_back(i+3);
    }
    printVector(v);
    
    // at
    printInt(v.at(2));
    v.at(2) = 10;
    printInt(v.at(2));
    
    // []
    printInt(v[4]);
    v[4] = 99;
    printInt(v[4]);
    
    // front
    printInt(v.front());
    
    // back
    printInt(v.back());
    
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 容器互换

```c++
void printVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    vector<int> v;
    for (int i = 0; i < 5; i++) {
        v.push_back(i+3);
    }
    printVector(v);
    
    vector<int> v1;
    for (int i = 3; i > 0; i--) {
        v1.push_back(i + 3);
    }
    printVector(v1);
    
    v.swap(v1); // 容器交换
    printVector(v);
    printVector(v1);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

* 实际用途：巧用swap可以收缩内存空间

```c++
void printVector(vector<int>& v) {
    for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    vector<int> v;
    for (int i = 0; i < 10000; i++) {
        v.push_back(i);
    }
    cout << "容量：" << v.capacity() << endl; // 16384
    cout << "大小：" << v.size() << endl; // 10000
    
    // resize不改变容量 造成空间浪费
    v.resize(3);
    cout << "容量：" << v.capacity() << endl; // 16384
    cout << "大小：" << v.size() << endl; // 3
    
    // 巧用swap收缩空间 这里通过一个匿名对象和原始vector交换空间，从而改变容量，而匿名对象也在交换完成后释放
    vector<int>(v).swap(v);
    cout << "容量：" << v.capacity() << endl; // 3
    cout << "大小：" << v.size() << endl; // 3
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}  
```

## 预留空间

```c++
void test01() {
    vector<int> v;
    v.reserve(10000);
    int *p = NULL;
    int num = 0;
    for (int i = 0; i < 10000; i++) {
        v.push_back(i);
        if (p != &v[0]) {
            p = &v[0];
            num++;
        }
    }
    cout << "开辟次数：" << num << endl; // 15 -> 1
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

