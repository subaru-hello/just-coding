### 進捗管理ひょう

https://docs.google.com/spreadsheets/d/1PagcdZnm_VD1wX7kqpK3XL1hF7puu9Fn27W00ie-Udo/edit?gid=0#gid=0

### C ++ Basic Code Sample

[基本的なメソッド](https://www.notion.so/e70efe56464c4f2c83861619e5777e3a?pvs=21) 

- tuple（）

a data structure that contains a fixed number of elements, immutable.

```cpp

```

- pair

固定長で２つの値を保持する時に使う

```cpp
pair<int, int>
```

how to access

```cpp
pair<int, int> p
p.first
p.second
```

- auto（型推論）

lambda関数のようにコンパイラーが返却型を決める関数の場合、autoを使って型推論させるといい。

```cpp
auto comp = [](pair<int, int>& a, pair<int, int>& b) {
  return a.second > b.second;  // Comparator for min-heap behavior
};
```

immutabilityを保証したい時やreassignを防ぎたい場合は、constを使う。

constを使う場合

```cpp
const auto comp = [](const pair<int, int>& a, const pair<int, int>& b) {
  return a.second > b.second;  // Comparator for min-heap behavior
};
```

- vector<type>（配列）

- heap（ヒープ木）

min heap.  a parent node is always smaller than children node.

- The left child is located at index `2 * i + 1`.
- The right child is located at index `2 * i + 2`.
- The parent of any node at index `i` is located at index `(i - 1) / 2`.

```cpp
// create a heap. and push back nums in max heap.
priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(comp)> heap(comp);

for(auto& entry: counter) {
    heap.push({entry.first, entry.second});
};
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/d754495e-9e0f-4a9f-90a3-d9033bf94c16/a0876b4a-bb32-4b04-9a7f-02cbe1260dc3/image.png)

- cout（入力受取）

- unordered_map

**Hash Table-Based Implementation**: `unordered_map` uses a hash table for storing its elements. This provides **average constant-time complexity, O(1),** for search, insertion, and deletion operations, compared to `O(log n)` in `std::map` which uses a balanced binary tree (usually a Red-Black tree

```cpp
    // Iterate over the unordered_map
    for (const auto& pair : ageMap) {
        std::cout << pair.first << " is " << pair.second << " years old." << std::endl;
    }
```

**Without `const` Reference**:

```cpp
cppCopy code
for (auto pair : ageMap) { /*...*/ }
```

- Here, `pair` is a copy of each element in `ageMap`. This involves copying each `std::pair<const std::string, int>`, which can be expensive if the elements are large or if the map has many elements. Also, if you accidentally modify `pair`, it won't affect the original map, which could be both a benefit or a drawback depending on the use case.
- **Using Just a Reference (`auto& pair`)**:
    
    ```cpp
    cppCopy code
    for (auto& pair : ageMap) { /*...*/ }
    ```
    
    - Here, `pair` is a non-constant reference to each element. This allows modification of the elements in `ageMap`. It is efficient (no copying), but there is a risk of accidentally modifying the map's contents when only read access is intended.
- **Explicit Type with `const std::pair<const std::string, int>&`**:
    
    ```cpp
    cppCopy code
    for (const std::pair<const std::string, int>& pair : ageMap) { /*...*/ }
    
    ```
    
    - This is the most explicit version where you write out the entire type. It achieves the same result as `const auto& pair` but is more verbose. Using `auto` keeps the code cleaner and reduces the chance of errors if the type changes in the future.

- set

https://cpprefjp.github.io/reference/set/set/insert.html

unordered_setを使ってuniqueな値を保持することができる。

```cpp
[pair](https://cpprefjp.github.io/reference/utility/pair.html)<iterator,bool> insert(**const** value_type& x); // (1)
[pair](https://cpprefjp.github.io/reference/utility/pair.html)<iterator,bool> insert(value_type&& y);
vector<int,int>& nums1;
unordered_set<int> unique_num1(nums1.begin(), nums1.end());
        for(int x: nums2) {
            if(unique_num1.count(x)){
                ans.insert(x);
            }
        }
```
