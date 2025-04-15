#Easy 
## Problem Description
Given a string `s` which consists of lowercase or uppercase letters, return the length of the **longest palindrome** that can be built with those letters.

> A **palindrome** is a string that reads the same forward and backward.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome.

### Examples
#### 1
> **Input:** `s = "abccccdd"`
> **Output:** `7`
> **Explanation:** `One longest palindrome that can be built is "dccaccd", whose length is 7.`
#### 2
> **Input:** `s = "a"`
> **Output:** `1`
> **Explanation:** `The longest palindrome that can be built is "a", whose length is 1.`

### Constraints
- `1 <= s.length <= 2000`
- `s` consists of lowercase **and/or** uppercase English letters only.

### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Hash Table]]
> - [[Greedy]]

---
## 我的答案
### Attempt 1
~~~tabs
tab: Python
```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        appeared_char = {}
        length = 0

        for i in s:
            if i not in appeared_char:
                appeared_char[i] = 1
            else:
                appeared_char[i] += 1
        
        for j in appeared_char:
            if appeared_char[j] > 1:
                length += appeared_char[j]
        
        return length + 1
```
```
~~~
#### Result
Wrong Answer
- `s = "bb"`
	- `output = 3` , expected `output = 2`

### Attempt 2 
~~~tabs
tab: Python
```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        appeared_char = {}
        length = 0
        lone_char = False

        for i in s:
            if i not in appeared_char:
                appeared_char[i] = 1
            else:
                appeared_char[i] += 1
        
        for j in appeared_char:
            if appeared_char[j] > 1:
                length += appeared_char[j]
        
        for k in appeared_char:
            if appeared_char[k] == 1:
                lone_char = True
                break
            else:
                continue
        
        return length + 1 if lone_char else length
```
~~~

#### Result
Wrong Answer
- `s = "bananas"`
	- `output = 36` , expected `output = 5`

### Attempt 3
~~~tabs
tab: Python
```python
class Solution(object):
    def longestPalindrome(self, s):
        appeared_char = {}
        length = 0
        odd_char = False

        for i in s:
            if i not in appeared_char:
                appeared_char[i] = 1
            else:
                appeared_char[i] += 1
        
        for j in appeared_char:
            if appeared_char[j] % 2 == 0:
                length += appeared_char[j]
            else:
                odd_char = True
                length += (appeared_char[j] - 1)

        if odd_char:
            return length + 1
        else:
            return length
```
~~~

#### Result
Accepted
- Runtime: $3$ ms, beats <font color="#c3d69b">73.99%</font>
- Memory: 12.47 MB, beats <font color="#d7e3bc">53.58%</font>

---
## 解題思路
本題難點在於針對各個字元出現的個數做出判斷，對於一個回文單字，最多只會有一個字元出現次數是奇數。那麼，輸入中若有多個字元出現次數是基數的時候要如何處理？

> 這些字元中，必定有一個字元出線次數是最多的。因此，我們在計算回文長度時，碰到這些字元的時候，將其出現次數 $-1$ 累加至回文長度計數器。最後在回傳的時候，再把事先排除掉的 $1$ 加回去即可。