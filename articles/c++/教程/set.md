# set/multiset

## 基本概念

* 属于关联容器，底层二叉树结构
* 所有元素在插入是会自动排序
* set中不允许有重复元素，multiset允许有重复元素

## set构造和赋值

```c++
void showSet(set<int>& s) {
    for (set<int>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    set<int> s1;
    s1.insert(30);
    s1.insert(10);
    s1.insert(20);
    s1.insert(10);
    // 10 20 30 不论插入先后顺序，遍历输出是有序的
    showSet(s1);
    // 拷贝构造
    set<int> s2(s1);
    showSet(s2);
    // 赋值
    set<int> s3;
    s3 = s2;
    showSet(s3); 
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## set大小和交换

```c++
void showSet(set<int>& s) {
    for (set<int>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    set<int> s1;
    s1.insert(30);
    s1.insert(10);
    s1.insert(20);
    s1.insert(10);
    showSet(s1);
    // 判空
    if (s1.empty()) {
        cout << "set is empty" << endl;
    } else {
        cout << "set size is " << s1.size() << endl;
    }
    // 交换
    set<int> s2;
    s2.insert(77);
    s2.insert(99);
    s2.insert(66);
    s2.insert(33);
    showSet(s2);
    swap(s1, s2);
    showSet(s1);
    showSet(s2);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## set插入和删除

```c++
void showSet(set<int>& s) {
    for (set<int>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    set<int> s1;
    // 插入
    s1.insert(30);
    s1.insert(10);
    s1.insert(20);
    showSet(s1);
    // 删除指定位置
    s1.erase(++s1.begin());
    showSet(s1);
    // 删除指定区间
    s1.erase(s1.begin(),s1.end());
    showSet(s1);
    s1.insert(30);
    s1.insert(10);
    s1.insert(20);
    // 删除指定元素
    s1.erase(20);
    showSet(s1);
    // 清空
    s1.clear();
    showSet(s1);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## set查找和统计

```c++
void showSet(set<int>& s) {
    for (set<int>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    set<int> s1;
    // 插入
    s1.insert(30);
    s1.insert(10);
    s1.insert(20);
    s1.insert(20);
    s1.insert(20);
    s1.insert(20);
    showSet(s1);
    // find 查找
    set<int>::iterator it = s1.find(30);
    // 找不到就返回 s1.end()迭代器
    if (it != s1.end()) {
        cout << *it << endl;
    }
    // count 统计元素个数 对set而言，结果无非就是 0 或者 1
    cout << s1.count(20) << endl;
    
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## set和multiset区别

* set不可以有重复数据，multiset可以
* set插入数据会返回插入结果，表示是否插入成功，返回一个对组`pair<iterator,bool>`
* multiset不会校验插入数据，可以重复插入，返回一个迭代器，代表插入位置

```c++
void showSet(set<int>& s) {
    for (set<int>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    set<int> s1;
    pair<set<int>::iterator, bool> ret = s1.insert(10);
    if (ret.second) {
        cout << "插入" << *ret.first << "成功" << endl;
    } else {
        cout << "插入" << *ret.first << "失败" << endl;
    }
    ret = s1.insert(10);
    if (ret.second) {
        cout << "插入" << *ret.first << "成功" << endl;
    } else {
        cout << "插入" << *ret.first << "失败" << endl;
    }
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 对组pair<T1,T2>

```c++
void showSet(set<int>& s) {
    for (set<int>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    pair<string, int> p1("lisi",20);
    cout << p1.first << "," << p1.second << endl;
    pair<string, int> p2 = make_pair("wangwu", 18);
    cout << p2.first << "," << p2.second << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## set排序

* 默认升序
* 可以使用仿函数进行排序

```c++
void showSet(set<int>& s) {
    for (set<int>::iterator it = s.begin(); it != s.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}
class MyCompare {
public:
    bool operator()(int a, int b) {
        return a > b;
    }
};

void test01() {
    set<int> s1;
    s1.insert(99);
    s1.insert(44);
    s1.insert(66);
    s1.insert(11);
    s1.insert(55);
    showSet(s1); // 默认升序
    // set没有sort函数，需要使用仿函数进行排序，需要在定义的时候就传入
    set<int, MyCompare> s2;
    s2.insert(99);
    s2.insert(44);
    s2.insert(66);
    s2.insert(11);
    s2.insert(55);
    for (set<int, MyCompare>::iterator it = s2.begin(); it != s2.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}

```

* 自定义数据排序：必须指定排序规则

```c++
class Person {
public:
    Person(string name, int age) {
        this->name = name;
        this->age = age;
    }
    bool operator()(Person& p1, Person& p2) {
        return p1.age > p2.age;
    }
    
    string name;
    int age;
};
class MyCompare {
public:
    bool operator()(const Person& p1, const Person& p2) {
        return p1.age > p2.age;
    }
};

void test01() {
    Person p1("张三", 23);
    Person p2("李四", 24);
    Person p3("王五", 25);
    set<Person, MyCompare> s1;
    s1.insert(p1);
    s1.insert(p2);
    s1.insert(p3);
    for (set<Person, MyCompare>::iterator it = s1.begin(); it != s1.end(); it++) {
        cout << it->name << "," << it->age << endl;
    }
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

