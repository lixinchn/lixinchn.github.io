---
layout: post
title: (Dynamic Programming) 01 knapsack problem
---

### 描述
给定一个背包，背包能装的重量是有限的，假设为n。

再给一系列带有重量的物品，每个物品对应不同的价值。问如果将背包装满，能得到的最大价值是多少？


### 分析
如果有3块物品，重量分别为[1,2,3]，对应的价值分别为[1,3,3]，背包的载重量为3。

假设
* 如果只有物品1

  + 不论背包的载重量是多少，能得到的总价值只能是1
    
* 如果有物品1和2

  + 如果背包载重量为1，总价值只能是1，跟上一次一样
  + 如果背包载重量为2，总价值为max(3, 1) = 3，装了物品2或者上一次背包载重量为2时得到的总价值，取大
  + 如果背包载重量为3，总价值为max(3 + 1, 1) = 4，装了物品2和物品1或者上一次背包载重量为3时得到的总价值

* 如果有物品1，2，3
  + 如果背包载重量为1，总价值只能是1，跟上一次一样
  + 如果背包载重量为2，因为装不下物品3，总价值只能是上一次背包载重量为2时的价值，也就是3
  + 如果背包载重量为3，总价值为max(3, 4) = 4，也就是装了物品3或者上一次背包载重量为3时的总价值，取大

----
可以得出转移方程:

            dp[i - 1][j] if 当前物品重量大于背包载重量
            
dp[i][j] = 

            max(当前商品价值 + dp[i - 1][j - 当前商品重量],  dp[i - 1][j])
            
其中，i表示当前是第i个商品，j表示当前背包的载重量。

----

图表如下：


|   背包        | 0kg | 1kg | 2kg | 3kg |
| --------   | ----  | ----  | ---- | ---- |
| 1kg(1value)      | 0value | 1value | 1value | 1value |
| 2kg(3value)      | 0value | 1value | max(3 + 0, 1) = 3value | max(3 + 1, 1) = 4value |
| 3kg(3value)      | 0value | 1value | 3value | max(3 + 0, 4) = 4value |


----

### Python Code

```python
class Solution():
    def do(self, list_weights, list_values, total_weight):
        '''
        dp[i][j] = 
                    if list_weights[i] > j:
                        dp[i - 1][j]
                    else:
                        max(list_values[i] + dp[i - 1][j - list_weights[i]], dp[i - 1][j])
        '''
        dp = [[0] * (total_weight + 1) for _ in range(len(list_weights))]
        for i in range(len(list_weights)):
            for j in range(total_weight + 1):
                if j == 0:
                    continue

                if i == 0:
                    if list_weights[i] > total_weight:
                        continue
                    dp[i][j] = list_values[i]
                else:
                    if list_weights[i] > j:
                        dp[i][j] = dp[i - 1][j]
                    else:
                        dp[i][j] = max(list_values[i] + dp[i - 1][j - list_weights[i]], dp[i - 1][j])
        return dp[-1][-1]



solution = Solution()
print solution.do([1, 3, 4, 5], [1, 4, 5, 7], 7)
```
