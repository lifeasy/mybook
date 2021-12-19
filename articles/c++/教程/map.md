# map

## 概念

* map中所有的元素都是pair
* pair中的第一个元素为key，起到索引作用，第二个值为value
* 所有元素都会根据key进行升序排序 
* 关联式容器，底层使用二叉树实现
* map中不允许有重复key，multimap允许有重复key
* 插入数据时，使用对组

## 构造和复制

```c++
void showMap(map<int, int>& m) {
    for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
        cout << "key is " << it->first << ", value is " << it->second << endl;
    }
    cout << endl;
}

void test01() {
    // 默认构造
    map<int, int> m;
    m.insert(pair<int, int>(1,10));
    m.insert(pair<int, int>(2,20));
    m.insert(pair<int, int>(3,30));
    m.insert(pair<int, int>(4,40));
    showMap(m);
    // 拷贝构造
    map<int, int> m1(m);
    showMap(m1);
    // 赋值
    map<int, int> m2 = m;
    showMap(m2);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 大小和交换

* size()返回大小
* empty()判空
* swap(st)交换两个容器

## 插入和删除

* insert(elem) 插入元素

```c++
void showMap(map<int, int>& m) {
    for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
        cout << "key is " << it->first << ", value is " << it->second << endl;
    }
    cout << endl;
}

void test01() {
    // 默认构造
    map<int, int> m;
    m.insert(pair<int, int>(1,10));
    m.insert(make_pair(2, 20));
    m.insert(map<int, int>::value_type(3, 30));
    m[4] = 40; // 如果key之前存在，会直接修改之前的值，但是上面的方法不会修改之前的值，会修改失败
    // 使用[]访问一个不存在的key是，会直接赋值0
    cout << m[5] << endl; // key is 5, value is 0
    showMap(m);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

* clear() 删除所有元素
* erase(pos) 删除迭代器所指的元素，返回下一个元素的迭代器
* erase(begin,end) 删除迭代器区间，前闭后开，返回下一个元素的迭代器
* earse(key) 删除容器中为key的元素

## 查找和统计

* find(key) 查找key是否存在，若存在，返回这个key的迭代器，不存在返回map.end迭代器
* count(key) 统计key的元素个数，map中为0或者1，multimap可能存在多个

## 排序

* 默认按照key升序排列
* 利用仿函数，可以改变排序规则

```c++
class MyCompare {
public:
  // 注意const
    bool operator()(int a, int b) const{
        return a > b;
    }
};

void showMap(map<int, int>& m) {
    for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
        cout << "key is " << it->first << ", value is " << it->second << endl;
    }
    cout << endl;
}

void test01() {
    // 默认构造
    map<int, int> m;
    m.insert(pair<int, int>(1,10));
    m.insert(pair<int, int>(2,20));
    m.insert(pair<int, int>(4,40));
    m.insert(pair<int, int>(3,30));
    m.insert(pair<int, int>(5,50));
    // 默认升序
    showMap(m);
    // 
    map<int, int, MyCompare> m1;
    m1.insert(make_pair(1,10));
    m1.insert(make_pair(2,20));
    m1.insert(make_pair(4,40));
    m1.insert(make_pair(3,30));
    m1.insert(make_pair(5,50));
    for (map<int, int, MyCompare>::iterator it = m1.begin(); it != m1.end(); it++) {
        cout << "key is " << it->first << ", value is " << it->second << endl;
    }
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

