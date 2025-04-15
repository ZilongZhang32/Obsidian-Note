## Introduction
Sliding Window 主要用於有效解決與子陣列或子字串相關的問題。它的核心思想是設計一個「窗口」來動態追蹤子結構的範圍，並透過調整窗口的大小或位置來優化解法。這種方法通常能將暴力解法的時間複雜度從 $O(n^2)$ 降低到 $O(n)$。

```python
def fn(arr):
    left = ans = curr = 0

    for right in range(len(arr)):
        # do logic here to add arr[right] to curr

        while WINDOW_CONDITION_BROKEN:
            # remove arr[left] from curr
            left += 1

        # update ans
    
    return ans
```

## When to Use?
1.	處理連續子陣列的問題
	•	尋找子陣列或子字串的特定屬性
2. 查找字串的（連續不間斷的）子字串問題
	•	找出字串中包含某些字符的最短子字串。
```python
def min_window(s, t):
    from collections import Counter
    need, missing = Counter(t), len(t)
    left, start, end = 0, 0, 0
    for right, char in enumerate(s, 1):
        missing -= need[char] > 0
        need[char] -= 1
        if not missing:
            while left < right and need[s[left]] < 0:
                need[s[left]] += 1
                left += 1
            if not end or right - left < end - start:
                start, end = left, right
    return s[start:end]
```
3. 固定大小的子陣列或子字串處理問題
	•	計算固定大小的子陣列中最大和。

## Usage
### Dynamic Size Window 動態窗口大小
適合處理可變大小的子陣列問題。
```python
def find_min_subarray(target, nums):
    left, current_sum = 0, 0
    min_length = float('inf')
    for right in range(len(nums)):
        current_sum += nums[right]
        while current_sum >= target:
            min_length = min(min_length, right - left + 1)
            current_sum -= nums[left]
            left += 1
    return min_length if min_length != float('inf') else 0
```

### Fixed Size Window 固定窗口大小
適合處理固定長度的子陣列問題。
```python
def max_sum_subarray(nums, k):
    current_sum = sum(nums[:k])
    max_sum = current_sum
    for i in range(k, len(nums)):
        current_sum += nums[i] - nums[i - k]
        max_sum = max(max_sum, current_sum)
    return max_sum
```

## Complexity Analysis
### Time complexity
1. **最佳情況**：$O(n)$（每個元素最多被訪問兩次）。
2. **最壞情況**：依賴於邏輯條件，可能會稍微增加額外的操作，但通常仍維持 $O(n)$。
### Space complexity
通常為 $O(1)$，因為只需額外的變數來追蹤指標和臨時計算值。

## Common Problem Patterns


## Pros and Cons
### Pros
1. **高效率**
	能將多數Nested Loop問題優化為線性時間。
2. **簡單實現**
	邏輯結構清晰且容易理解。
### Cons
1. **範圍邏輯複雜**
	對於需要多條件判斷的問題，邊界處理較為繁瑣。
2. **依賴特定結構**
	僅適用於有序或連續結構（如陣列、字串）。