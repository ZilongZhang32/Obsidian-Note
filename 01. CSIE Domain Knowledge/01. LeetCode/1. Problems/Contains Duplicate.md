#Easy 
## Problem Description
Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

### Examples
#### 1
> **Input:** `nums = [1,2,3,1]`
> **Output:** `true`
> **Explanation:** `The element 1 occurs at the indices 0 and 3.`
#### 2
> **Input:** `[1,2,3,4]`
> **Output:** `false`
> **Explanation:** `All elements are distinct.`
#### 3
> **Input:** `[1,1,1,3,3,4,3,2,4,2]`
> **Output:** `true`

### Constraints
- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`

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
~~~~tabs

tab: Python
```python
class Solution(object):
    def containsDuplicate(self, nums):
        appeared = set()
        for i in nums:
            if i not in appeared:
                appeared.add(i)
            else:
                return True
        
        return False
```
~~~~

#### Result
Accepted
- Runtime: $11$ ms, beats <font color="#c3d69b">75.99%</font>
- Memory: 26.00 MB, beats <font color="#e5b9b7">44.23%</font>

---
## 解題思路
### Sorting
略
### Hash Map / Hash Set
略