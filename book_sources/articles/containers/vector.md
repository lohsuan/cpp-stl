# std::vector

## Basic

- random access with `[]` & `at()`
- 添加 `insert()`/刪除 `erase` 除了尾端之外的 element 都非常耗時
  - relocate all the elements after the segment erased to their new positions
- `push_back()`
- `pop_back()`

## Initialize

```cpp
#include <vector>

int main(int argc, const char * argv[]) {
    // (1) empty container constructor (default constructor)
    std::vector<int> v1;
    v1.push_back(1);
    v1.push_back(2);

    // (2) range constructor: [first,last)
    std::vector<int> v2(v1.begin(), v1.end());
    int myints[] = {1,2,1,2};
    std::vector<int> v3(myints, myints + sizeof(myints) / sizeof(int));

    // (3) fill constructor: 3 int with value 9
    vector<int> v4(3,9);

    // (4) copy constructor
    vector<int> v5 = v4;

    return 0;
}

```

## Iterators

- `[]`: out of range 時不拋錯，產生 _undefined brhavior_
- `at()`: 有 bound-checked，會拋 _out_of_range exception_

```cpp
int main(int argc, const char * argv[]) {

    vector<int> v1;
    for (int i = 0; i < 10; i++) v1.push_back(i);

    // [], at()
    for (int i = 0; i < v1.size(); i++) {
        cout << v1[i] << " ";
        cout << v1.at(i) << "\n";
    }

    // iterator
    for (vector<int>::iterator it = v1.begin(); it != v1.end(); it++) {
        cout << *it << " ";
    }

    // reverse_iterator
    for (vector<int>::reverse_iterator it = v1.rbegin(); it != v1.rend(); it++) {
        cout << *it << " ";
    }

    return 0;
}
```

## `erase` element

### Removes a single element (position)

> iterator erase (iterator position); \

### Remove a range of elements [first,last)

> iterator erase (iterator first, iterator last);

---

- 除了 erase 尾端的元素，其他都很慢
- 回傳：An iterator pointing to the new location of the element that followed the last element erased by the function call.

```cpp
int main(int argc, const char * argv[]) {

    vector<int> v1;
    for (int i = 1; i < 9; i++) v1.push_back(i); // [1,2,3,4,5,6,7,8]

    v1.erase(v1.begin() +3); // 刪第4個: [1,2,3,5,6,7,8]
    v1.erase(v1.begin(), v1.begin() + 3); // 刪前3個: [5,6,7,8]

    v1.push_back(2);
    v1.push_back(2);

    // 删 value = 2 的 element
    vector<int>::iterator it = v1.begin();
    while (it != v1.end()) {
        if (*it == 2) {
            it = v1.erase(it); // 刪除後會移到下一個位置，所以不需要 it++
        } else {
            it++;
        }
    }
    return 0;
}
```

## `insert` element

### 1. single element

> iterator insert (iterator position, const value_type& val); \

### 2. fill with size

> void insert (iterator position, size_type n, const value_type& val);

### 3. range

> template <class InputIterator> void insert (iterator position, InputIterator first, InputIterator last);

---

- 除了 insert 尾端的元素，其他都很慢

```cpp
int main(int argc, const char * argv[]) {

    vector<int> v1;
    for (int i = 1; i < 9; i++) v1.push_back(i); // [1,2,3,4,5,6,7,8]

    // (1) single element
    v1.insert(v1.begin() + 3, 10);  // [1,2,3,10,4,5,6,7,8]
    // (2) fill
    v1.insert(v1.begin(), 3, 11);   // [11,11,11,1,2,3,10,4,5,6,7,8]
    // (3) range
    int myarray [] = { 20,21,22 };
    v1.insert (v1.begin(), myarray, myarray+3);

    return 0;
}
```
