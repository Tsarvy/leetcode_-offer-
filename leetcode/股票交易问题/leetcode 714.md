#### leetcode 714

---

给定一个整数数组 prices，其中第 i 个元素代表了第 i 天的股票价格 ；非负整数 fee 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

---

- 最优子结构：根据之前的经验，我们可以得到之前的状态都是相符的

- 无后效性： 也是符合的

- 状态转移方程：

  dp[i] [0] = max(dp[i-1] [0] , dp[i-1] [1] - fee + price[i])

  dp[i] [1] = max(dp[i-1] [1] , dp[i-1] [0] - price[i])

- 最终的结果等于dp[n-1] [0] 

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)

        dp = [0] * n
        for i in range(n):
            dp[i] = [0] * 2
        
        dp[0][1] = 0 - prices[0] 
        for i in range(1,n):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] - fee + prices[i])
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
        return dp[n-1][0]
```

