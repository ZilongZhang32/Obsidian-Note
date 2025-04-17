#Easy 
## Problem Description
Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

### Examples
#### 1
> **Input:** `nums = [-1,0,3,5,9,12], target = 9`
> **Output:** `4`
> **Explanation:** `9 exists in nums and its index is 4`
#### 2
> **Input:** `nums = [-1,0,3,5,9,12], target = 2`
> **Output:** `-1`
> **Explanation:** `2 does not exist in nums so return -1`

### Constraints
- `1 <= nums.length <= 104`
- `-104 < nums[i], target < 104`
- All the integers in `nums` are **unique**.
- `nums` is sorted in ascending order.

### Follow-up
None.

## Hints
No hints for this solution.

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
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        i = 0
        j = len(nums) - 1
        
        while i <= j:
            middle = i + (j - i) // 2

            if nums[middle] < target:
                i = middle + 1
            
            elif nums[middle] > target:
                j = middle - 1
            
            else:
                return middle
            
        return -1
```
~~~

#### Result
Accepted
- Runtime: $0$ ms, beats <font color="#76923c">100.00%</font>
- Memory: 13.30 MB, beats <font color="#9bbb59">93.59%</font>

---
## 解題思路
非常基本的資料結構與演算法。就是將 [[Binary Search Tree#Searching for a node|Binary Search]] 實作出來而已。