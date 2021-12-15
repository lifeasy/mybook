# list

## 概念

* 双向循环链表

## 构造函数

```c++
void showList(list<int>& l) {
    for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    list<int> l;
    l.push_back(11);
    l.push_back(12);
    l.push_back(13);
    l.push_back(14);
    l.push_back(15);
    showList(l);
    // assign赋值
  	// 区间赋值
  	
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 赋值

```c++
void showList(list<int>& l) {
    for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    list<int> l;
    l.push_back(11);
    l.push_back(12);
    l.push_back(13);
    l.push_back(14);
    l.push_back(15);
    showList(l);
    // assign 区间赋值
    list<int> l1;
    l1.assign(l.begin(),l.end());
    showList(l1);
    // assign n个elem赋值
    list<int> l2;
    l2.assign(5,5);
    showList(l2);
    // = 赋值
    list<int> l3 = l2;
    showList(l3);
    // 将list与本身的元素互换
    swap(l, l3);
    showList(l);
    showList(l3);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 大小操作

```c++
void showList(list<int>& l) {
    for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    list<int> l;
    l.push_back(11);
    l.push_back(12);
    l.push_back(13);
    l.push_back(14);
    l.push_back(15);
    showList(l);
    // 判空
    if (l.empty()) {
        cout << "list is empty" << endl;
    } else {
        cout << "list size is " << l.size() << endl;
    }
    // resize
    l.resize(7);
    showList(l);
    l.resize(10, 88);
    showList(l);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 插入删除

```c++
void showList(list<int>& l) {
    for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    list<int> l;
    l.push_back(1);
    l.push_back(2);
    l.push_front(-1);
    l.push_front(-2);
    showList(l);
    l.pop_back();
    l.pop_front();
    showList(l);
    // insert
    l.insert(l.begin(), -2);
    showList(l);
    l.insert(l.begin(), 3, -3);
    showList(l);
    l.insert(l.begin(), l.begin(), l.end());
    showList(l);
    l.erase(l.begin());
    showList(l);
    l.erase(l.begin(),++l.begin());
    showList(l);
    // 删除所有与 elem相同的节点
    l.remove(-3);
    showList(l);
    l.clear();
    showList(l);
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 数据存取

```c++
void showList(list<int>& l) {
    for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

void test01() {
    // 默认构造
    list<int> l;
    l.push_back(1);
    l.push_back(2);
    l.push_front(-1);
    l.push_front(-2);
    showList(l);
    cout << l.front() << endl;
    cout << l.back() << endl;
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

## 翻转和排序

```c++
void showList(list<int>& l) {
    for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
    cout << endl;
}

bool mySort(int a, int b) {
    return a > b;
}
void test01() {
    // 默认构造
    list<int> l;
    l.push_back(1);
    l.push_back(2);
    l.push_front(-1);
    l.push_front(-2);
    showList(l);
    l.reverse();
    showList(l);
    // 全局函数的sort不支持非随机访问迭代器的排序
    // 默认升序
    l.sort();
    showList(l);
    // 降序
    l.sort(mySort);
    showList(l);
    
}

int main(int argc, const char * argv[]) {
    test01();
    return 0;
}
```

