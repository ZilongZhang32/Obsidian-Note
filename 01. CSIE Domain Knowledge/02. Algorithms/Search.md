# Binary Search
## Introduction
二分搜尋（Binary Search）是一種基於[[Divide and Conquer|分治策略]]的高效搜尋演算法。它利用資料的有序性，每輪縮小一半搜尋範圍，直至找到目標元素或搜尋區間為空為止。二分搜尋最常被用於以下問題類型：

> 給定一個長度為 $n$ 的陣列 `nums` ，元素按從小到大的順序排列且不重複。請查詢並返回元素 `target` 在該陣列中的索引。若陣列不包含該元素，則返回 $−1$ 。

我們先初始化指標 $i=0$ 和 $j=n−1$ ，分別指向陣列首元素和尾元素，代表搜尋區間 $[0,n−1]$ 。請注意，中括號表示閉區間，其包含邊界值本身。接下來，迴圈執行以下兩步：
1. 計算中點索引 $m=⌊(i+j)/2⌋$ ，其中 $⌊$  $⌋$ 表示向下取整操作。
2. 判斷 `nums[m]` 和 `target` 的大小關係，分為以下三種情況。
	1. 當 `nums[m] < target` 時
	    說明 `target` 在區間 $[m+1,j]$ 中，因此執行 $i=m+1$ 。
    2. 當 `nums[m] > target` 時
	    說明 `target` 在區間 $[i,m−1]$ 中，因此執行 $j=m−1$ 。
    3. 當 `nums[m] = target` 時
	    說明找到 `target` ，因此返回索引 $m$ 。

若陣列不包含目標元素，搜尋區間最終會縮小為空。此時返回 $−1$。

~~~~tabs

tab: C
```c
/* 二分搜尋（雙閉區間） */
int binarySearch(int *nums, int len, int target) {
    // 初始化雙閉區間 [0, n-1] ，即 i, j 分別指向陣列首元素、尾元素
    int i = 0, j = len - 1;
    // 迴圈，當搜尋區間為空時跳出（當 i > j 時為空）
    while (i <= j) {
        int m = i + (j - i) / 2; // 計算中點索引 m
        if (nums[m] < target)    //  target 在區間 [m+1, j] 中
            i = m + 1;
        else if (nums[m] > target) //  target 在區間 [i, m-1] 中
            j = m - 1;
        else // 找到目標元素，返回其索引
            return m;
    }
    // 未找到目標元素，返回 -1
    return -1;
}
```
tab: C++
```cpp
/* 二分搜尋（雙閉區間） */
int binarySearch(vector<int> &nums, int target) {
    // 初始化雙閉區間 [0, n-1] ，即 i, j 分別指向陣列首元素、尾元素
    int i = 0, j = nums.size() - 1;
    // 迴圈，當搜尋區間為空時跳出（當 i > j 時為空）
    while (i <= j) {
        int m = i + (j - i) / 2; // 計算中點索引 m
        if (nums[m] < target)    // 此情況說明 target 在區間 [m+1, j] 中
            i = m + 1;
        else if (nums[m] > target) // 此情況說明 target 在區間 [i, m-1] 中
            j = m - 1;
        else // 找到目標元素，返回其索引
            return m;
    }
    // 未找到目標元素，返回 -1
    return -1;
}
```
tab: Python
```python
def binary_search(nums: list[int], target: int) -> int:
    """二分搜尋（雙閉區間）"""
    # 初始化雙閉區間 [0, n-1] ，即 i, j 分別指向陣列首元素、尾元素
    i, j = 0, len(nums) - 1
    # 迴圈，當搜尋區間為空時跳出（當 i > j 時為空）
    while i <= j:
        # 理論上 Python 的數字可以無限大（取決於記憶體大小），無須考慮大數越界問題
        m = (i + j) // 2  # 計算中點索引 m
        if nums[m] < target:
            i = m + 1  # 此情況說明 target 在區間 [m+1, j] 中
        elif nums[m] > target:
            j = m - 1  # 此情況說明 target 在區間 [i, m-1] 中
        else:
            return m  # 找到目標元素，返回其索引
    return -1  # 未找到目標元素，返回 -1
```
~~~~

值得注意的是，由於 $i$ 和 $j$ 都是 `int` 型別，**因此 $i+j$ 可能會超出 `int` 型別的取值範圍**。為了避免大數越界，我們通常採用公式 $m=⌊i+(j−i)/2⌋$ 來計算中點。

## Complexity analysis
### Time complexity
 $O(\log ⁡n)$：在二分迴圈中，區間每輪縮小一半，因此迴圈次數為 $\log_2{⁡n}$ 。
### Space complexity
 $O(1)$：指標 $i$ 和 $j$ 使用常數大小空間。

## Interval Representation
除了上述雙閉區間外，常見的區間表示還有「左閉右開」區間，定義為 $[0,n)$ ，即左邊界包含自身，右邊界不包含自身。在該表示下，區間 $[i,j)$ 在 $i=j$ 時為空。我們可以基於該表示實現具有相同功能的二分搜尋演算法：

~~~tabs
~~~~~~tabs

tab: C
```c
/* 二分搜尋（左閉右開區間） */
int binarySearchLCRO(int *nums, int len, int target) {
    // 初始化左閉右開區間 [0, n) ，即 i, j 分別指向陣列首元素、尾元素+1
    int i = 0, j = len;
    // 迴圈，當搜尋區間為空時跳出（當 i = j 時為空）
    while (i < j) {
        int m = i + (j - i) / 2; // 計算中點索引 m
        if (nums[m] < target)    // target 在區間 [m+1, j) 中
            i = m + 1;
        else if (nums[m] > target) // target 在區間 [i, m) 中
            j = m;
        else // 找到目標元素，返回其索引
            return m;
    }
    // 未找到目標元素，返回 -1
    return -1;
}
```
tab: C++
```cpp
/* 二分搜尋（左閉右開區間） */
int binarySearchLCRO(vector<int> &nums, int target) {
    // 初始化左閉右開區間 [0, n) ，即 i, j 分別指向陣列首元素、尾元素+1
    int i = 0, j = nums.size();
    // 迴圈，當搜尋區間為空時跳出（當 i = j 時為空）
    while (i < j) {
        int m = i + (j - i) / 2; // 計算中點索引 m
        if (nums[m] < target)    // 此情況說明 target 在區間 [m+1, j) 中
            i = m + 1;
        else if (nums[m] > target) // 此情況說明 target 在區間 [i, m) 中
            j = m;
        else // 找到目標元素，返回其索引
            return m;
    }
    // 未找到目標元素，返回 -1
    return -1;
}
```
tab: Python
```python
def binary_search_lcro(nums: list[int], target: int) -> int:
    """二分搜尋（左閉右開區間）"""
    # 初始化左閉右開區間 [0, n) ，即 i, j 分別指向陣列首元素、尾元素+1
    i, j = 0, len(nums)
    # 迴圈，當搜尋區間為空時跳出（當 i = j 時為空）
    while i < j:
        m = (i + j) // 2  # 計算中點索引 m
        if nums[m] < target:
            i = m + 1  # 此情況說明 target 在區間 [m+1, j) 中
        elif nums[m] > target:
            j = m  # 此情況說明 target 在區間 [i, m) 中
        else:
            return m  # 找到目標元素，返回其索引
    return -1  # 未找到目標元素，返回 -1
```
~~~~~~
> [!NOTE] 由於「雙閉區間 $[i,j]$ 」表示中的左右邊界都被定義為閉區間，因此透過指標 $i$ 和指標 $j$ 縮小區間的操作也是對稱的。這樣更不容易出錯，**因此一般建議採用「雙閉區間」的寫法**。

## Pros and Cons
### Pros
二分搜尋在時間和空間方面都有較好的效能：
- 二分搜尋的時間效率高。在大資料量下，對數階的時間複雜度具有顯著優勢。
	例如，當資料大小 $n=2^{20}$ 時，線性查詢需要 $2^{20}=1048576$ 輪迴圈，而二分搜尋僅需 $\log_2{⁡2^{20}}=20$ 輪迴圈。
- 二分搜尋無須額外空間。
	相較於需要藉助額外空間的搜尋演算法（例如雜湊查詢），二分搜尋更加節省空間。
### Cons
二分搜尋並非適用於所有情況，主要有以下原因：
- 二分搜尋僅適用於有序資料。
	若輸入資料無序，為了使用二分搜尋而專門進行排序，得不償失。因為排序演算法的時間複雜度通常為 $O(n\log ⁡n)$ ，比線性查詢和二分搜尋都更高。對於頻繁插入元素的場景，為保持陣列有序性，需要將元素插入到特定位置，時間複雜度為 $O(n)$ ，也是非常昂貴的。
- 二分搜尋僅適用於陣列。
	二分搜尋需要跳躍式（非連續地）訪問元素，而在鏈結串列中執行跳躍式訪問的效率較低，因此不適合應用在鏈結串列或基於鏈結串列實現的資料結構。
- 小資料量下，線性查詢效能更佳。
	線上性查詢中，每輪只需 1 次判斷操作；而在二分搜尋中，需要 1 次加法、1 次除法、1 ~ 3 次判斷操作、1 次加法（減法），共 4 ~ 6 個單元操作；因此，當資料量 $n$ 較小時，線性查詢反而比二分搜尋更快。

## Binary Search Insertion
二分搜尋不僅可用於搜尋目標元素，還可用於解決許多變種問題，比如搜尋目標元素的插入位置。
### For no duplicates situation
>給定一個長度為 n 的有序陣列 `nums` 和一個元素 `target` ，陣列不存在重複元素。現將 `target` 插入陣列 `nums` 中，並保持其有序性。若陣列中已存在元素 `target` ，則插入到其左方。請返回插入後 `target` 在陣列中的索引。

```md
nums:[1,3,6,8,12,15,23,26,31,35]
         ^insert index

nums:[1,3,6#,6,8,12,15,23,26,31,35]
```

如果想複用上一節的二分搜尋程式碼，則需要回答以下兩個問題：
> [!QUESTION]- **問題一**：當陣列中包含 `target` 時，插入點的索引是否是該元素的索引？
> 題目要求將 `target` 插入到相等元素的左邊，這意味著新插入的 `target` 替換了原來 `target` 的位置。也就是說，**當陣列包含 `target` 時，插入點的索引就是該 `target` 的索引**。

> [!QUESTION]- **問題二**：當陣列中不存在 `target` 時，插入點是哪個元素的索引？
> 進一步思考二分搜尋過程：當 `nums[m] < target` 時 $i$ 移動，這意味著指標 $i$ 在向大於等於 `target` 的元素靠近。同理，指標 $j$ 始終在向小於等於 `target` 的元素靠近。

> [!CONCLUSION]- 
> 因此二分結束時一定有：$i$ 指向首個大於 `target` 的元素，$j$ 指向首個小於 `target` 的元素。**易得當陣列不包含 `target` 時，插入索引為 $i$** 。

~~~tabs
tab: C
```c
/* 二分搜尋插入點（無重複元素） */
int binarySearchInsertionSimple(int *nums, int numSize, int target) {
    int i = 0, j = numSize - 1; // 初始化雙閉區間 [0, n-1]
    while (i <= j) {
        int m = i + (j - i) / 2; // 計算中點索引 m
        if (nums[m] < target) {
            i = m + 1; // target 在區間 [m+1, j] 中
        } else if (nums[m] > target) {
            j = m - 1; // target 在區間 [i, m-1] 中
        } else {
            return m; // 找到 target ，返回插入點 m
        }
    }
    // 未找到 target ，返回插入點 i
    return i;
}
```

tab: C++
```cpp
/* 二分搜尋插入點（無重複元素） */
int binarySearchInsertionSimple(vector<int> &nums, int target) {
    int i = 0, j = nums.size() - 1; // 初始化雙閉區間 [0, n-1]
    while (i <= j) {
        int m = i + (j - i) / 2; // 計算中點索引 m
        if (nums[m] < target) {
            i = m + 1; // target 在區間 [m+1, j] 中
        } else if (nums[m] > target) {
            j = m - 1; // target 在區間 [i, m-1] 中
        } else {
            return m; // 找到 target ，返回插入點 m
        }
    }
    // 未找到 target ，返回插入點 i
    return i;
}
```

tab: Python
```python
def binary_search_insertion_simple(nums: list[int], target: int) -> int:
    """二分搜尋插入點（無重複元素）"""
    i, j = 0, len(nums) - 1  # 初始化雙閉區間 [0, n-1]
    while i <= j:
        m = (i + j) // 2  # 計算中點索引 m
        if nums[m] < target:
            i = m + 1  # target 在區間 [m+1, j] 中
        elif nums[m] > target:
            j = m - 1  # target 在區間 [i, m-1] 中
        else:
            return m  # 找到 target ，返回插入點 m
    # 未找到 target ，返回插入點 i
    return i
```
~~~

### For duplicates situation
```md
nums:[1,3,6@,6#,6$,6^,6&,10,12,15]
```

假設陣列中存在多個 `target` ，則普通二分搜尋只能返回其中一個 `target` 的索引，**而無法確定該元素的左邊和右邊還有多少 `target`**。若要將目標元素插入到最左邊，**我們需要查詢陣列中最左一個 `target` 的索引**，最直觀的方法，可以使用線性查詢。但是，線性查詢的時間複雜度為 $O(n)$，效率很低。

因此，我們在這裡使用另一個效率比較高的做法：
> [!HINT]- 提示
> 首個小於 `target` 的元素區間應該為何？

> [!INFO]- 完整流程
> 每輪先計算中點索引 $m$ ，再判斷 `target` 和 `nums[m]` 的大小關係，分為以下幾種情況。
> - 當 `nums[m] < target` 或 `nums[m] > target` 時，說明還沒有找到 `target` ，因此採用普通二分搜尋的縮小區間操作，**從而使指標 $i$ 和 $j$ 向 `target` 靠近**。
> - 當 `nums[m] == target` 時，說明小於 `target` 的元素在區間 $[i,m−1]$ 中，因此採用 $j=m−1$ 來縮小區間，**從而使指標 $j$ 向小於 `target` 的元素靠近**。
> 迴圈完成後，$i$ 指向最左邊的 `target` ，$j$ 指向首個小於 `target` 的元素，**因此索引 $i$ 就是插入點**。

## Binary Search Boundaries
### Left boundary searching
> 給定一個長度為 $n$ 的有序陣列 `nums` ，其中可能包含重複元素。請返回陣列中最左一個元素 `target` 的索引。若陣列中不包含該元素，則返回 $−1$ 。

二分搜尋插入點的方法，搜尋完成後 $i$ 指向最左一個 `target` ，**因此查詢插入點本質上是在查詢最左一個 `target` 的索引**。考慮透過查詢插入點的函式實現查詢左邊界。
但是，陣列中可能不包含 `target` ，這種情況可能導致以下兩種結果：
- 插入點的索引 $i$ 越界。
- 元素 `nums[i]` 與 `target` 不相等。

當遇到以上兩種情況時，直接返回 $−1$ 即可。
~~~tabs
tab: C
```c
/* 二分搜尋最左一個 target */
int binarySearchLeftEdge(int *nums, int numSize, int target) {
    // 等價於查詢 target 的插入點
    int i = binarySearchInsertion(nums, numSize, target);
    // 未找到 target ，返回 -1
    if (i == numSize || nums[i] != target) {
        return -1;
    }
    // 找到 target ，返回索引 i
    return i;
}
```

tab: C++
```cpp
/* 二分搜尋最左一個 target */
int binarySearchLeftEdge(vector<int> &nums, int target) {
    // 等價於查詢 target 的插入點
    int i = binarySearchInsertion(nums, target);
    // 未找到 target ，返回 -1
    if (i == nums.size() || nums[i] != target) {
        return -1;
    }
    // 找到 target ，返回索引 i
    return i;
}
```

tab: Python
```python
def binary_search_left_edge(nums: list[int], target: int) -> int:
    """二分搜尋最左一個 target"""
    # 等價於查詢 target 的插入點
    i = binary_search_insertion(nums, target)
    # 未找到 target ，返回 -1
    if i == len(nums) or nums[i] != target:
        return -1
    # 找到 target ，返回索引 i
    return i
```
~~~

### Right boundary searching
最直接的方式是修改查詢[[#Left boundary searching|左邊界]]的程式碼，替換在 `nums[m] == target` 情況下的指標收縮操作。

不過，有兩個更為巧妙的方式可以達到目的：
#### Reuse left boundary searching method
顧名思義，就是利用查詢最左元素的函式來查詢最右元素。
這裡就不附上程式碼，請參考以下提示思考如何編寫：
> [!HINT]
> ```md
> nums:  [ 1, 3, 6, 6, 6, 6, 6,10,12,13]
> index: [  , j, i,  ,  ,  , j, i,  ,  ]
> 
> return j
> ``` 

#### Element search transformation
我們知道，當陣列不包含 `target` 時，最終 $i$ 和 $j$ 會分別指向首個大於、小於 `target` 的元素。因此，我們可以構造一個陣列中不存在的元素，用於查詢左右邊界。

> [!WARNING] 
> 此法只在元素皆是整數（`type: int`）的時候適用，並且 `target` 需宣告為浮點數。
> 此時元素中若有浮點數（`type: float`）形態時，可能會與元素衝突，導致不適用。

---
# Hash Search
在演算法題中，**我們常透過將線性查詢替換為雜湊查詢來降低演算法的時間複雜度**。
這裡，我們將以 LeetCode 中最經典的例題 [[Two Sum]] 做為範例：
> 給定一個整數陣列 `nums` 和一個目標元素 `target` 。
> 請在陣列中搜索「和」為 `target` 的兩個元素，並返回它們的陣列索引。若存在多個解時，返回任意一個解即可，索引順序不拘。

最直觀的線性走訪（Linear Search）直接走訪所有可能的組合。我們使用一個兩層迴圈，在每輪中判斷兩個整數的和是否為 `target` ，若是，則返回它們的索引。對於這個題目來說，該方法為窮舉的暴力解。雖然空間效率非常高，僅有 $O(1)$，但時間效率卻是 $O(n^2)$，較為低落，在資料量大的情形下非常耗時。這是以時間換空間的做法。

然而，以現今科技技術來說，[[Memory and Cache#Computer Storage Devices|空間相對來說是非常便宜的]]。因此目前的主流做法，為下面提及的雜湊搜尋法。雜湊搜尋是一種利用[[Hash Table|雜湊表]]來進行高效查找的搜尋技術。它的核心思想是將數據映射到一個固定大小的雜湊表中，然後根據雜湊函數的結果直接找到目標數據，從而實現平均 $O(1)$ 的查找時間。
關於雜湊搜尋的實際範例，請參考[[Two Sum|這裡]]。

---
# Tree Search
請參照 [[Binary Search Tree]]。

---
# Choosing Search Method
搜尋演算法的選擇還取決於資料體量、搜尋效能要求、資料查詢與更新頻率等。
下表比較了常見的搜尋方法：

|       | Linear search |  Binary search  |   Tree search   | Hash search |
| ----- | :-----------: | :-------------: | :-------------: | :---------: |
| 查詢    |    $O(n)$     |   $O(\log n)$   |   $O(\log n)$   |   $O(1)$    |
| 插入    |    $O(1)$     |     $O(n)$      |   $O(\log n)$   |   $O(1)$    |
| 刪除    |    $O(n)$     |     $O(n)$      |   $O(\log n)$   |   $O(1)$    |
| 額外空間  |    $O(1)$     |     $O(1)$      |     $O(n)$      |   $O(n)$    |
| 資料預處理 |       -       | 排序$O(n \log n)$ | 建樹$O(n \log n)$ |  建表$O(n)$   |
| 資料有序  |       N       |        Y        |        Y        |      N      |

## Linear Search
- 通用性較好，無須任何資料預處理操作。假如我們僅需查詢一次資料，那麼其他三種方法的資料預處理的時間比線性搜尋的時間還要更長。
- 適用於體量較小的資料，此情況下時間複雜度對效率影響較小。
- 適用於資料更新頻率較高的場景，因為該方法不需要對資料進行任何額外維護。

## Binary Search
- 適用於大資料量的情況，效率表現穩定，最差時間複雜度為 $O(\log ⁡n)$ 。
- 資料量不能過大，因為儲存陣列需要連續的記憶體空間。
- 不適用於高頻增刪資料的場景，因為維護有序陣列的開銷較大。

## Hash Search
- 適合對查詢效能要求很高的場景，平均時間複雜度為 $O(1)$ 。
- 不適合需要有序資料或範圍查詢的場景，因為雜湊表無法維護資料的有序性。
- 對雜湊函式和雜湊衝突處理策略的依賴性較高，具有較大的效能劣化風險。
- 不適合資料量過大的情況，因為雜湊表需要額外空間來最大程度地減少衝突，從而提供良好的查詢效能。

## Tree Search
- 適用於海量資料，因為樹節點在記憶體中是分散儲存的。
- 適合需要維護有序資料或範圍查詢的場景。
- 在持續增刪節點的過程中，二元搜尋樹可能產生傾斜，時間複雜度劣化至 $O(n)$ 。
- 若使用 [[AVL Tree|AVL 樹]]或[[Red-Black Tree|紅黑樹]]，則各項操作可在 $O(\log ⁡n)$ 效率下穩定執行，但維護樹平衡的操作會增加額外的開銷。