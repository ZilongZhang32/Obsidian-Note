## Introduction
二元樹（binary tree）是一種非線性資料結構，代表「祖先」與「後代」之間的派生關係，體現了「一分為二」的分治邏輯。與鏈結串列類似，二元樹的基本單元是節點，每個節點包含值、左子節點引用和右子節點引用。

~~~tabs
tab: C
```c
/* 二元樹節點結構體 */
typedef struct TreeNode {
    int val;                // 節點值
    int height;             // 節點高度
    struct TreeNode *left;  // 左子節點指標
    struct TreeNode *right; // 右子節點指標
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
/* 二元樹節點結構體 */
struct TreeNode {
    int val;          // 節點值
    TreeNode *left;   // 左子節點指標
    TreeNode *right;  // 右子節點指標
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

tab: Python
```python
class TreeNode:
    """二元樹節點類別"""
    def __init__(self, val: int):
        self.val: int = val                # 節點值
        self.left: TreeNode | None = None  # 左子節點引用
        self.right: TreeNode | None = None # 右子節點引用
```
~~~

每個節點都有兩個引用（指標），分別指向左子節點（left-child node）和右子節點（right-child node），該節點被稱為這兩個子節點的父節點（parent node）。當給定一個二元樹的節點時，我們將該節點的左子節點及其以下節點形成的樹稱為該節點的左子樹（left subtree），同理可得右子樹（right subtree）。

**在二元樹中，除葉節點外，其他所有節點都包含子節點和非空子樹。**
如圖 7-1 所示，如果將「節點 2」視為父節點，則其左子節點和右子節點分別是「節點 4」和「節點 5」，左子樹是「節點 4 及其以下節點形成的樹」，右子樹是「節點 5 及其以下節點形成的樹」。

![[binary_tree_definition.png]]

## Terminology
- 根節點，根（root node）
	位於二元樹頂層的節點，沒有父節點。
- 葉節點，葉子（leaf node）
	沒有子節點的節點，其兩個指標均指向 `None` 。
- 邊（edge）
	連線兩個節點的線段，即節點引用（指標）。
- 層（level）
	從頂至底遞增，根節點所在層為 $1$ 。
- 度（degree）
	節點的子節點的數量。在二元樹中，度的取值範圍是 $0、1、2$ 。
- 二元樹的高度，二元樹的深度（height / depth of the binary tree）
	從根節點到最遠葉節點所經過的邊的數量。
- 節點的深度（depth of the node）
	從根節點到該節點所經過的邊的數量
- 節點的高度（height of the node）。
	從距離該節點最遠的葉節點到該節點所經過的邊的數量。

> [!TIP] 術語差異
> 我們通常將「高度」和「深度」定義為「經過的『邊』的數量」，但有些題目或教材可能會將其定義為「經過的『節點』的數量」。在這種情況下，高度和深度都需要加 $1$ 。

## Common Opreations
### Initializing
與鏈結串列類似，首先初始化節點，然後構建引用（指標）。
~~~tabs
tab: C
```c
/* 初始化二元樹 */
// 初始化節點
TreeNode *n1 = newTreeNode(1);
TreeNode *n2 = newTreeNode(2);
TreeNode *n3 = newTreeNode(3);
TreeNode *n4 = newTreeNode(4);
TreeNode *n5 = newTreeNode(5);
// 構建節點之間的引用（指標）
n1->left = n2;
n1->right = n3;
n2->left = n4;
n2->right = n5;
```

tab: C++
```cpp
/* 初始化二元樹 */
// 初始化節點
TreeNode* n1 = new TreeNode(1);
TreeNode* n2 = new TreeNode(2);
TreeNode* n3 = new TreeNode(3);
TreeNode* n4 = new TreeNode(4);
TreeNode* n5 = new TreeNode(5);
// 構建節點之間的引用（指標）
n1->left = n2;
n1->right = n3;
n2->left = n4;
n2->right = n5;
```

tab: Python
```python
# 初始化二元樹
# 初始化節點
n1 = TreeNode(val=1)
n2 = TreeNode(val=2)
n3 = TreeNode(val=3)
n4 = TreeNode(val=4)
n5 = TreeNode(val=5)
# 構建節點之間的引用（指標）
n1.left = n2
n1.right = n3
n2.left = n4
n2.right = n5
```
~~~

### Inserting/Removing nodes
與鏈結串列類似，在二元樹中插入與刪除節點可以透過修改指標來實現。
~~~tabs
tab: C
```c
/* 插入與刪除節點 */
TreeNode *P = newTreeNode(0);
// 在 n1 -> n2 中間插入節點 P
n1->left = P;
P->left = n2;
// 刪除節點 P
n1->left = n2;
// 釋放記憶體
free(P);
```

tab: C++
```cpp
/* 插入與刪除節點 */
TreeNode* P = new TreeNode(0);
// 在 n1 -> n2 中間插入節點 P
n1->left = P;
P->left = n2;
// 刪除節點 P
n1->left = n2;
// 釋放記憶體
delete P;
```

tab: Python
```python
# 插入與刪除節點
P = TreeNode(0)
# 在 n1 -> n2 中間插入節點 P
n1.left = P
P.left = n2
# 刪除節點 P
n1.left = n2
# 釋放記憶體
del P
```
~~~

## Common Types of Binary Trees
### Perfect binary tree
滿二元樹，或完美二元樹（Perfect binary tree）所有層的節點都被完全填滿。在滿二元樹中，葉節點的度為 $0$，其餘所有節點的度都為 $2$；若樹的高度為 $h$，則節點總數為 $2^{h+1}-1$，呈現標準的指數級關係，反映了自然界中常見的細胞分裂現象。一言以蔽之就是除了葉節點外，每個節點都有 $2$ 個子節點。

對於一棵深度為 $k$ 的滿二元樹：
- 共有 $2^{k+1}−1$ 個節點
- 節點個數一定為奇數
- 第 $i$ 層有 $2^{i−1}$ 個節點
- 有 $2^k$ 個葉節點
 
### Complete binary tree
完全二元樹（Complete binary tree）只有最底層的節點未被填滿，且最底層節點儘量靠左填充。
> [!TIP] 請注意，滿二元樹也是一棵完全二元樹。

- 對於一棵具有 $n$ 個節點的完全二元樹，其高度為 $log_2{n}+1$
- 高度為 $k$ 的完全二元樹，其至少有 $2^k-1$ 個節點，至多有 $2^k+1$ 個節點

### Full binary tree
完滿二元樹（Full binary tree）除了葉節點之外，其餘所有節點都有兩個子節點。

### Balanced binary tree
平衡二元樹（Balanced binary tree）中任意節點的左子樹和右子樹的高度之差的絕對值不超過 $1$ 。
![[balanced_binary_tree.png]]

### Degenerate binary tree
退化二元樹（Degenerate binary tree）中，絕大多數的節點都只有一個子節點。用另一個角度來看的話，這種二元樹可以說是退化為「鏈結串列」，此時各項操作都變為線性操作，時間複雜度退化至 $O(n)$ 。

對於一棵深度為 $k$ 的退化二元樹：
- 共有 $k+1$ 個節點
- 第 $i$ 層有 $1$ 個節點
- 有 $1$ 個葉節點

## Binary Tree Traversal
從物理結構的角度來看，樹是一種基於鏈結串列的資料結構，因此其走訪方式是透過指標逐個訪問節點。然而，樹是一種非線性資料結構，這使得走訪樹比走訪鏈結串列更加複雜，需要藉助搜尋演算法來實現。

二元樹常見的走訪方式包括：
- 廣度優先：它體現了一種「一圈一圈向外擴展」的逐層走訪方式
	- 層序走訪
- 深度優先：深度優先走訪就像是繞著整棵二元樹的外圍「走」一圈
	- 前序走訪
	- 中序走訪
	- 後序走訪
### Level-order traversal
層序走訪（Level-order traversal）從頂部到底部逐層走訪二元樹，並在每一層按照從左到右的順序訪問節點。層序走訪本質上屬於廣度優先走訪（Breadth-first traversal），也稱廣度優先搜尋（Breadth-first search, BFS）。

廣度優先走訪通常藉助「[[Queue|佇列]]」來實現。佇列遵循「先進先出」的規則，而廣度優先走訪則遵循「逐層推進」的規則，兩者背後的思想是一致的。

~~~tabs
tab: C
```c
/* 層序走訪 */
int *levelOrder(TreeNode *root, int *size) {
    /* 輔助佇列 */
    int front, rear;
    int index, *arr;
    TreeNode *node;
    TreeNode **queue;

    /* 輔助佇列 */
    queue = (TreeNode **)malloc(sizeof(TreeNode *) * MAX_SIZE);
    // 佇列指標
    front = 0, rear = 0;
    // 加入根節點
    queue[rear++] = root;
    // 初始化一個串列，用於儲存走訪序列
    /* 輔助陣列 */
    arr = (int *)malloc(sizeof(int) * MAX_SIZE);
    // 陣列指標
    index = 0;
    while (front < rear) {
        // 隊列出隊
        node = queue[front++];
        // 儲存節點值
        arr[index++] = node->val;
        if (node->left != NULL) {
            // 左子節點入列
            queue[rear++] = node->left;
        }
        if (node->right != NULL) {
            // 右子節點入列
            queue[rear++] = node->right;
        }
    }
    // 更新陣列長度的值
    *size = index;
    arr = realloc(arr, sizeof(int) * (*size));

    // 釋放輔助陣列空間
    free(queue);
    return arr;
}
```

tab: C++
```cpp
/* 層序走訪 */
vector<int> levelOrder(TreeNode *root) {
    // 初始化佇列，加入根節點
    queue<TreeNode *> queue;
    queue.push(root);
    // 初始化一個串列，用於儲存走訪序列
    vector<int> vec;
    while (!queue.empty()) {
        TreeNode *node = queue.front();
        queue.pop();              // 隊列出隊
        vec.push_back(node->val); // 儲存節點值
        if (node->left != nullptr)
            queue.push(node->left); // 左子節點入列
        if (node->right != nullptr)
            queue.push(node->right); // 右子節點入列
    }
    return vec;
}
```

tab: Python
```python
def level_order(root: TreeNode | None) -> list[int]:
    """層序走訪"""
    # 初始化佇列，加入根節點
    queue: deque[TreeNode] = deque()
    queue.append(root)
    # 初始化一個串列，用於儲存走訪序列
    res = []
    while queue:
        node: TreeNode = queue.popleft()  # 隊列出隊
        res.append(node.val)  # 儲存節點值
        if node.left is not None:
            queue.append(node.left)  # 左子節點入列
        if node.right is not None:
            queue.append(node.right)  # 右子節點入列
    return res
```
~~~

#### Complexity Analysis
##### Time complexity
所有節點被訪問一次，使用 $O(n)$ 時間，其中 $n$ 為節點數量。
##### Space complexity
在最差情況下，即滿二元樹時，走訪到最底層之前，佇列中最多同時存在 $\frac{n+1}{2}$ 個節點，佔用 $O(n)$ 空間。

### Pre-order , in-order, and posr-order traversal
- 前序走訪
	在即將訪問左子樹時記錄節點。
- 中序走訪
	訪問完左子樹，即將訪問右子樹時記錄節點。
- 後序走訪
	左右子樹皆訪問完，返回函式時記錄節點。

~~~tabs
tab: C
```c
/* 前序走訪 */
void preOrder(TreeNode *root, int *size) {
    if (root == NULL)
        return;
    // 訪問優先順序：根節點 -> 左子樹 -> 右子樹
    arr[(*size)++] = root->val;
    preOrder(root->left, size);
    preOrder(root->right, size);
}

/* 中序走訪 */
void inOrder(TreeNode *root, int *size) {
    if (root == NULL)
        return;
    // 訪問優先順序：左子樹 -> 根節點 -> 右子樹
    inOrder(root->left, size);
    arr[(*size)++] = root->val;
    inOrder(root->right, size);
}

/* 後序走訪 */
void postOrder(TreeNode *root, int *size) {
    if (root == NULL)
        return;
    // 訪問優先順序：左子樹 -> 右子樹 -> 根節點
    postOrder(root->left, size);
    postOrder(root->right, size);
    arr[(*size)++] = root->val;
}
```

tab: C++
```cpp
/* 前序走訪 */
void preOrder(TreeNode *root) {
    if (root == nullptr)
        return;
    // 訪問優先順序：根節點 -> 左子樹 -> 右子樹
    vec.push_back(root->val);
    preOrder(root->left);
    preOrder(root->right);
}

/* 中序走訪 */
void inOrder(TreeNode *root) {
    if (root == nullptr)
        return;
    // 訪問優先順序：左子樹 -> 根節點 -> 右子樹
    inOrder(root->left);
    vec.push_back(root->val);
    inOrder(root->right);
}

/* 後序走訪 */
void postOrder(TreeNode *root) {
    if (root == nullptr)
        return;
    // 訪問優先順序：左子樹 -> 右子樹 -> 根節點
    postOrder(root->left);
    postOrder(root->right);
    vec.push_back(root->val);
}
```

tab: Python
```python
def pre_order(root: TreeNode | None):
    """前序走訪"""
    if root is None:
        return
    # 訪問優先順序：根節點 -> 左子樹 -> 右子樹
    res.append(root.val)
    pre_order(root=root.left)
    pre_order(root=root.right)

def in_order(root: TreeNode | None):
    """中序走訪"""
    if root is None:
        return
    # 訪問優先順序：左子樹 -> 根節點 -> 右子樹
    in_order(root=root.left)
    res.append(root.val)
    in_order(root=root.right)

def post_order(root: TreeNode | None):
    """後序走訪"""
    if root is None:
        return
    # 訪問優先順序：左子樹 -> 右子樹 -> 根節點
    post_order(root=root.left)
    post_order(root=root.right)
    res.append(root.val)
```
~~~

#### Complexity Analysis
##### Time complexity
所有節點被訪問一次，使用 $O(n)$ 時間，其中 $n$ 為節點數量。
##### Space complexity
在最差情況下，即樹為退化二元樹，遞迴深度達到 $n$，佔用 $O(n)$ 的堆疊幀空間。

## Array Representation
在鏈結串列表示下，二元樹的儲存單元為節點 `TreeNode` ，節點之間透過指標相連線。上一節介紹了鏈結串列表示下的二元樹的各項基本操作。

那麼，我們能否用陣列來表示二元樹呢？答案是肯定的。
~~~tabs
tab: C
```c
/* 二元樹的陣列表示 */
// 使用 int 最大值標記空位，因此要求節點值不能為 INT_MAX
int tree[] = {1, 2, 3, 4, INT_MAX, 6, 7, 8, 9, INT_MAX, INT_MAX, 12, INT_MAX, INT_MAX, 15};
```

tab: C++
```cpp
/* 二元樹的陣列表示 */
// 使用 int 最大值 INT_MAX 標記空位
vector<int> tree = {1, 2, 3, 4, INT_MAX, 6, 7, 8, 9, INT_MAX, INT_MAX, 12, INT_MAX, INT_MAX, 15};
```

tab: Python
```python
# 二元樹的陣列表示
# 使用 None 來表示空位
tree = [1, 2, 3, 4, None, 6, 7, 8, 9, None, None, 12, None, None, 15]
```
~~~
值得說明的是，**完全二元樹非常適合使用陣列來表示**。回顧完全二元樹的定義，`None` 只出現在最底層且靠右的位置，**因此所有 `None` 一定出現在層序走訪序列的末尾**。這意味著使用陣列表示完全二元樹時，可以省略儲存所有 `None`。

以一棵完美二元樹為例，我們將所有節點按照層序走訪的順序儲存在一個陣列中，則每個節點都對應唯一的陣列索引。根據層序走訪的特性，我們可以推導出父節點索引與子節點索引之間的「對映公式」：**若某節點的索引為 $i$ ，則該節點的左子節點索引為 $2i+1$ ，右子節點索引為 $2i+2$** 。

> [!QUESTION]- 如何獲取節點 $i$ 的父節點的索引？
> 我們可以畫出一棵任意的完全二元樹，並遵循廣度優先法依序標示節點索引。不難觀察出可以透過公式 $\lfloor (i-1)/2 \rfloor$  計算出父節點的索引。 
> 
> 當 $i=3$，則其父節點索引為 $\lfloor (3-1)/2 \rfloor = \lfloor 1 \rfloor = 1$
> 當 $i=4$，則其父節點索引為 $\lfloor (4-1)/2 \rfloor = \lfloor 1.5 \rfloor = 1$。
> 程式實作方面，只須使用向下整除 `/ or //` 即可達到上述公式的效果。
> 
> 從上述範例可以發現，這兩個節點的父節點是一樣的。

以下程式碼實現了一棵基於陣列表示的二元樹及以下操作：
- 給定某節點，獲取它的值、左（右）子節點、父節點。
- 獲取前序走訪、中序走訪、後序走訪、層序走訪序列。

~~~tabs
tab: C
```c
/* 陣列表示下的二元樹結構體 */
typedef struct {
    int *tree;
    int size;
} ArrayBinaryTree;

/* 建構子 */
ArrayBinaryTree *newArrayBinaryTree(int *arr, int arrSize) {
    ArrayBinaryTree *abt = (ArrayBinaryTree *)malloc(sizeof(ArrayBinaryTree));
    abt->tree = malloc(sizeof(int) * arrSize);
    memcpy(abt->tree, arr, sizeof(int) * arrSize);
    abt->size = arrSize;
    return abt;
}

/* 析構函式 */
void delArrayBinaryTree(ArrayBinaryTree *abt) {
    free(abt->tree);
    free(abt);
}

/* 串列容量 */
int size(ArrayBinaryTree *abt) {
    return abt->size;
}

/* 獲取索引為 i 節點的值 */
int val(ArrayBinaryTree *abt, int i) {
    // 若索引越界，則返回 INT_MAX ，代表空位
    if (i < 0 || i >= size(abt))
        return INT_MAX;
    return abt->tree[i];
}

/* 層序走訪 */
int *levelOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    // 直接走訪陣列
    for (int i = 0; i < size(abt); i++) {
        if (val(abt, i) != INT_MAX)
            res[index++] = val(abt, i);
    }
    *returnSize = index;
    return res;
}

/* 深度優先走訪 */
void dfs(ArrayBinaryTree *abt, int i, char *order, int *res, int *index) {
    // 若為空位，則返回
    if (val(abt, i) == INT_MAX)
        return;
    // 前序走訪
    if (strcmp(order, "pre") == 0)
        res[(*index)++] = val(abt, i);
    dfs(abt, left(i), order, res, index);
    // 中序走訪
    if (strcmp(order, "in") == 0)
        res[(*index)++] = val(abt, i);
    dfs(abt, right(i), order, res, index);
    // 後序走訪
    if (strcmp(order, "post") == 0)
        res[(*index)++] = val(abt, i);
}

/* 前序走訪 */
int *preOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    dfs(abt, 0, "pre", res, &index);
    *returnSize = index;
    return res;
}

/* 中序走訪 */
int *inOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    dfs(abt, 0, "in", res, &index);
    *returnSize = index;
    return res;
}

/* 後序走訪 */
int *postOrder(ArrayBinaryTree *abt, int *returnSize) {
    int *res = (int *)malloc(sizeof(int) * size(abt));
    int index = 0;
    dfs(abt, 0, "post", res, &index);
    *returnSize = index;
    return res;
}
```

tab: C++
```cpp
/* 陣列表示下的二元樹類別 */
class ArrayBinaryTree {
  public:
    /* 建構子 */
    ArrayBinaryTree(vector<int> arr) {
        tree = arr;
    }

    /* 串列容量 */
    int size() {
        return tree.size();
    }

    /* 獲取索引為 i 節點的值 */
    int val(int i) {
        // 若索引越界，則返回 INT_MAX ，代表空位
        if (i < 0 || i >= size())
            return INT_MAX;
        return tree[i];
    }

    /* 獲取索引為 i 節點的左子節點的索引 */
    int left(int i) {
        return 2 * i + 1;
    }

    /* 獲取索引為 i 節點的右子節點的索引 */
    int right(int i) {
        return 2 * i + 2;
    }

    /* 獲取索引為 i 節點的父節點的索引 */
    int parent(int i) {
        return (i - 1) / 2;
    }

    /* 層序走訪 */
    vector<int> levelOrder() {
        vector<int> res;
        // 直接走訪陣列
        for (int i = 0; i < size(); i++) {
            if (val(i) != INT_MAX)
                res.push_back(val(i));
        }
        return res;
    }

    /* 前序走訪 */
    vector<int> preOrder() {
        vector<int> res;
        dfs(0, "pre", res);
        return res;
    }

    /* 中序走訪 */
    vector<int> inOrder() {
        vector<int> res;
        dfs(0, "in", res);
        return res;
    }

    /* 後序走訪 */
    vector<int> postOrder() {
        vector<int> res;
        dfs(0, "post", res);
        return res;
    }

  private:
    vector<int> tree;

    /* 深度優先走訪 */
    void dfs(int i, string order, vector<int> &res) {
        // 若為空位，則返回
        if (val(i) == INT_MAX)
            return;
        // 前序走訪
        if (order == "pre")
            res.push_back(val(i));
        dfs(left(i), order, res);
        // 中序走訪
        if (order == "in")
            res.push_back(val(i));
        dfs(right(i), order, res);
        // 後序走訪
        if (order == "post")
            res.push_back(val(i));
    }
};
```

tab: Python
```python
class ArrayBinaryTree:
    """陣列表示下的二元樹類別"""

    def __init__(self, arr: list[int | None]):
        """建構子"""
        self._tree = list(arr)

    def size(self):
        """串列容量"""
        return len(self._tree)

    def val(self, i: int) -> int | None:
        """獲取索引為 i 節點的值"""
        # 若索引越界，則返回 None ，代表空位
        if i < 0 or i >= self.size():
            return None
        return self._tree[i]

    def left(self, i: int) -> int | None:
        """獲取索引為 i 節點的左子節點的索引"""
        return 2 * i + 1

    def right(self, i: int) -> int | None:
        """獲取索引為 i 節點的右子節點的索引"""
        return 2 * i + 2

    def parent(self, i: int) -> int | None:
        """獲取索引為 i 節點的父節點的索引"""
        return (i - 1) // 2

    def level_order(self) -> list[int]:
        """層序走訪"""
        self.res = []
        # 直接走訪陣列
        for i in range(self.size()):
            if self.val(i) is not None:
                self.res.append(self.val(i))
        return self.res

    def dfs(self, i: int, order: str):
        """深度優先走訪"""
        if self.val(i) is None:
            return
        # 前序走訪
        if order == "pre":
            self.res.append(self.val(i))
        self.dfs(self.left(i), order)
        # 中序走訪
        if order == "in":
            self.res.append(self.val(i))
        self.dfs(self.right(i), order)
        # 後序走訪
        if order == "post":
            self.res.append(self.val(i))

    def pre_order(self) -> list[int]:
        """前序走訪"""
        self.res = []
        self.dfs(0, order="pre")
        return self.res

    def in_order(self) -> list[int]:
        """中序走訪"""
        self.res = []
        self.dfs(0, order="in")
        return self.res

    def post_order(self) -> list[int]:
        """後序走訪"""
        self.res = []
        self.dfs(0, order="post")
        return self.res
```
~~~

### Pros and Cons
#### Pros
- 陣列儲存在連續的記憶體空間中，對快取友好，訪問與走訪速度較快。
- 不需要儲存指標，比較節省空間。
- 允許隨機訪問節點。
#### Cons
- 陣列儲存需要連續記憶體空間，因此不適合儲存資料量過大的樹。
- 增刪節點需要透過陣列插入與刪除操作實現，效率較低。
- 當二元樹中存在大量 `None` 時，陣列中包含的節點資料比重較低，空間利用率較低。

## Q & A
> [!QUESTION]- 對於只有一個節點的二元樹，樹的高度和根節點的深度都是 $0$ 嗎？
> 是的，因為高度和深度通常定義為「經過的邊的數量」。

> [!QUESTION]- 二元樹中的插入與刪除一般由一套操作配合完成，這裡的「一套操作」是指什麼呢？可以理解為資源的子節點的資源釋放嗎？
> 拿二元搜尋樹來舉例，刪除節點操作要分三種情況處理，其中每種情況都需要進行多個步驟的節點操作。

> [!QUESTION]- 為什麼 DFS 走訪二元樹有前、中、後三種順序，分別有什麼用呢？
> 與順序和逆序走訪陣列類似，前序、中序、後序走訪是三種二元樹走訪方法，我們可以使用它們得到一個特定順序的走訪結果。例如在二元搜尋樹中，由於節點大小滿足 `左子節點值 < 根節點值 < 右子節點值` ，因此我們只要按照「左 → 根 → 右」的優先順序走訪樹，就可以獲得有序的節點序列。

