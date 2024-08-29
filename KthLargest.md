## 問題

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of test scores `nums`.
- `int add(int val)` Adds a new test score `val` to the stream and returns the element representing the `kth` largest element in the pool of test scores so far.

https://leetcode.com/problems/kth-largest-element-in-a-stream/

## Intuition

保存先の上位k番目を取得するアルゴリズムを構築する。

minHeapを使うと

initializeにO(N log N)

addは追加と検索なのでO(N)

quickSortを使うと

initializeにO(1)

addは追加と検索なのでO(N), 最悪(N*2)

## 方針

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

minHeap

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

quick select

vectorだとtime exceededになってしまった。

```cpp
class KthLargest {
public:
int k;
vector<int> nums;
priority_queue<int, vector<int>, greater<int>> pq;
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        this->nums = nums;
    }
    
    int add(int val) {
        nums.push_back(val);
        return quickSelect(nums, 0, nums.size() -1, k -1);
    }

private:
    // 降順にswapしながらソートしていく
    int partition(vector<int>& arr, int left, int right) {
        int pivot = arr[right];
        int i = left; 
        // 左から右にtraverseしていってpivotより大きければswap
        for (int j = left; j < right; j++) {
            // 降順に並び替え
            if(arr[j] > pivot) {
                swap(arr[i], arr[j]);
                i++;
            }
        }
        swap(arr[i], arr[right]); //右端と左端を交換
        return i;
    }

    int quickSelect(vector<int>& arr, int left, int right, int k){
        if(left == right) {
            return arr[left];
        }

        int pivotIndex = partition(arr, left, right);

        if(pivotIndex == k) {
            return arr[pivotIndex];
        }else if (pivotIndex > k){
            return quickSelect(arr, left, pivotIndex - 1, k);
        } else {
            return quickSelect(arr, pivotIndex + 1, right, k);
        }
    }
};
/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```
