閱讀此篇筆記之前，請先閱讀：
- [[Binary Tree]]

## Introduction
二元搜尋樹（Binary search tree），是二元樹的一種，其滿足條件如下：
1. 對於根節點，左子樹中所有節點的值 < 根節點的值 < 右子樹中所有節點的值。
2. **任意節點的左、右子樹也是二元搜尋樹**，即同樣滿足條件 `1.` 。

## Operations
### Searching for a node
給定目標節點值 `num` ，可以根據二元搜尋樹的性質來查詢。
我們宣告一個節點 `cur` ，從二元樹的根節點 `root` 出發，迴圈比較節點值 `cur.val` 和 `num` 之間的大小關係。

- 若 `cur.val < num` ，說明目標節點在 `cur` 的右子樹中，因此執行 `cur = cur.right` 。
- 若 `cur.val > num` ，說明目標節點在 `cur` 的左子樹中，因此執行 `cur = cur.left` 。
- 若 `cur.val = num` ，說明找到目標節點，跳出迴圈並返回該節點。

二元搜尋樹的查詢操作與二分搜尋演算法的工作原理一致，都是每輪排除一半情況。迴圈次數最多為二元樹的高度，當二元樹平衡時，使用 $O(\log ⁡n)$ 時間。

~~~tabs
tab: C
```c
/* 查詢節點 */
TreeNode *search(BinarySearchTree *bst, int num) {
    TreeNode *cur = bst->root;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != NULL) {
        if (cur->val < num) {
            // 目標節點在 cur 的右子樹中
            cur = cur->right;
        } else if (cur->val > num) {
            // 目標節點在 cur 的左子樹中
            cur = cur->left;
        } else {
            // 找到目標節點，跳出迴圈
            break;
        }
    }
    // 返回目標節點
    return cur;
}
```

tab: C++
```cpp
/* 查詢節點 */
TreeNode *search(int num) {
    TreeNode *cur = root;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != nullptr) {
        // 目標節點在 cur 的右子樹中
        if (cur->val < num)
            cur = cur->right;
        // 目標節點在 cur 的左子樹中
        else if (cur->val > num)
            cur = cur->left;
        // 找到目標節點，跳出迴圈
        else
            break;
    }
    // 返回目標節點
    return cur;
}
```

tab: Python
```python
def search(self, num: int) -> TreeNode | None:
    """查詢節點"""
    cur = self._root
    # 迴圈查詢，越過葉節點後跳出
    while cur is not None:
        # 目標節點在 cur 的右子樹中
        if cur.val < num:
            cur = cur.right
        # 目標節點在 cur 的左子樹中
        elif cur.val > num:
            cur = cur.left
        # 找到目標節點，跳出迴圈
        else:
            break
    return cur
```
~~~

### Inserting a node
給定一個待插入元素 `num` ，為了保持二元搜尋樹「左子樹 < 根節點 < 右子樹」的性質，插入操作流程如下：

1. **查詢插入位置**
	與查詢操作相似，從根節點出發，根據當前節點值和 `num` 的大小關係迴圈向下搜尋，直到越過葉節點（走訪至 `None` ）時跳出迴圈。
2. **在該位置插入節點**
	初始化節點 `num` ，將該節點置於 `None` 的位置。

在程式碼實現中，需要注意以下兩點：
- 二元搜尋樹不允許存在重複節點，否則將違反其定義。因此，若待插入節點在樹中已存在，則不執行插入，直接返回。
- 為了實現插入節點，我們需要藉助節點 `pre` 儲存上一輪迴圈的節點。這樣在走訪至 `None` 時，我們可以獲取到其父節點，從而完成節點插入操作。

與查詢節點相同，插入節點使用 $O(\log ⁡n)$ 時間。

~~~tabs
tab: C
```c
/* 插入節點 */
void insert(BinarySearchTree *bst, int num) {
    // 若樹為空，則初始化根節點
    if (bst->root == NULL) {
        bst->root = newTreeNode(num);
        return;
    }
    TreeNode *cur = bst->root, *pre = NULL;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != NULL) {
        // 找到重複節點，直接返回
        if (cur->val == num) {
            return;
        }
        pre = cur;
        if (cur->val < num) {
            // 插入位置在 cur 的右子樹中
            cur = cur->right;
        } else {
            // 插入位置在 cur 的左子樹中
            cur = cur->left;
        }
    }
    // 插入節點
    TreeNode *node = newTreeNode(num);
    if (pre->val < num) {
        pre->right = node;
    } else {
        pre->left = node;
    }
}
```

tab: C++
```cpp
/* 插入節點 */
void insert(int num) {
    // 若樹為空，則初始化根節點
    if (root == nullptr) {
        root = new TreeNode(num);
        return;
    }
    TreeNode *cur = root, *pre = nullptr;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != nullptr) {
        // 找到重複節點，直接返回
        if (cur->val == num)
            return;
        pre = cur;
        // 插入位置在 cur 的右子樹中
        if (cur->val < num)
            cur = cur->right;
        // 插入位置在 cur 的左子樹中
        else
            cur = cur->left;
    }
    // 插入節點
    TreeNode *node = new TreeNode(num);
    if (pre->val < num)
        pre->right = node;
    else
        pre->left = node;
}
```

tab: Python
```python
def insert(self, num: int):
    """插入節點"""
    # 若樹為空，則初始化根節點
    if self._root is None:
        self._root = TreeNode(num)
        return
    # 迴圈查詢，越過葉節點後跳出
    cur, pre = self._root, None
    while cur is not None:
        # 找到重複節點，直接返回
        if cur.val == num:
            return
        pre = cur
        # 插入位置在 cur 的右子樹中
        if cur.val < num:
            cur = cur.right
        # 插入位置在 cur 的左子樹中
        else:
            cur = cur.left
    # 插入節點
    node = TreeNode(num)
    if pre.val < num:
        pre.right = node
    else:
        pre.left = node
```
~~~

### Removing a node
先在二元樹中查詢到目標節點，再將其刪除。與插入節點類似，我們需要保證在刪除操作完成後，二元搜尋樹的「左子樹 < 根節點 < 右子樹」的性質仍然滿足。因此，我們根據目標節點的子節點數量，分 $0、1$ 和 $2$ 三種情況，執行對應的刪除節點操作：

- 當待刪除節點的度為 $0$ 時，表示該節點是葉節點，可以直接刪除。
- 當待刪除節點的度為 $1$ 時，將待刪除節點替換為其子節點即可。
- 當待刪除節點的度為 $2$ 時，我們無法直接刪除它，而需要使用一個節點替換該節點。由於要保持二元搜尋樹「左子樹 < 根節點 < 右子樹」的性質，**因此這個節點可以是右子樹的最小節點或左子樹的最大節點**。假設我們選擇右子樹的最小節點（中序走訪的下一個節點），則刪除操作流程如下：
	1. 找到待刪除節點在“中序走訪序列”中的下一個節點，記為 `tmp` 。
	2. 用 `tmp` 的值覆蓋待刪除節點的值，並在樹中遞迴刪除節點 `tmp` 。

刪除節點操作同樣使用 $O(\log ⁡n)$ 時間，其中查詢待刪除節點需要 $O(\log ⁡n)$ 時間，獲取中序走訪後繼節點需要 $O(\log ⁡n)$ 時間。

~~~tabs
tab: C
```c
/* 刪除節點 */
// 由於引入了 stdio.h ，此處無法使用 remove 關鍵詞
void removeItem(BinarySearchTree *bst, int num) {
    // 若樹為空，直接提前返回
    if (bst->root == NULL)
        return;
    TreeNode *cur = bst->root, *pre = NULL;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != NULL) {
        // 找到待刪除節點，跳出迴圈
        if (cur->val == num)
            break;
        pre = cur;
        if (cur->val < num) {
            // 待刪除節點在 root 的右子樹中
            cur = cur->right;
        } else {
            // 待刪除節點在 root 的左子樹中
            cur = cur->left;
        }
    }
    // 若無待刪除節點，則直接返回
    if (cur == NULL)
        return;
    // 判斷待刪除節點是否存在子節點
    if (cur->left == NULL || cur->right == NULL) {
        /* 子節點數量 = 0 or 1 */
        // 當子節點數量 = 0 / 1 時， child = nullptr / 該子節點
        TreeNode *child = cur->left != NULL ? cur->left : cur->right;
        // 刪除節點 cur
        if (pre->left == cur) {
            pre->left = child;
        } else {
            pre->right = child;
        }
        // 釋放記憶體
        free(cur);
    } else {
        /* 子節點數量 = 2 */
        // 獲取中序走訪中 cur 的下一個節點
        TreeNode *tmp = cur->right;
        while (tmp->left != NULL) {
            tmp = tmp->left;
        }
        int tmpVal = tmp->val;
        // 遞迴刪除節點 tmp
        removeItem(bst, tmp->val);
        // 用 tmp 覆蓋 cur
        cur->val = tmpVal;
    }
}
```

tab: C++
```cpp
/* 刪除節點 */
void remove(int num) {
    // 若樹為空，直接提前返回
    if (root == nullptr)
        return;
    TreeNode *cur = root, *pre = nullptr;
    // 迴圈查詢，越過葉節點後跳出
    while (cur != nullptr) {
        // 找到待刪除節點，跳出迴圈
        if (cur->val == num)
            break;
        pre = cur;
        // 待刪除節點在 cur 的右子樹中
        if (cur->val < num)
            cur = cur->right;
        // 待刪除節點在 cur 的左子樹中
        else
            cur = cur->left;
    }
    // 若無待刪除節點，則直接返回
    if (cur == nullptr)
        return;
    // 子節點數量 = 0 or 1
    if (cur->left == nullptr || cur->right == nullptr) {
        // 當子節點數量 = 0 / 1 時， child = nullptr / 該子節點
        TreeNode *child = cur->left != nullptr ? cur->left : cur->right;
        // 刪除節點 cur
        if (cur != root) {
            if (pre->left == cur)
                pre->left = child;
            else
                pre->right = child;
        } else {
            // 若刪除節點為根節點，則重新指定根節點
            root = child;
        }
        // 釋放記憶體
        delete cur;
    }
    // 子節點數量 = 2
    else {
        // 獲取中序走訪中 cur 的下一個節點
        TreeNode *tmp = cur->right;
        while (tmp->left != nullptr) {
            tmp = tmp->left;
        }
        int tmpVal = tmp->val;
        // 遞迴刪除節點 tmp
        remove(tmp->val);
        // 用 tmp 覆蓋 cur
        cur->val = tmpVal;
    }
}
```

tab: Python
```python
def remove(self, num: int):
    """刪除節點"""
    # 若樹為空，直接提前返回
    if self._root is None:
        return
    # 迴圈查詢，越過葉節點後跳出
    cur, pre = self._root, None
    while cur is not None:
        # 找到待刪除節點，跳出迴圈
        if cur.val == num:
            break
        pre = cur
        # 待刪除節點在 cur 的右子樹中
        if cur.val < num:
            cur = cur.right
        # 待刪除節點在 cur 的左子樹中
        else:
            cur = cur.left
    # 若無待刪除節點，則直接返回
    if cur is None:
        return

    # 子節點數量 = 0 or 1
    if cur.left is None or cur.right is None:
        # 當子節點數量 = 0 / 1 時， child = null / 該子節點
        child = cur.left or cur.right
        # 刪除節點 cur
        if cur != self._root:
            if pre.left == cur:
                pre.left = child
            else:
                pre.right = child
        else:
            # 若刪除節點為根節點，則重新指定根節點
            self._root = child
    # 子節點數量 = 2
    else:
        # 獲取中序走訪中 cur 的下一個節點
        tmp: TreeNode = cur.right
        while tmp.left is not None:
            tmp = tmp.left
        # 遞迴刪除節點 tmp
        self.remove(tmp.val)
        # 用 tmp 覆蓋 cur
        cur.val = tmp.val
```
~~~

### In-order traversal
二元樹的中序走訪遵循「左 → 根 → 右」的走訪順序，而二元搜尋樹滿足「左子節點 < 根節點 < 右子節點」的大小關係。這意味著在二元搜尋樹中進行中序走訪時，總是會優先走訪下一個最小節點，從而得出一個重要性質：**二元搜尋樹的中序走訪序列是升序的**。

利用中序走訪升序的性質，我們在二元搜尋樹中獲取有序資料僅需 $O(n)$ 時間，無須進行額外的排序操作，非常高效。

## Efficiency
給定一組資料，我們考慮使用陣列或二元搜尋樹儲存。觀察下表，二元搜尋樹的各項操作的時間複雜度都是對數階，具有穩定且高效的效能。只有在高頻新增、低頻查詢刪除資料的場景下，陣列比二元搜尋樹的效率更高。

|      | Unsorted array | Binary search tree |
| ---- | :------------: | :----------------: |
| 查詢元素 |     $O(n)$     |    $O(\log n)$     |
| 插入元素 |     $O(1)$     |    $O(\log n)$     |
| 刪除元素 |     $O(n)$     |    $O(\log n)$     |

在理想情況下，二元搜尋樹是“平衡”的，這樣就可以在 $\log⁡ n$ 輪迴圈內查詢任意節點。然而，如果我們在二元搜尋樹中不斷地插入和刪除節點，可能導致二元樹退化為鏈結串列，這時各種操作的時間複雜度也會退化為 $O(n)$ 。

## Typical Applications
- 用作系統中的多級索引，實現高效的查詢、插入、刪除操作。
- 作為某些搜尋演算法的底層資料結構。
- 用於儲存資料流，以保持其有序狀態。

## Q & A
> [!QUESTION]- 如何從一組輸入資料構建一棵二元搜尋樹？根節點的選擇是不是很重要？
> 是的，構建樹的方法已在二元搜尋樹程式碼中的 `build_tree()` 方法中給出。至於根節點的選擇，我們通常會將輸入資料排序，然後將中點元素作為根節點，再遞迴地構建左右子樹。這樣做可以最大程度保證樹的平衡性。

> [!QUESTION]- 廣度優先走訪到最底層之前，佇列中的節點數量是 $2^h$ 嗎？
> 是的，例如高度 $h=2$ 的滿二元樹，其節點總數 $n=7$ ，則底層節點數量 $4=2^h=(n+1)/2$ 。
