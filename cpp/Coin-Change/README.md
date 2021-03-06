# 题目描述:  零钱兑换

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。


# 解题思路:
用dp[i] 来表示组成i块钱，需要最少的硬币数，那么

1. 第j个硬币我可以选择不拿 这个时候， 硬币数 = dp[i]

2. 第j个硬币我可以选择拿 这个时候， 硬币数 = dp[i - coins[j]] + 1

  - 和背包问题不同， 硬币是可以拿任意个

  - 对于每一个 dp[i] 我们都选择遍历一遍 coin， 不断更新 dp[i]

PS：为啥 dp 数组初始化为 amount + 1 呢，因为凑成 amount 金额的硬币数最多只可能等于 amount（全用 1 元面值的硬币），所以初始化为 amount + 1 就相当于初始化为正无穷，便于后续取最小值。


# 时间复杂度：
  O(kn) 子问题总数不会超过金额数 n，即子问题数目为 O(n)。处理一个子问题的时间不变，仍是 O(k)，所以总的时间复杂度是 O(kn)。k 为coins种类数
# 空间复杂度
  O(kn)
  
# 代码

## [C++](./Coin-Change.cpp):
###  动态 规划
```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int res = -1;
        vector<int> dp(amount+1, amount+1);
        dp[0] = 0;
        int n = dp.size();
        for (int i = 0;i<n;i++)
        {
            for (int coin : coins)
            {
                // 只有i>coin(面值)的时候才有解
                if (i - coin < 0)
                {
                    continue;
                }
                dp[i] = min(dp[i], 1+dp[i-coin]);
            }
        }
        if (dp[amount] == amount+1)
            return -1;
        else
            res = dp[amount];
        return res;
    }
};
```

## [Python](https://github.com/bryceustc/LeetCode_Note/blob/master/python/Coin-Change/Coin-Change.py):
### 
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        res = -1
        dp = [amount+1 for _ in range(amount+1)]
        dp[0] = 0
        n = len(dp)
        for i in range(n):
            for coin in coins:
                if i < coin:
                    continue
                dp[i] = min(dp[i], dp[i-coin]+1)
        if dp[amount]==amount+1:
            res = -1
        else:
            res = dp[amount]
        return res
```
## 参考
  - [题解讨论](https://leetcode-cn.com/problems/coin-change/solution/dong-tai-gui-hua-suan-fa-si-xiang-by-hikes/)
