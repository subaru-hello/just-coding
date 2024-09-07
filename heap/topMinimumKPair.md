## 概要

vector aとvector bのpairを作って小さい方からkこ返す。

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/d754495e-9e0f-4a9f-90a3-d9033bf94c16/342e2925-00dd-4698-ae8a-04d96d522cd9/image.png)

## Intuition

pairのmin heapを作って、kが0になるまで上からk個返す。

普通にやるとi,jを使ってO(N*2)かかりそうだが、なんとかしてO(NM)にできないだろうか。

## Complexity

Time Complexity: `O(m * n log (m * n))`)

```cpp
class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        // create min heap with pair vector int.
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>>pq;
        //  create return vector with int
        vector<vector<int>> answer;
        // initialize heap elements with {sum, nums2 position}. push sum in order to extract num1 value later.
        for (int n: nums1) {
            pq.push({n + nums2[0], 0});
        }
        // while k and pq is not empty push the top of pq to the return element
        while(k-- && !pq.empty()) {
            // get minimum sum and position
            int sum = pq.top().first;
            int pos = pq.top().second;
            pq.pop();
            answer.push_back({sum-nums2[pos], nums2[pos]});
            // if nums2 has next elements and next pos does not go over nums2 size, push to heap
            if(pos + 1 < nums2.size()) {
                pq.push({sum-nums2[pos]+nums2[pos + 1], pos + 1});
            }
        }
        return answer;
    }
};
```

他にもこんな書き方があるらしい

これはほとんどDijkstra Algorithmを応用させたものっぽい

Dijkstra AlgorithmはNo Cycleなグラフの最短s-tを探すアルゴリズムで、訪れた場所と重みを記録することで、最短のs-tを探すことができる。

```cpp
class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        int m = nums1.size();
        int n = nums2.size();

        vector<vector<int>> ans;
        set<pair<int, int>> visited;

        priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>,
                       greater<pair<int, pair<int, int>>>> minHeap;
        minHeap.push({nums1[0] + nums2[0], {0, 0}});
        visited.insert({0, 0});

        while (k-- && !minHeap.empty()) {
            auto top = minHeap.top();
            minHeap.pop();
            int i = top.second.first;
            int j = top.second.second;

            ans.push_back({nums1[i], nums2[j]});

            if (i + 1 < m && visited.find({i + 1, j}) == visited.end()) {
                minHeap.push({nums1[i + 1] + nums2[j], {i + 1, j}});
                visited.insert({i + 1, j});
            }

            if (j + 1 < n && visited.find({i, j + 1}) == visited.end()) {
                minHeap.push({nums1[i] + nums2[j + 1], {i, j + 1}});
                visited.insert({i, j + 1});
            }
        }

        return ans;
    }
};
```
