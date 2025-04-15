#Easy 
## Problem Description
Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

### Examples
#### 1
![400px](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)
> **Input:** `root = [1,2,3,4,5]`
> **Output:** `3`
> **Explanation:** `3 is the length of the path [4,2,1,3] or [5,2,1,3].`
#### 2
> **Input:** `root = [1,2]`
> **Output:** `1`

### Constraints
- The number of nodes in the tree is in the range `[1, 10^4]`.
- `-100 <= Node.val <= 100`
### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Binary Tree]]
> - [[Depth-First Search]]

---
## 我的答案
### Attempt 1
~~~tabs
tab: Python
```python
class Solution(object):
    def diameterOfBinaryTree(self, root):

        """
        :type root: Optional[TreeNode]
        :rtype: int
        """
        self.max = 0

        def maxDepth(root):
            if root == None: return 0
            left = maxDepth(root.left)
            right = maxDepth(root.right)
            self.max = max(self.max, left + right)

            return max(left, right) + 1

        maxDepth(root)
        return self.max
```
~~~

#### Result
Accepted
- Runtime: $16$ ms, beats <font color="#c0504d">17.77%</font>
- Memory: 26.33 MB, beats <font color="#e5b9b7">43.46%</font>

---
## 解題思路
### Traversing the Whole Tree or Not

> [!TIP] **Local optimal ≠ Global optimal**

「區域性比較法」可以提早捕捉很多大型直徑的線索，或是利用剪枝省去檢查時間，但**仍然無法保證找到所有潛藏的深路徑**，尤其是那些跨越兩個深層子樹的情況。所以，即便你只從 A 開始，並比較 A、B、C，也沒辦法保證你已經涵蓋了所有跨子樹的可能性，除非你往每一層繼續做這件事（也就等於在遍歷整棵樹了 ）。

本題對於樹的直徑的定義為：
> 樹中存在一組節點 (A, B) 使得節點 A 至節點 B 的路徑長皆大於或等於任意挑選的一組節點的路徑長。

這句話單用實作方面來說，可能有點抽象，我們用另一個角度來看。
對於某個節點來說，經過某個節點的最長路徑為：
> $\text{其左子樹高度} + \text{其右子樹高度} + 1$

因此，根據上述的資訊，我們不難得知要得出題目所求，只需針對數中每個節點計算出其直徑，並使用一個變數來儲存目前看過的最大直徑值。最後，回傳該值即可。