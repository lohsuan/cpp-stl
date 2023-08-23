# Container 比較整理

## `vector` vs `list`

- `vector` 封裝了 **array list**
- `list` 封裝了 **doubly linked list**
- 隨機存取：
  - `vector` 使用 **contiguous memory** -> 支援 **random access** `[]`/`at`。
  - `list` 非連續記憶體空間，不支持 random access `[]`。
- 插入/刪除：
  - `vector` 很慢 _O(N)_，因需要拷貝和移動數據，尤其是頭部，在尾部則很快。
  - `list` 很快 _O(1)_，只需要**改變指針的指向**，不需要拷貝和移動數據。O(1)

p.s. single linked list 的新增刪除還是 _O(N)_，因爲少 forward pointer，找前一個元素就必須從頭掃一次。

結論：常需要隨機訪問用 `vector`、常需要新增刪除中間數據用 `list`

## `map` vs `unordered_map`

- 存放 key-value pairs 的映射資料結構
- `map` 封裝了**紅黑樹**， Self balancing Binary Search Tree (BST)
- `unordered_map` 封裝了 **Hash Table**

|              | map                 | unordered_map                  |
| ------------ | ------------------- | ------------------------------ |
| ordering     | 有序 (key 由小到大) | 無序                           |
| search       | log(n)              | O(1) -> Average, O(n) -> Worst |
| insert/erase | log(n) + Rebalance  | same as search                 |

結論：需要鍵值排序用 `map`，需要搜尋資料、插入刪除用 `unordered_map`

## `set` vs `map`

- `set` 只存 key，`map` 則是存 key value pairs
- 都是紅黑樹 (Self-Balancing BST)

### [C++11] 內部實作是 hash table 的 `unordered_set/unordered_map`

### [C++11] 可擁有多個相同鍵值的 `multiset/multimap`

## `set` vs `vector`

- set 不允許重複數據

### [C++11] `std::array`

- 在編譯階段就決定好大小，事後不能修改

### [C++11] `std::forward_list`

- list 的單向版本（singly-linked list）
- 非連續的記憶體配置依序連結
- 比 `vector` 更快速地將新元素插入在任何位置、或把任何位置的元素移除
  - 雖然還是 O(N)，但省掉拷貝和移動數據的時間
- 沒有隨機存取的能力，要讀取中間的值會需要從頭開始找
