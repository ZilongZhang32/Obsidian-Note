#Easy 
## Problem Description
Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

### Examples
#### 1
![300](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)
> **Input:** `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8`
> **Output:** `6`
> **Explanation:**  `The LCA of nodes 2 and 8 is 6.`
#### 2
![300](https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png)
> **Input:** `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4`
> **Output:** `2`
> **Explanation:** `The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.`
#### 3
> **Input:** `root = [2,1], p = 2, q = 1`
> **Output:** `2`

### Constraints
- The number of nodes in the tree is in the range `[2, 10^5]`.
- `-10^9 <= Node.val <= 10^9`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the BST.

### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Binary Search Tree]]
> - [[Depth-First Search]]

---
## 我的答案
### Attempt 1
~~~~tabs

tab: Python
```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```
~~~~

#### Result
Accepted
- Runtime: $56$ ms, beats 43.94%
- Memory: 20.45 MB, beats 70.44%

---
## 解題思路
當你試著使用題目給的範例嘗試不同的 `p` 與 `q`，應該不難發現題目要找的其實就是第一個符合 `p` $<$ `target` $<$ `q` 的值。