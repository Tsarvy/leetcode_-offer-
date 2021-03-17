#### leetcode 309

---

给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

---

- 最优子结构：满足之前的 

- 无后效性： 因为卖出第二天才能买，含冷冻期，所以要多加考虑一下

- 状态转移方程：

  dp[i] [0] = max(dp[i-1] [0] , dp[i-1] [1] + price[i])

  dp[i] [1] = max(dp[i-1] [1] , dp[i-2] [0] - price[i])

- 最终的结果等于dp[n-1] [0] 

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        dp = [0] * n
        for i in range(n):
            dp[i] = [0,0]
        
        dp[0][1] = 0 - prices[0]
        dp[0][0] = 0

        for i in range(1,n):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
            if i >= 2:
                dp[i][1] = max(dp[i-1][1], dp[i-2][0] - prices[i])
            else:
                dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
        return dp[n-1][0]
```

