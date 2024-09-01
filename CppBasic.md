### C ++ Basic Code Sample

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

- cout（入力受取）
