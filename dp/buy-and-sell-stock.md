## 概要

https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

## Intuition

利益が最大の取引がしたい。

- 同じ日に取引はできない
- 取引は一度のみ
- マイナスになるような取引は0になる

## Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        
        if(prices.empty()) return 0;
        int min_price = prices[0];
        int max_profit = 0;
        int n = prices.size();
        for(int i = 0; i < n; i++) {
            if(prices[i] < min_price) min_price = prices[i];
            int profit = prices[i] - min_price;
            if(profit > max_profit) max_profit = profit;
        }
        return max_profit;
    }
};
```
