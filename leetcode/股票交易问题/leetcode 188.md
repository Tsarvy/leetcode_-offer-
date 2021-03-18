#### leetcode 188

---

给定一个整数数组 prices ，它的第 i 个元素 prices[i] 是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

---

- 同之前的股票一样，使用动态规划，和上一题的区别是这个是可以完成k次交易，之前的最多2次交易，因此，我们将之前那个数组的这个维度设置成k，得到dp[i] [k] [j] 表示第i天的第k次交易是否持有股票时的最大收益。因为我在这里设置的是当k=1的时候才算第一次交易的完成，所以最终的结果是max(dp[i] [:] [0])

- 状态转移方程：

  - dp[i] [j] [1] = max(dp[i-1] [j] [1], dp[i-1] [j] [0] - prices[i])

    dp[i] [k] [0] = max(dp[i-1] [j] [0],dp[i-1] [j-1] [1] + prices[i])

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        n = len(prices)
        if not prices:
            return 0
        if k == 0:
            return 0
        dp = [0] * n
        for i in range(n):
            dp[i] = [0] * (k+1)
        #dp[i][j] 表示在第i天的最大收益，j表示持有与否和交易次数，奇数表示持有，偶数表示未持有，最终结果是max(dp[n-1][0,2,4,...2k])
        for i in range(n):
            for j in range(k+1):
                dp[i][j] = [0] * 2
        for i in range(n):
            for k_ in range(k+1):
                dp[i][k_][0] = 0
                dp[i][k_][1] = -999


        dp[0][0][1] =  -prices[0]
        for i in range(1,n):
            for k_ in range(k+1):
                if k_ < 1:
                    dp[i][k_][1] = max(dp[i-1][k_][1], dp[i-1][k_][0] - prices[i])
                else:
                    dp[i][k_][1] = max(dp[i-1][k_][1], dp[i-1][k_][0] - prices[i])
                    dp[i][k_][0] = max(dp[i-1][k_][0],dp[i-1][k_-1][1] + prices[i])
                    
        ans = 0
        for i in range(n):
            for j in range(k+1):
                if ans < dp[i][j][0]:
                    ans = dp[i][j][0]
        return ans

```

----

- 另外一种就是将三维的转变成两维的状态表，因为将k和是否持有放在一起，从0到2k，奇数的时候表示持有，偶数的时候表示卖出了

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        n = len(prices)
        if not prices:
            return 0
        if k == 0:
            return 0
        dp = [0] * n
        for i in range(n):
            dp[i] = [0] * (2*k+1)
        #dp[i][j] 表示在第i天的最大收益，j表示持有与否和交易次数，奇数表示持有，偶数表示未持有，最终结果是max(dp[n-1][0,2,4,...2k])
        for i in range(n):
            for j in range(2*k+1):
                if j % 2 != 0:
                    dp[i][j] = -999
        dp[0][1] =  -prices[0]
        for i in range(1,n):
            for j in range(1,2*k+1):
                if j % 2 == 0:
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-1] + prices[i])
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-1] - prices[i])
        print(dp)
        return max(dp[n-1])


```

