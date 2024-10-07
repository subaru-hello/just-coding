## 概要

https://leetcode.com/problems/coin-change/description/

## Intuition

amountとcoinsが渡される。

同じ通貨も使っていい

amountに達するまでに最低何このコインを使う必要があるのかは、coinごとにamountまで何枚必要かを計算すると分かる

## Code

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if(amount == 0) return 0;
        int sentiment = amount + 1;
        // 到達しない値でdpを初期化する
        vector<int> dp(sentiment, sentiment);
        dp[0] = 0;
        for(int& coin : coins) {
            for(int i = coin; i <= amount; i++) {
                dp[i] = min(dp[i], dp[i-coin] + 1);
            }
        }
        return dp[amount] != sentiment ? dp[amount] : -1;
    }
};
```
