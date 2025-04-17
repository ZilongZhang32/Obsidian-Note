#Easy 
## Problem Description
Given the `root` of a binary tree, invert the tree, and return _its root_.

### Examples
#### 1
> **Input:** `root = [4,2,7,1,3,6,9]`
> **Output:** `[4,7,2,9,6,3,1]`
> **Explanation:** ![[inverted_binary_tree_example_1.png]]
#### 2
> **Input:** `root = [2,1,3]`
> **Output:** `[2,3,1]`
#### 3
> **Input:** `root = []`
> **Output:** `[]`

### Constraints
- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`

### Follow-up
None.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Binary Tree]]
> 	- [[Binary Tree#Pre-order , in-order, and posr-order traversal|Depth-First Search]]
> 	- [[Binary Tree#Level-order traversal|Breadth-First Search]]
> - [[Recursion]]

---
## 我的答案
### Attempt 1
~~~tabs
tab: Python
```python
def invertTree(self, root):
    """
    :type root: Optional[TreeNode]
    :rtype: Optional[TreeNode]
    """
    if root:
        temp = root.left
        root.left = root.right
        root.right = temp

        self.invertTree(root.left)
        self.invertTree(root.right)

        return root

    else:
        return None
```
~~~

#### Result
Accepted
- Runtime: $0$ ms, beats 100.00%
- Memory: $12.47$ MB, beats 62.11%

---
## 解題思路
### Recursion
本題只須將左子樹與右子數互相交換位置即可達成題目要求。因此我們逐步交換即可。
而遞迴可以將解題方法歸納為「自頂向下、逐層交換左右子樹」並將程式碼精簡化，這解釋了為何遞迴是本題的思路核心。

步驟如下：
1. **基礎情境（Base Case）**
	- 當節點為 nullptr（空節點）時，直接返回，不做任何處理。
2. **遞迴步驟（Recursive Case）**
	- **交換** 當前節點的左子樹和右子樹。
	- **對左子樹遞迴執行反轉**。
	- **對右子樹遞迴執行反轉**。
3. **遞迴結束後，返回根節點**（最上層的節點）。

#### 時間與空間複雜度
- **時間複雜度：$O(n)$**
	每個節點都被訪問一次
- **空間複雜度：$O(h)$**
	遞迴函式的呼叫棧，h 為樹的高度，在最壞情況下為 $O(n)$