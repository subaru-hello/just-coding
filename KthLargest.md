## 問題

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of test scores `nums`.
- `int add(int val)` Adds a new test score `val` to the stream and returns the element representing the `kth` largest element in the pool of test scores so far.

https://leetcode.com/problems/kth-largest-element-in-a-stream/

## Intuition

vector内の数値を一つずつtraverseして、

渡されるvectorはsortedじゃない。

## 方針

priority queueを使う方法が最も手っ取り早い

priority queueの実装はここから [priority queue](https://cpprefjp.github.io/reference/queue/priority_queue.html)

追加：push()

取り出し：top()

```cpp
namespace std {
	template <class T,
						class Container = std::vector<T>,
						class COmpare = less<typename Container::value_type>>
	class priority_queue;
```

## 解法

```cpp
class KthLargest {
public:
    int k;
    
    priority_queue<int, vector<int>,greater<int>> pq; // initialize min heap which always contains kth largest.
    KthLargest(int k, vector<int>& nums) {
        this->k = k; // assign k to public variable k.
        for(int n: nums){
            pq.push(n);
            if(pq.size() > k) pq.pop();
        }
    }
    
    int add(int val) {
        pq.push(val);
        if(pq.size()>k)pq.pop();
        return pq.top();
    }
};

```
