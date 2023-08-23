# std::vector

## Basic

- random access with `[]` & `at()`
- 添加 `insert()`/刪除 `erase()` 除了尾端之外的元素都非常耗時
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

## Traversal

[], at()

- `[]`: out of range 時不拋錯，產生 _undefined brhavior_
- `at()`: 有 bound-checked，會拋 _out_of_range exception_

```cpp
    for (int i = 0; i < v1.size(); i++) {
        cout << v1[i] << " ";
        cout << v1.at(i) << "\n";
    }
```

iterator

```cpp
    for (vector<int>::iterator it = v1.begin(); it != v1.end(); it++) {
        cout << *it << " ";
    }
```

reverse_iterator

```cpp
    for (vector<int>::reverse_iterator it = v1.rbegin(); it != v1.rend(); it++) {
        cout << *it << " ";
    }
```

## `erase` Element

- 除了 erase 尾端的元素，其他都很慢
- 回傳：An _iterator pointing to the new location_ of the element that _followed the last element erased by the function call_.

### Single Element (position)

> iterator erase (iterator position);

```cpp
    v1.erase(v1.begin() + 3); // 刪第4個
```

### A Range of Elements [first,last)

> iterator erase (iterator first, iterator last);

```cpp
    v1.erase(v1.begin(), v1.begin() + 3); // 刪前3個
```

### 刪特定 value 的元素

```cpp
int main(int argc, const char * argv[]) {

    int myints[] = { 1, 2, 3, 2, 1 };
    std::vector<int> v1(myints, myints + sizeof(myints) / sizeof(int));

    // 删 value = 2 的 element
    vector<int>::iterator it = v1.begin();
    while (it != v1.end()) {
        if (*it == 2) {
            it = v1.erase(it); // 刪除後會回傳下一個位置的 pointer，所以不需要 it++
        } else {
            it++;
        }
    }
    return 0;
}
```

## `insert` Element

### Single Element

> iterator insert (iterator position, const value_type& val); \

```cpp
    // [0,1,2,3,4,5,6,7,8,9]
    v1.insert(v1.begin() + 3, 10); // [0,1,2,10,3,4,5,6,7,8,9]
```

#### 2. Fill with Size

- 除了 insert 尾端的元素，其他都很慢

> void insert (iterator position, size_type n, const value_type& val);

```cpp
    // [0,1,2,3,4,5,6,7,8,9]
    v1.insert(v1.begin()+2, 3, 11);   // [0,1,11,11,11,2,3,4,5,6,7,8,9]
```

### 3. range

> template <class InputIterator> void insert (iterator position, InputIterator first, InputIterator last);

```cpp
    // [0,1,2,3,4,5,6,7,8,9]
    int myarray [] = { 10, 11, 12 };
    v1.insert(v1.begin(), myarray, myarray+3); // [10,11,12,0,1,2,3,4,5,6,7,8,9]
```
