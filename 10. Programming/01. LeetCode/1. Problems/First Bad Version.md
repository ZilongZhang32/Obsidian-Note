#Easy 
## Problem Description
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

### Examples
#### 1
> **Input:** `n = 5 (bad = 4)`
> **Output:** `4`
> **Explanation:** 
> `call isBadVersion(3) -> false`
> `call isBadVersion(5) -> true`
> `call isBadVersion(4) -> true`
> `Then 4 is the first bad version.`
#### 2
> **Input:** `n = 1 (bad = 1)`
> **Output:** `1`

### Constraints
- `1 <= bad <= n <= 2^31 - 1`

### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - 

---
## 我的答案
### Attempt 1
~~~tabs
tab: Python
```python
class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        left = 1
        right = n

        while left <= right:
            middle = left + (right - left) // 2
            if not isBadVersion(middle):
                left = middle + 1
            else:
                right = middle - 1
        
        return left
```
~~~

#### Result
Accepted
- Runtime: 17 ms, beats <font color="#d99694">38.49%</font>
- Memory: 12.47 MB, beats <font color="#d7e3bc">50.49%</font>


---
## 解題思路
這提相當於使用 [[Search#Binary Search|Binary Search]] 在陣列中找出第一個 `target` 出現的索引值。詳細方法請參照[[Search#Left boundary searching|這裡]]。