#Easy 
## Problem Description
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

### Examples
#### 1
> **Input:** s = `"()"`
> **Output:** `true`
> **Explanation:** 
#### 2
> **Input:** `(){}[]`
> **Output:** `true`
#### 3
> **Input:** nums = `(]`
> **Output:** `false`
#### 4
> **Input:** nums = `([])`
> **Output:** `true`

### Constraints
- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

### Follow-up
None

## Hints
> [!HINT]- Hint 1
> Pick a appropirate data structure for this problem: [[Stack]]

> [!HINT]- Hint 2
> When you encounter an opening bracket, push it to the top of the stack.

> [!HINT]- Hint 3
> When you encounter a closing bracket, check if the top of the stack was the opening for it. If yes, **do something for the stack**. Otherwise, return false.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Stack]]
> - String

---
## 我的答案
### Attempt 1
~~~~tabs
tab: Python
```py
def isValid(self, s: str) -> bool:
	stack: list[str] = []
	for i in s:
		match i:
			case '(' | '[' | '{':
				stack.append(i)
			case ')':
				if stack[-1] == '(':
					temp = stack.pop()
				else:
					return False
			case ']':
				if stack[-1] == '[':
					temp = stack.pop()
				else:
					return False
			case '}':
				if stack[-1] == '{':
					temp = stack.pop()
				else:
					return False
					
		return True
```
~~~~

#### Result
Wrong Answer
原因：未考慮 `((`、`[`、`{`  等類型的輸入。

### Attempt 2 
~~~~tabs

tab: Python
```py
def isValid(self, s: str) -> bool:
	stack: list[str] = []
	for i in s:
		match i:
			case '(' | '[' | '{':
				stack.append(i)
			case ')':
				if stack[-1] == '(':
					temp = stack.pop()
				else:
					return False
			case ']':
				if stack[-1] == '[':
					temp = stack.pop()
				else:
					return False
			case '}':
				if stack[-1] == '{':
					temp = stack.pop()
				else:
					return False

    if len(stack) == 0:
        return True
    else:
        return False
```
~~~~

#### Result
Runtime Error
原因：未考慮 `]`、`)}`、`)({}` 等類型的輸入。導致程式存取 `stack[-1]` 發生錯誤。

### Attempt 3 
~~~~tabs

tab: Python
```py
def isValid(self, s: str) -> bool:
    stack: list[str] = []
    for i in s:
        match i:
            case '(' | '[' | '{':
                stack.append(i)
            case ')':
                if len(stack) == 0:
                    stack.append(i)
                    break
                if stack[-1] == '(':
                    temp = stack.pop()
                else:
                    return False
            case ']':
                if len(stack) == 0:
                    stack.append(i)
                    break
                if stack[-1] == '[':
                    temp = stack.pop()
                else:
                    return False
            case '}':
                if len(stack) == 0:
                    stack.append(i)
                    break
                if stack[-1] == '{':
                    temp = stack.pop()
                else:
                    return False
   
        return True if len(stack) == 0 else False
```
~~~~

#### Result
Accepted
- Runtime: $0$ ms, beats 100.00%
- Memory: $17.93$ MB, beats 32.19%

---
## 解題思路
### Stack only
此題標準的解法。解題思路如同本題提示所述。

### Hash Map and Stack
只使用 Stack 會遇到一個問題，也是我解題時遇到的主要問題：
- 如果輸入只有 `[`、`{` 等左邊單邊括號
- 如果輸入只有 `)`、`]` 等右邊單邊括號，或是輸入以右邊單邊括號為始

這時候就必須額外去寫 `if else` 判斷。
因此，這方法借助 Hash Map 來記錄對應關係，並在相對應輸入情形查詢 Hasp Map，即可省去繁冗的 `if else` 判斷句。

~~~~tabs

tab: Python
```python
def isValid(self, s: str) -> bool:
    stack = []
    # 由於左括號不需檢查，因此以右括號為key，左括號為value
    mapping = {")": "(", "}": "{", "]": "["}

    for char in s:
        if char not in mapping:
            stack.append(char)
            continue
        if not stack or mapping[char] != stack.pop():
            return False
    return not stack
```
~~~~

> [!TIP] 小技巧
> 使用 `not` 關鍵字判斷 `stack` 是否為空，比起 `len()` 查看長度，較有效率，且簡潔。
> 在特定程式語言上，這兩者的執行速度可能會有差距。

ㄊ