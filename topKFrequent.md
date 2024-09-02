## 問題

https://leetcode.com/problems/top-k-frequent-elements/description/

## 概要

heapに出てきた回数と値を昇順で入れる。

(frequency, number)
(3, 20)
(2, 10)
(1, 30)

heapはmin heap(降順)がデフォルトで、pop()をしたら回数が少ないものをとってしまう。

なので、max heapになるようにheapへ昇順にする関数を渡す

### 解法

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        // create unorderd map to hold counter and value pair
        unordered_map<int, int> counter;

        for(int n: nums) {
            counter[n]++;
        };
        // create comp lambda function to reorder the heap by frequency

        auto comp = [](pair<int, int>& a, pair<int, int>& b){
           return a.second < b.second;
        };
        // create max heap. each node has a pair with counter. counter describe the frequency.
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(comp)>heap(comp);

        for (auto&entry : counter) {
            heap.push({entry.first, entry.second});
        };
        // extract kth frequent values until k length reach to 0 from the heap
        vector<int> res;
        while(k-- > 0){
            res.push_back({heap.top().first});
            heap.pop();
        };

        // return res
        return res;
    }
};
```
