## Introduction
**Bit Manipulation（位元操作）** 是直接對一個數字的「二進位位元」做操作的技巧。因為所有數字在電腦底層本來就是用二進位表示的，所以這種操作速度非常快、記憶體也省！
它常用來解決：
- 效能要求高的演算法（例如競賽）
- 位元旗標（bit flags）
- 位圖（bitmaps）
- 資料壓縮 / 加密
- 奇偶性、交換、不使用額外變數的計算…

位元操作強大的原因有：
- CPU 原生支援
	**每一個操作幾乎都是一個 CPU 指令**
- 空間省
	一個整數可以表示 **32 或 64 個布林值**
- 效能爆炸快
	比[[Array|陣列]]、map、set 快很多（如果能用上的話）

## Common Operations
這邊是最常見的幾個位元操作技巧：

| **操作** | **說明**          | **範例（以** `x = 10` **即** `0b1010`**）** |
| ------ | --------------- | ------------------------------------- |
| &（AND） | 兩個位元都為 1，結果才為 1 | `x & 1`：取得最後一位（奇偶性）                   |
| \|     | \|（OR）          | 只要有一個為 `1`，結果就是 `1`                   |
| ^（XOR） | 相同為 0，不同為 1     | `x ^ 1`：翻轉最後一位                        |
| ~（NOT） | 位元反轉            | `~x`：得到補數（全反）                         |
| <<（左移） | 把位元向左推，低位補 0    | `x << 1`：乘以 2                         |
| >>（右移） | 把位元向右推，高位補 0/1  | `x >> 1`：除以 2（整除）                     |

## Classic Examples
~~~~tabs
tab: Odd or even
```python
if x & 1 == 1:
    print("奇數")
else:
    print("偶數")
```
tab: Flip the specified bit
```py
x = x ^ (1 << k)
```
tab: set the specified bit (to 1)
```py
x = x | (1 << k)
```
~~~~

~~~tabs
tab: clear the specified bit (to 0)
```py
x = x & ~(1 << k)
```
tab: get the specified bit
```py
bit = (x >> k) & 1
```
tab: removing last 1
```py
# calculating hamming weight
# calculate the number of 1
count = 0
while x:
    x = x & (x - 1)
    count += 1
```
~~~


## Typical Application
| **題型**     | **舉例**                                  |
| ---------- | --------------------------------------- |
| 位元加法       | [[Add Binary]], [[Sum of Two Integers]] |
| 子集生成       | Subsets (用 binary mask 產生所有子集)          |
| 單一數值辨認     | Single Number (XOR 所有元素找出不重複的)          |
| 字符統計       | Letter counting using 26-bit flags      |
| Bitmask DP | 如「可疊積的石頭」、「記憶所有子集狀態」                    |
