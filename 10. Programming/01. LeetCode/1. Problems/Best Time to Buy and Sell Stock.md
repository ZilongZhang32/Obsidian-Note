#Easy 
## Problem Description
You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

### Examples
#### 1
> **Input:** `prices = [7,1,5,3,6,4]`
> **Output:** `5`
> **Explanation:** `Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.`
#### 2
> **Input:** `prices = [7,6,4,3,1]`
> **Output:** `0`
> **Explanation:** `In this case, no transactions are done and the max profit = 0.`

### Constraints
- `1 <= prices.length <= 10^5`
- `0 <= prices[i] <= 10^4`

### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Array]]
> - [[Dynamic Programming]]

---
## 我的答案
### Attempt 1
~~~~tabs

tab: Python
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        low = prices[0]
        high = prices[0]
        profit = 0

        for i in prices:
            if i < low:
                low = i
                # reset high, because all numbers
                # before low become useless
                high = low
            if i > high:
                high = i
            if high - low > profit:
                profit = high - low
        
        return profit
```
~~~~

#### Result
Accepted
- Runtime: $35$ ms, beats <font color="#9bbb59">87.72%</font>
- Memory: 19 MB, beats <font color="#c3d69b">71.90%</font>
---
## 解題思路
使用三個變數：
- `low` 儲存目前看過的最低值
- `high` 儲存目前看過的最高值
- `profit` 計算 `high - low`

只需跑一次迴圈即可完成，解題重點在於更新這三個變數的條件：
	如果新的 `profit` 比原先的要高，更新 `profit = high - low`
	如果新的 `low` 比原先的要低，更新 `low` 與 `high`。
