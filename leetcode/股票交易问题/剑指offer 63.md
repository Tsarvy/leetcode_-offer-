#### 剑指offer 63

---

假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。

---

- 因为是最大利润问题，所以我们考虑使用dp动态规划求解这类问题（当然暴力也能解决，花费大概是O（n^2））
  - 我们首先考虑是否满足最优子结构性质，假设dp[i]表示以price[i]为结尾的最大利润，那么dp[i]可以通过max (dp[i-1] , price[i] + min(price[:i])）来完成
  - 无后效性满足，因为之前的dp状态取值方法不会对后续的dp状态转移产生影响
- 因此我们可以使用dp来解决该问题，转移方程也在上面写出。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        n = len(prices)

        dp = [0] * n
        minn = prices[0]
        for i in range(1,n):
            if prices[i] < minn:
                minn = prices[i]
            dp[i] = max(dp[i-1] , prices[i] - minn)
        return dp[n-1]
```

