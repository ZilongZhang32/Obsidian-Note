## Introduction
Two Pointers（雙指標）是一種在資料結構（通常是[[Array]]或[[String]]）上操作的技巧，透過同時使用兩個pointer，來有效解決涉及索引比較或移動的問題。這種技巧常用於優化時間複雜度，特別是在排序資料結構上表現尤為突出。

## When to Use?
Two Pointers 適用於以下類型的問題：
1.	排序陣列上的搜尋問題
	•	找出兩數之和（Two Sum）或是否存在某對數字滿足條件。
2.	子陣列或子字串問題
	•	找出符合條件的最小子陣列長度。
3.	[[Linked List]]操作
	•	檢測迴圈或找到中間節點。
4.	合併與交集或差集
	•	合併兩個排序陣列或找出兩個陣列的交集或差集。
5. 比較兩個數據集合
	•	比對相似性，或是處理包含關係。
	•	[[Sorting#Merge Sort|Merge Sort]]中，合併兩個子陣列。

## Usage
### Opposite direction 雙向移動
一個指標從頭開始，另一個指標從尾開始，兩指針向中間靠攏。
```python
def two_sum(nums, target):
    nums.sort()  # 假設輸入陣列已排序，否則需先排序
    left, right = 0, len(nums) - 1
    
    while left < right:
        current_sum = nums[left] + nums[right]
        if current_sum == target:
            return [nums[left], nums[right]]
        elif current_sum < target:
            left += 1  # 移動左指針
        else:
            right -= 1  # 移動右指針
            
    return []  # 未找到
```

### Same direction 同向移動
兩個指標都從同一側開始，其中一個指標用於固定，另一個指標用於探索。
```python
def min_subarray_len(target, nums):
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

### Two arrays 兩個陣列
兩個指標（通常使用Same Drection方法）分別操作兩個陣列，透過逐步移動指針來完成遍歷或合併的邏輯。
```python
def two_pointers_with_two_arrays(arr1, arr2):
    i = j = 0
    result = [] # or results = 0, depends on the problem

    while i < len(arr1) and j < len(arr2):
        # do some logic here
        if CONDITION:
            i += 1
        else:
            j += 1

    while i < len(arr1):
        # do some logic
        i += 1
    
    while j < len(arr2):
        # do some logic
        j += 1
    
    return result
```

## Complexity Analysis
### Time complexity
大部分情形之下為$O(n)$，因為每個指標最多遍歷一次陣列。
### Space complexity
大部分情形之下為$O(1)$，因為只需要使用兩個變數來存放指標。

## Common Problem Patterns
1. Two Sum
2. Longest Substring without Repeating Characters
3. Container with Most Water
4. Remove Duplicates from Sorted Array

## Pros and Cons
### Pros
1. 簡單易用，邏輯清晰。
2. 適用於處理大部分涉及排序或區間的問題。
### Cons
1. 只適用於有序資料結構（有時需先排序）。
2. 需要問題本身具備雙指針可以解的結構性。