## 概要

https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/

> 
> 
> 
> You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
> 
> On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.
> 
> Find and return *the **maximum** profit you can achieve*.
> 

## Intuition

１回の取引で最大利益、ではなく

複数の取引で最大利益、になった

なので、保持する値は

蓄積利益のみになる。

あとは、現在と手前の株価の差分がプラスなら取引をして、取引結果を利益に蓄積していくだけ

## Code

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.empty()) return 0;
        int size = prices.size();
        int profit = 0;
        for(int i = 1; i < size; i++) {
            if(prices[i] - prices[i-1] > 0) profit += prices[i] - prices[i-1];
        }
        return profit;
    }
};
```
