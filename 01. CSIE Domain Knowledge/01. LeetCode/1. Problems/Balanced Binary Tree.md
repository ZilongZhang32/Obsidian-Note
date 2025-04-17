#Easy 
## Problem Description
Given a binary tree, determine if it is **height-balanced**.

### Examples
#### 1
![|400px](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)
> **Input:** `root = [3,9,20,null,null,15,7]`
> **Output:** `true`
#### 2
![400px](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)
> **Input:** `[1,2,2,3,3,null,null,4,4]`
> **Output:** `false`
#### 3
> **Input:** `[]`
> **Output:** `true`

### Constraints
- The number of nodes in the tree is in the range `[0, 5000]`.
- $-10^4$ `<= Node.val <=` $10^4$

### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Tree]]
> - [[Binary Tree]]

---
## 我的答案
### Attempt 1
~~~~tabs

tab: Python
```python
def isBalanced(self, root: Optional[TreeNode]) -> bool:
    def dfs(node):
        if not node:
            return [True, 0]

        left = dfs(node.left)
        right = dfs(node.right)
        balanced = left[0] and right[0] and \
                   abs(left[1] - right[1]) <= 1
        
        return [balanced, 1+max(left[1], right[1])]

    return dfs(root)[0]
```
~~~~

#### Result
Accepted
- Runtime: $8$ ms, beats <font color="#c0504d">19.80%</font>
- Memory: 18.98MB, beats <font color="#c0504d">25.89%</font>

---
## 解題思路
有關樹的題目，遇到這種要判斷整棵樹是否符合某個條件的題目時，比較好的做法是從數的底部開始做。這樣的好處是我們只需訪問每個節點一次，從而達到時間複雜度僅有 $O(n)$。如果是從樹根開始做，樹根會被訪問 $n$ 次，時間複雜度就變成 $n \cdot O(n) = O(n^2)$。

在訪問節點時，使用一個陣列儲存針對某個節點作為樹根的狀態以及函式 `DFS()` 的回傳值：`[bool, int]`，其中：
- `bool` 儲存當前節點作為樹根是否滿足 balanced binary tree
- `int` 儲存當前節點作為樹根的樹高。關於樹高的求法，則是左樹高度與右樹高度終曲高的，並額外 `+1` （樹根也算）。

所以，使用 DFS 找出樹最深的節點。再逐個判斷是否符合題目要求。如果有其中一個節點不符合要求就直接回傳 `false`，否則回傳 `true`。最後回傳 `DFS(root)[0]` 即為題目所求。