#### leetcode 123

---

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

---

- 之前是可以完成无限次交易，现在只可以完成两笔交易，因此之前的方法就会影响到无后效性，所以我们要增加状态。
  - 之前是二维的dp表，现在在之前的每个状态上面增加当前的交易次数这个状态，最优子结构同之前的股票问题。
  - 加上状态之后就满足了无后效性
- 状态转移方程及初始化：
  - dp[i] [j] [k] i表示第i天是否持有股票在第几笔交易之后的最大利润，j表示是否持有股票，k表示第几笔交易。可以知道我们最终想要的结果是max(dp[n-1] [0] [1], dp[n-1] [0] [2], 0)，状态转移类似于之前的。
    - dp[i] [0] [1] = 0 - prices[0].   dp[i] [0] [2] = -np.inf
    - dp[i] [0] [1] = max( dp[i-1] [0] [1], dp[i-1] [1] [0] + prices[i])
    - dp[i] [1] [1] = max( dp[i-1] [0] [1] - prices[i], dp[i-1] [1] [1])
    - dp[i] [1] [0] = max( dp[i-1] [0] [1] + price[i], dp[i-1] [1] [0])
    - dp[i] [0] [2] = max( dp[i-1] [0] [2], dp[i-1] [1] [1] + prices[i]) 

- 最终得到的结果是max(dp[n-1] [0] [1], dp[n-1] [0] [2])

- ```python
  class Solution:
      def maxProfit(self, prices: List[int]) -> int:
          n = len(prices)
          dp = [0] * n
          for i in range(n):
              dp[i] = [0] * 2
          for i in range(n):
              for j in range(2):
                  dp[i][j] = [0] * 3
          
          dp[0][1][0] = 0 - prices[0]
          dp[0][1][1] = -9999
  
          for i in range(1,n):
              dp[i][1][0] = max(dp[i-1][1][0],dp[i-1][0][0] - prices[i])
              dp[i][0][1] = max(dp[i-1][1][0] + prices[i], dp[i-1][0][1])
              dp[i][1][1] = max(dp[i-1][0][1] - prices[i], dp[i-1][1][1])
              dp[i][0][2] = max(dp[i-1][1][1] + prices[i], dp[i-1][0][2])
          return max(dp[n-1][0][1],dp[n-1][0][2])
  ```

  