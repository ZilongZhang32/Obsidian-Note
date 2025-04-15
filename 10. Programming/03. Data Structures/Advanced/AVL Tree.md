閱讀此篇筆記之前，請先閱讀：
- [[Binary Tree]]
- [[Binary Search Tree]]

## Introduction
在「二元搜尋樹」章節中我們提到，在多次插入和刪除操作後，二元搜尋樹可能退化為鏈結串列。在這種情況下，所有操作的時間複雜度將從 $O(\log ⁡n)$ 劣化為 $O(n)$ 。

1962 年 G. M. Adelson-Velsky 和 E. M. Landis 在論文 “An algorithm for the organization of information” 中提出了 AVL 樹。論文中詳細描述了一系列操作，確保在持續新增和刪除節點後，AVL 樹不會退化，從而使得各種操作的時間複雜度保持在 $O(\log ⁡n)$ 級別。換句話說，在需要頻繁進行增刪查改操作的場景中，AVL 樹能始終保持高效的資料操作效能，具有很好的應用價值。

## Common Terminology
AVL 樹既是二元搜尋樹，也是平衡二元樹，同時滿足這兩類二元樹的所有性質，因此是一種平衡二元搜尋樹（Balanced binary search tree）。

### Node height
由於 AVL 樹的相關操作需要獲取節點高度，因此我們需要為節點類別新增 `height` 變數。

~~~tabs
tab: C
```c
/* AVL 樹節點結構體 */
typedef struct TreeNode {
    int val;
    int height;
    struct TreeNode *left;
    struct TreeNode *right;
} TreeNode;

/* 建構子 */
TreeNode *newTreeNode(int val) {
    TreeNode *node;

    node = (TreeNode *)malloc(sizeof(TreeNode));
    node->val = val;
    node->height = 0;
    node->left = NULL;
    node->right = NULL;
    return node;
}
```

tab: C++
```cpp
/* AVL 樹節點類別 */
struct TreeNode {
    int val{};          // 節點值
    int height = 0;     // 節點高度
    TreeNode *left{};   // 左子節點
    TreeNode *right{};  // 右子節點
    TreeNode() = default;
    explicit TreeNode(int x) : val(x){}
};
```

tab: Python
```python
class TreeNode:
    """AVL 樹節點類別"""
    def __init__(self, val: int):
        self.val: int = val                 # 節點值
        self.height: int = 0                # 節點高度
        self.left: TreeNode | None = None   # 左子節點引用
        self.right: TreeNode | None = None  # 右子節點引用
```
~~~

「節點高度」是指從該節點到它的最遠葉節點的距離，即所經過的「邊」的數量。需要特別注意的是，葉節點的高度為 $0$ ，而空節點的高度為 $−1$ 。我們將建立兩個工具函式，分別用於獲取和更新節點的高度：

~~~~tabs

tab: C
```c
/* 獲取節點高度 */
int height(TreeNode *node) {
    // 空節點高度為 -1 ，葉節點高度為 0
    if (node != NULL) {
        return node->height;
    }
    return -1;
}

/* 更新節點高度 */
void updateHeight(TreeNode *node) {
    int lh = height(node->left);
    int rh = height(node->right);
    // 節點高度等於最高子樹高度 + 1
    if (lh > rh) {
        node->height = lh + 1;
    } else {
        node->height = rh + 1;
    }
}
```
tab: C++
```cpp
/* 獲取節點高度 */
int height(TreeNode *node) {
    // 空節點高度為 -1 ，葉節點高度為 0
    return node == nullptr ? -1 : node->height;
}

/* 更新節點高度 */
void updateHeight(TreeNode *node) {
    // 節點高度等於最高子樹高度 + 1
    node->height = max(height(node->left), height(node->right)) + 1;
}
```
tab: Python
```python
def height(self, node: TreeNode | None) -> int:
    """獲取節點高度"""
    # 空節點高度為 -1 ，葉節點高度為 0
    if node is not None:
        return node.height
    return -1

def update_height(self, node: TreeNode | None):
    """更新節點高度"""
    # 節點高度等於最高子樹高度 + 1
    node.height = max([self.height(node.left), self.height(node.right)]) + 1
```
~~~~

### Node balance factor
節點的平衡因子（balance factor）定義為節點左子樹的高度減去右子樹的高度，同時規定空節點的平衡因子為 $0$ 。我們同樣將獲取節點平衡因子的功能封裝成函式，方便後續使用：

~~~tabs
tab: C
```c
/* 獲取平衡因子 */
int balanceFactor(TreeNode *node) {
    // 空節點平衡因子為 0
    if (node == NULL) {
        return 0;
    }
    // 節點平衡因子 = 左子樹高度 - 右子樹高度
    return height(node->left) - height(node->right);
}
```

tab: C++
```cpp
/* 獲取平衡因子 */
int balanceFactor(TreeNode *node) {
    // 空節點平衡因子為 0
    if (node == nullptr)
        return 0;
    // 節點平衡因子 = 左子樹高度 - 右子樹高度
    return height(node->left) - height(node->right);
}
```

tab: Python
```python
def balance_factor(self, node: TreeNode | None) -> int:
    """獲取平衡因子"""
    # 空節點平衡因子為 0
    if node is None:
        return 0
    # 節點平衡因子 = 左子樹高度 - 右子樹高度
    return self.height(node.left) - self.height(node.right)
```
~~~

> [!TIP]
> 設平衡因子為 $f$ ，則一棵 AVL 樹的任意節點的平衡因子皆滿足 $−1≤f≤1$ 。

## Rotations
AVL 樹的特點在於「旋轉」操作，它能夠在不影響二元樹的中序走訪序列的前提下，使失衡節點重新恢復平衡。換句話說，**旋轉操作既能保持「二元搜尋樹」的性質，也能使樹重新變為「平衡二元樹」**。

我們將平衡因子絕對值 $>1$ 的節點稱為「失衡節點」。根據節點失衡情況的不同，旋轉操作分為四種：右旋、左旋、先右旋後左旋、先左旋後右旋。下面詳細介紹這些旋轉操作。
### Right rotation
~~~tabs
tab: 情形一
我們關注以該失衡節點為根節點的子樹，將該節點記為 `node` ，其左子節點記為 `child` ，執行「右旋」操作。完成右旋後，子樹恢復平衡，並且仍然保持二元搜尋樹的性質。


tab: 情形二
當節點 `child` 有右子節點（記為 `grand_child` ）時，需要在右旋中新增一步：將 `grand_child` 作為 `node` 的左子節點。

~~~

~~~tabs
tab: C
```c
/* 右旋操作 */
TreeNode *rightRotate(TreeNode *node) {
    TreeNode *child, *grandChild;
    child = node->left;
    grandChild = child->right;
    // 以 child 為原點，將 node 向右旋轉
    child->right = node;
    node->left = grandChild;
    // 更新節點高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋轉後子樹的根節點
    return child;
}
```

tab: C++
```cpp
/* 右旋操作 */
TreeNode *rightRotate(TreeNode *node) {
    TreeNode *child = node->left;
    TreeNode *grandChild = child->right;
    // 以 child 為原點，將 node 向右旋轉
    child->right = node;
    node->left = grandChild;
    // 更新節點高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋轉後子樹的根節點
    return child;
}
```

tab: Python
```python
def right_rotate(self, node: TreeNode | None) -> TreeNode | None:
    """右旋操作"""
    child = node.left
    grand_child = child.right
    # 以 child 為原點，將 node 向右旋轉
    child.right = node
    node.left = grand_child
    # 更新節點高度
    self.update_height(node)
    self.update_height(child)
    # 返回旋轉後子樹的根節點
    return child
```
~~~

### Left rotation
~~~tabs
tab: 情形一
我們關注以該失衡節點為根節點的子樹，將該節點記為 `node` ，其右子節點記為 `child` ，執行「左旋」操作。完成左旋後，子樹恢復平衡，並且仍然保持二元搜尋樹的性質。


tab: 情形二
當節點 `child` 有左子節點（記為 `grand_child` ）時，需要在左旋中新增一步：將 `grand_child` 作為 `node` 的右子節點。

~~~

基於對稱性，我們只需將右旋的實現程式碼中的所有的 `left` 替換為 `right` ，將所有的 `right` 替換為 `left` ，即可得到左旋的實現程式碼：
~~~tabs
tab: C
```c
/* 左旋操作 */
TreeNode *leftRotate(TreeNode *node) {
    TreeNode *child, *grandChild;
    child = node->right;
    grandChild = child->left;
    // 以 child 為原點，將 node 向左旋轉
    child->left = node;
    node->right = grandChild;
    // 更新節點高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋轉後子樹的根節點
    return child;
}
```

tab: C++
```cpp
/* 左旋操作 */
TreeNode *leftRotate(TreeNode *node) {
    TreeNode *child = node->right;
    TreeNode *grandChild = child->left;
    // 以 child 為原點，將 node 向左旋轉
    child->left = node;
    node->right = grandChild;
    // 更新節點高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋轉後子樹的根節點
    return child;
}
```

tab: Python
```python
def left_rotate(self, node: TreeNode | None) -> TreeNode | None:
    """左旋操作"""
    child = node.right
    grand_child = child.left
    # 以 child 為原點，將 node 向左旋轉
    child.left = node
    node.right = grand_child
    # 更新節點高度
    self.update_height(node)
    self.update_height(child)
    # 返回旋轉後子樹的根節點
    return child
```
~~~

### Left-right rotation
如果僅使用左旋或右旋都無法使子樹恢復平衡時，就需要先對 `child` 執行「左旋」，再對 `node` 執行「右旋」。

### Right-left rotation
同上。不過，此類旋轉需要先對 `child` 執行「右旋」，再對 `node` 執行「左旋」。

### Choice of rotation
如下表所示，我們透過判斷失衡節點的平衡因子以及較高一側子節點的平衡因子的正負號，來確定失衡節點屬於哪種情況。

| Unbalanced node's $f$ | Child node's $f$ | Rotation type |
| --------------------- | ---------------- | ------------- |
| $>1$（左偏）              | $≥0$             | 右旋            |
| $>1$（左偏）              | $<0$             | 左旋->右旋        |
| $< -1$（右偏）            | $≤0$             | 左旋            |
| $< -1$（右偏）            | $>0$             | 右旋->左旋        |

為了便於使用，我們將旋轉操作封裝成一個函式。**有了這個函式，我們就能對各種失衡情況進行旋轉，使失衡節點重新恢復平衡，而不需要另外判斷失衡情形**。

~~~tabs
tab: C
```c
/* 執行旋轉操作，使該子樹重新恢復平衡 */
TreeNode *rotate(TreeNode *node) {
    // 獲取節點 node 的平衡因子
    int bf = balanceFactor(node);
    // 左偏樹
    if (bf > 1) {
        if (balanceFactor(node->left) >= 0) {
            // 右旋
            return rightRotate(node);
        } else {
            // 先左旋後右旋
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }
    }
    // 右偏樹
    if (bf < -1) {
        if (balanceFactor(node->right) <= 0) {
            // 左旋
            return leftRotate(node);
        } else {
            // 先右旋後左旋
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }
    }
    // 平衡樹，無須旋轉，直接返回
    return node;
}
```

tab: C++
```cpp
/* 執行旋轉操作，使該子樹重新恢復平衡 */
TreeNode *rotate(TreeNode *node) {
    // 獲取節點 node 的平衡因子
    int _balanceFactor = balanceFactor(node);
    // 左偏樹
    if (_balanceFactor > 1) {
        if (balanceFactor(node->left) >= 0) {
            // 右旋
            return rightRotate(node);
        } else {
            // 先左旋後右旋
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }
    }
    // 右偏樹
    if (_balanceFactor < -1) {
        if (balanceFactor(node->right) <= 0) {
            // 左旋
            return leftRotate(node);
        } else {
            // 先右旋後左旋
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }
    }
    // 平衡樹，無須旋轉，直接返回
    return node;
}
```

tab: Python
```python
def rotate(self, node: TreeNode | None) -> TreeNode | None:
    """執行旋轉操作，使該子樹重新恢復平衡"""
    # 獲取節點 node 的平衡因子
    balance_factor = self.balance_factor(node)
    # 左偏樹
    if balance_factor > 1:
        if self.balance_factor(node.left) >= 0:
            # 右旋
            return self.right_rotate(node)
        else:
            # 先左旋後右旋
            node.left = self.left_rotate(node.left)
            return self.right_rotate(node)
    # 右偏樹
    elif balance_factor < -1:
        if self.balance_factor(node.right) <= 0:
            # 左旋
            return self.left_rotate(node)
        else:
            # 先右旋後左旋
            node.right = self.right_rotate(node.right)
            return self.left_rotate(node)
    # 平衡樹，無須旋轉，直接返回
    return node
```
~~~

## Common Operations

### Inserting a node
AVL 樹的節點插入操作與二元搜尋樹在主體上類似。唯一的區別在於，在 AVL 樹中插入節點後，從該節點到根節點的路徑上可能會出現一系列失衡節點。因此，**我們需要從這個節點開始，自底向上執行旋轉操作，使所有失衡節點恢復平衡**。
~~~tabs
tab: C
```c
/* 插入節點 */
void insert(AVLTree *tree, int val) {
    tree->root = insertHelper(tree->root, val);
}

/* 遞迴插入節點（輔助函式） */
TreeNode *insertHelper(TreeNode *node, int val) {
    if (node == NULL) {
        return newTreeNode(val);
    }
    /* 1. 查詢插入位置並插入節點 */
    if (val < node->val) {
        node->left = insertHelper(node->left, val);
    } else if (val > node->val) {
        node->right = insertHelper(node->right, val);
    } else {
        // 重複節點不插入，直接返回
        return node;
    }
    // 更新節點高度
    updateHeight(node);
    /* 2. 執行旋轉操作，使該子樹重新恢復平衡 */
    node = rotate(node);
    // 返回子樹的根節點
    return node;
}
```

tab: C++
```cpp
/* 插入節點 */
void insert(int val) {
    root = insertHelper(root, val);
}

/* 遞迴插入節點（輔助方法） */
TreeNode *insertHelper(TreeNode *node, int val) {
    if (node == nullptr)
        return new TreeNode(val);
    /* 1. 查詢插入位置並插入節點 */
    if (val < node->val)
        node->left = insertHelper(node->left, val);
    else if (val > node->val)
        node->right = insertHelper(node->right, val);
    else
        return node;    // 重複節點不插入，直接返回
    updateHeight(node); // 更新節點高度
    /* 2. 執行旋轉操作，使該子樹重新恢復平衡 */
    node = rotate(node);
    // 返回子樹的根節點
    return node;
}
```

tab: Python
```python
def insert(self, val):
    """插入節點"""
    self._root = self.insert_helper(self._root, val)

def insert_helper(self, node: TreeNode | None, val: int) -> TreeNode:
    """遞迴插入節點（輔助方法）"""
    if node is None:
        return TreeNode(val)
    # 1. 查詢插入位置並插入節點
    if val < node.val:
        node.left = self.insert_helper(node.left, val)
    elif val > node.val:
        node.right = self.insert_helper(node.right, val)
    else:
        # 重複節點不插入，直接返回
        return node
    # 更新節點高度
    self.update_height(node)
    # 2. 執行旋轉操作，使該子樹重新恢復平衡
    return self.rotate(node)
```
~~~

### Removing a node
同理，在二元搜尋樹的刪除節點方法的基礎上，也需要從底至頂執行旋轉操作，使所有失衡節點恢復平衡。
~~~tabs
tab: C
```c
/* 刪除節點 */
// 由於引入了 stdio.h ，此處無法使用 remove 關鍵詞
void removeItem(AVLTree *tree, int val) {
    TreeNode *root = removeHelper(tree->root, val);
}

/* 遞迴刪除節點（輔助函式） */
TreeNode *removeHelper(TreeNode *node, int val) {
    TreeNode *child, *grandChild;
    if (node == NULL) {
        return NULL;
    }
    /* 1. 查詢節點並刪除 */
    if (val < node->val) {
        node->left = removeHelper(node->left, val);
    } else if (val > node->val) {
        node->right = removeHelper(node->right, val);
    } else {
        if (node->left == NULL || node->right == NULL) {
            child = node->left;
            if (node->right != NULL) {
                child = node->right;
            }
            // 子節點數量 = 0 ，直接刪除 node 並返回
            if (child == NULL) {
                return NULL;
            } else {
                // 子節點數量 = 1 ，直接刪除 node
                node = child;
            }
        } else {
            // 子節點數量 = 2 ，則將中序走訪的下個節點刪除，並用該節點替換當前節點
            TreeNode *temp = node->right;
            while (temp->left != NULL) {
                temp = temp->left;
            }
            int tempVal = temp->val;
            node->right = removeHelper(node->right, temp->val);
            node->val = tempVal;
        }
    }
    // 更新節點高度
    updateHeight(node);
    /* 2. 執行旋轉操作，使該子樹重新恢復平衡 */
    node = rotate(node);
    // 返回子樹的根節點
    return node;
}
```

tab: C++
```cpp
/* 刪除節點 */
void remove(int val) {
    root = removeHelper(root, val);
}

/* 遞迴刪除節點（輔助方法） */
TreeNode *removeHelper(TreeNode *node, int val) {
    if (node == nullptr)
        return nullptr;
    /* 1. 查詢節點並刪除 */
    if (val < node->val)
        node->left = removeHelper(node->left, val);
    else if (val > node->val)
        node->right = removeHelper(node->right, val);
    else {
        if (node->left == nullptr || node->right == nullptr) {
            TreeNode *child = node->left != nullptr ? node->left : node->right;
            // 子節點數量 = 0 ，直接刪除 node 並返回
            if (child == nullptr) {
                delete node;
                return nullptr;
            }
            // 子節點數量 = 1 ，直接刪除 node
            else {
                delete node;
                node = child;
            }
        } else {
            // 子節點數量 = 2 ，則將中序走訪的下個節點刪除，並用該節點替換當前節點
            TreeNode *temp = node->right;
            while (temp->left != nullptr) {
                temp = temp->left;
            }
            int tempVal = temp->val;
            node->right = removeHelper(node->right, temp->val);
            node->val = tempVal;
        }
    }
    updateHeight(node); // 更新節點高度
    /* 2. 執行旋轉操作，使該子樹重新恢復平衡 */
    node = rotate(node);
    // 返回子樹的根節點
    return node;
}
```

tab: Python
```python
def remove(self, val: int):
    """刪除節點"""
    self._root = self.remove_helper(self._root, val)

def remove_helper(self, node: TreeNode | None, val: int) -> TreeNode | None:
    """遞迴刪除節點（輔助方法）"""
    if node is None:
        return None
    # 1. 查詢節點並刪除
    if val < node.val:
        node.left = self.remove_helper(node.left, val)
    elif val > node.val:
        node.right = self.remove_helper(node.right, val)
    else:
        if node.left is None or node.right is None:
            child = node.left or node.right
            # 子節點數量 = 0 ，直接刪除 node 並返回
            if child is None:
                return None
            # 子節點數量 = 1 ，直接刪除 node
            else:
                node = child
        else:
            # 子節點數量 = 2 ，則將中序走訪的下個節點刪除，並用該節點替換當前節點
            temp = node.right
            while temp.left is not None:
                temp = temp.left
            node.right = self.remove_helper(node.right, temp.val)
            node.val = temp.val
    # 更新節點高度
    self.update_height(node)
    # 2. 執行旋轉操作，使該子樹重新恢復平衡
    return self.rotate(node)
```
~~~

### Searching for a node
請參閱[[Binary Search Tree#Searching for a node|Binary Search Tree]]。

## Typical Applications
- 組織和儲存大型資料
	適用於高頻查詢、低頻增刪的場景。
- 用於構建資料庫中的索引系統。
- [[Red-Black Tree|紅黑樹]]
	也是一種常見的平衡二元搜尋樹。相較於 AVL 樹，紅黑樹的平衡條件更寬鬆，插入與刪除節點所需的旋轉操作更少，節點增刪操作的平均效率更高。

## Q & A
> [!QUESTION]- 右旋操作是處理失衡節點 `node`、`child`、`grand_child` 之間的關係，那 `node` 的父節點和 `node` 原來的連線不需要維護嗎？右旋操作後豈不是斷掉了？
> 我們需要從遞迴的視角來看這個問題。右旋操作 `right_rotate(root)` 傳入的是子樹的根節點，最終 `return child` 返回旋轉之後的子樹的根節點。子樹的根節點和其父節點的連線是在該函式返回後完成的，不屬於右旋操作的維護範圍。

> [!QUESTION]- 在 C++ 中，函式被劃分到 `private` 和 `public` 中，這方面有什麼考量嗎？為什麼要將 `height()` 函式和 `updateHeight()` 函式分別放在 `public` 和 `private` 中呢？
> 主要看方法的使用範圍，如果方法只在類別內部使用，那麼就設計為 `private` 。例如，使用者單獨呼叫 `updateHeight()` 是沒有意義的，它只是插入、刪除操作中的一步。而 `height()` 是訪問節點高度，類似於 `vector.size()` ，因此設定成 `public` 以便使用。

