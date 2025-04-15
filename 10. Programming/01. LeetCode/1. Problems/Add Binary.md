 #Easy
## Problem Description
Given two binary strings `a` and `b`, return _their sum as a binary string_.

### Examples
#### 1
> **Input:** `a = "11", b = "1"`
> **Output:** `"100"`
#### 2
> **Input:** `a = "1010", b = "1011"`
> **Output:** `"10101"`

### Constraints
- `1 <= a.length, b.length <= 104`
- `a` and `b` consist only of `'0'` or `'1'` characters.
- Each string does not contain leading zeros except for the `'0'` itself.

### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[String]]
> - [[Bit Manipulation]]
> - [[Simulation]]

---
## 我的答案
### Attempt 1
Primitive String Processing
~~~~tabs

tab: Python
```python
def addBinary(self, a, b):
    """
    :type a: str
    :type b: str
    :rtype: str
    """

    power = 0
    sum = 0
    sum_binary_inverted = ""

    for i in a[::-1]:
        sum += int(i) * (2 ** power)
        power += 1
        
    power = 0

    for j in b[::-1]:
        sum += int(j) * (2 ** power)
        power += 1

    if sum:
        while sum != 0:
            d = sum % 2
            sum_binary_inverted += str(d)
            sum //= 2
    else:
        return "0"


    return sum_binary_inverted[::-1]
```
~~~~
#### Result
Accepted
- Runtime: $11$ ms, beats <font color="#c0504d">11.96%</font>
	**Too slow**
- Memory: 12.5MB, beats 56.11%
### Attempt 2 
Basically as same as the Attempt 1, but a little bit faster.
~~~tabs
tab: Python
```python
def addBinary(self, a, b):
    """
    :type a: str
    :type b: str
    :rtype: str
    """

    sum_binary = []
    i = len(a) - 1
    j = len(b) - 1
    carry = 0

    # iterate form the rear
    while i >= 0 or j >= 0 or carry:
        if i >= 0:
            carry += int(a[i])
            i -= 1

        if j >= 0:
            carry += int(b[j])
            j -= 1

        # append 0 or 1
        sum_binary.append(str(carry % 2))

        # if carry >= 2, then it means we ned to add 1 to the bit add.
        # else, do nothing
        carry //= 2

    return "".join(reversed(sum_binary))
```
~~~
#### Result
Accepted
- Runtime: $7$ ms, beats <font color="#d99694">32.77%</font>
	**Still not fast enough**
- Memory: 12.38MB, beats <font color="#c3d69b">85.88%</font>
### Attempt 3
Bit manipulation approach
~~~~tabs

tab: Python
```python
def addBinary(self, a, b):
    """
    :type a: str
    :type b: str
    :rtype: str
    """
    a, b = int(a, 2), int(b, 2)

    while b:
        sum = a ^ b
        carry = (a & b) << 1
        a, b = sum, carry
        
	return bin(a)[2:]
```
~~~~
#### Result
Accepted
- Runtime: $0$ ms, beats <font color="#9bbb59">100.00%</font>
- Memory: 12.54MB, beats <font color="#c0504d">16.15%</font>


---
## 解題思路
首先，由於輸入資料型態是 `string`，因此要先轉成 `int` 等數字類的形態。
### String Processing
由於加法是從最後一位數字往前加，所以使用 for loop，由最後一位往前做。
做加法的時候要判斷是否要進位。

### [[Bit Manipulation]]
比較進階的技巧，沒用過這個技巧的人在第一時間可能有點難直接反應出來。
如果你熟悉電路或 CPU 運作，你會知道「加法器」會這樣運作：
- **XOR（^）**
	模擬「不帶進位」的 bit 加法。
	例如：`1 ^ 1 = 0`（因為 `1+1 = 10`，要進位），`1 ^ 0 = 1`。
- **AND（&）**
	用來找出「哪些位元要進位」，例如：`1 & 1 = 1` 表示這一位有進位。
- **左移（<<）**
	把進位移到「該進到的地方」。

把 XOR 結果跟進位結果當作「新的 `carry(a^b)` 與 `sum((a&b)<<1)`」，繼續重複這個動作，直到 **沒有進位了**（也就是 `carry == 0`），代表加法完成！
