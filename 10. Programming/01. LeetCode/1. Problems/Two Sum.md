#Easy 
## Problem Description
Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to_ `target`.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

**You can return the answer in any order.**
### Examples
#### 1
> **Input:** `nums = [2,7,11,15], target = 9`
> **Output:** `[0,1]`
> **Explanation:** Because `nums[0] + nums[1] == 9`, we return `[0, 1]`.
#### 2
> **Input:** `nums = [3,2,4], target = 6`
> **Output:** `[1,2]`
#### 3
> **Input:** nums = `[3,3], target = 6`
> **Output:** `[0,1]`

### Constraints
- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**

### Follow-up
Come up with an algorithm that is **less than $O(n^2)$ time complexity.**

## Hints
> [!HINT]- Hint 1
> A really brute force way would be to search for all possible pairs of numbers but that would be too slow. Again, it's best to try out brute force solutions for just for completeness. It is from these brute force solutions that you can come up with optimizations.

> [!HINT]- Hint 2
> So, if we fix one of the numbers, say `x`, we have to scan the entire array to find the next number `y` which is `value - x` where value is the input parameter. Can we change our array somehow so that this search becomes faster?

> [!HINT]- Hint 3
> The second train of thought is, without changing the array, can we use additional space somehow? Like maybe a hash map to speed up the search?

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Array]]
> - [[Hash Table]]

---
## 解題思路
### Brute force
最簡單直觀的做法，直接尋訪陣列，列出所有組合後，並返回符合要求的陣列索引。
實作上，使用兩層迴圈：
```python
# pseudo-code
for i in nums:
	for j in nums:
		# do logic
```

雖然空間效率僅有 $O(1)$，但時間效率卻是 $O(n^2)$，在資料量大的情形下非常耗時。

### Hash table / Hash search
在[[Search#Hash Optimization Strategies|搜尋策略]]中，有一種方法使用[[Hash Table]]來優化常規陣列搜尋的方法。在這個題目中，我們使用 $h'(x) = \text{target} - x$ 作為雜湊函數，該函數特性是鍵值對中的鍵值可以互換。也就是說，只要有其中一個 `key` 或是 `value`，我們就能輕鬆的推算出另一個期望的值。

> [!HINT]- 提示
> 使用雜湊函數逐步檢查 hash map 中是否已有存在的鍵值，如果有的話代表已經找到符合題目要求的解。沒有的話，則在 hash map 中新增相對應的雜湊函數值與其索引。

> [!HINT]- 流程
> ```md
> # pseudo-code
> target = 13
> nums:    [2,7,11,15]
> 
> 13 - 2 = 11 not in map
> mapk:    [2]
> mapv:    [0]
> 
> 13 - 7 = 6 not in map
> mapk:    [2,7]
> mapv:    [0,1]
> 
> 13 - 11 = 2 in map
> return [map[target - 11], 11's index]
> ```

> [!INFO]- 答案
> ```python
> hash_table = {}
> define two_sum(nums: list[int], target: int) -> list[int]:
> 	for i in range(len(nums)):
> 		if target - nums[i] in hash_table:
> 			return [i, hash_table[nums[i]]]
> 		hash_table[nums[i]] = hash_function(nums[i], i)		
> 	return [None] # if not found
> ```

