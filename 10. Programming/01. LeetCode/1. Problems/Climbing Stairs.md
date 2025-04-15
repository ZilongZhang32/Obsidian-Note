#Easy
## Problem Description
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

### Examples
#### 1
> **Input:** `n = 2`
> **Output:** `2`
> **Explanation:** `There are two ways to climb to the top.
> `1. 1 step + 1 step`
> `2. 2 steps`
#### 2
> **Input:** `n = 3`
> **Output:** `3`
> **Explanation:** `There are three ways to climb to the top.
> `1. 1 step + 1 step + 1 step`
> `2. 2 steps + 1 step`
> `3. 1 step + 2 steps`

### Constraints
- `1 <= n <= 45`

### Follow-up
None.

## Hints
> [!HINT]- Hint 1
> To reach nth step, what could have been your previous steps? (Think about the step sizes)

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Dynamic Programming]]

---
## 我的答案
### Attempt 1
~~~tabs
tab: Python
```python
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """

        record = [1,2]
        i = 3 - 1

        while i < n:
            record.append(record[i-1]+record[i-2])
            i += 1


        return record[n-1]
```
~~~

#### Result
Accepted
- Runtime: $0$ ms, beats <font color="#76923c">100.00%</font>
- Memory: 12.46 MB, beats <font color="#d7e3bc">54.31%</font>

---
## 解題思路
請參見[[Dynamic Programming#A classic easy question example|這裡]]。
