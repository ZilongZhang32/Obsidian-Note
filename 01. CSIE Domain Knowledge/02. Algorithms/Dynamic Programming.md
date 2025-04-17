## Introduction
動態規劃（Dynamic Programming）是一個重要的演算法範式，它將一個問題分解為一系列更小的子問題，並透過儲存子問題的解來避免重複計算，從而大幅提升時間效率。

### A classic easy question example
我們從一個經典例題入手，先給出它的暴力回溯解法，觀察其中包含的重疊子問題，再逐步導出更高效的動態規劃解法。

> [!QUESTION] [[Climbing Stairs|爬樓梯]]
> 給定一個共有 $n$ 階的樓梯，你每步可以上 $1$ 階或者 $2$ 階，請問有多少種方案可以爬到樓頂？

本題的目標是求解方案數量，**我們可以考慮透過回溯來窮舉所有可能性**。具體來說，將爬樓梯想象為一個多輪選擇的過程：從地面出發，每輪選擇上 $1$ 階或 $2$ 階，每當到達樓梯頂部時就將方案數量加 $1$ ，當越過樓梯頂部時就將其剪枝。程式碼如下所示：

~~~tabs
tab: C
```c
/* 回溯 */
void backtrack(int *choices, int state, int n, int *res, int len) {
    // 當爬到第 n 階時，方案數量加 1
    if (state == n)
        res[0]++;
    // 走訪所有選擇
    for (int i = 0; i < len; i++) {
        int choice = choices[i];
        // 剪枝：不允許越過第 n 階
        if (state + choice > n)
            continue;
        // 嘗試：做出選擇，更新狀態
        backtrack(choices, state + choice, n, res, len);
        // 回退
    }
}

/* 爬樓梯：回溯 */
int climbingStairsBacktrack(int n) {
    int choices[2] = {1, 2}; // 可選擇向上爬 1 階或 2 階
    int state = 0;           // 從第 0 階開始爬
    int *res = (int *)malloc(sizeof(int));
    *res = 0; // 使用 res[0] 記錄方案數量
    int len = sizeof(choices) / sizeof(int);
    backtrack(choices, state, n, res, len);
    int result = *res;
    free(res);
    return result;
}
```

tab: C++
```cpp/* 回溯 */
void backtrack(vector<int> &choices, int state, int n, vector<int> &res) {
    // 當爬到第 n 階時，方案數量加 1
    if (state == n)
        res[0]++;
    // 走訪所有選擇
    for (auto &choice : choices) {
        // 剪枝：不允許越過第 n 階
        if (state + choice > n)
            continue;
        // 嘗試：做出選擇，更新狀態
        backtrack(choices, state + choice, n, res);
        // 回退
    }
}

/* 爬樓梯：回溯 */
int climbingStairsBacktrack(int n) {
    vector<int> choices = {1, 2}; // 可選擇向上爬 1 階或 2 階
    int state = 0;                // 從第 0 階開始爬
    vector<int> res = {0};        // 使用 res[0] 記錄方案數量
    backtrack(choices, state, n, res);
    return res[0];
}
```

tab: Python
```python
def backtrack(choices: list[int], state: int, n: int, res: list[int]) -> int:
    """回溯"""
    # 當爬到第 n 階時，方案數量加 1
    if state == n:
        res[0] += 1
    # 走訪所有選擇
    for choice in choices:
        # 剪枝：不允許越過第 n 階
        if state + choice > n:
            continue
        # 嘗試：做出選擇，更新狀態
        backtrack(choices, state + choice, n, res)
        # 回退

def climbing_stairs_backtrack(n: int) -> int:
    """爬樓梯：回溯"""
    choices = [1, 2]  # 可選擇向上爬 1 階或 2 階
    state = 0  # 從第 0 階開始爬
    res = [0]  # 使用 res[0] 記錄方案數量
    backtrack(choices, state, n, res)
    return res[0]
```
~~~
#### Memorized Search
為了提升演算法效率，**我們希望所有的重疊子問題都只被計算一次**。為此，我們宣告一個陣列 `mem` 來記錄每個子問題的解，並在搜尋過程中將重疊子問題剪枝。

1. 當首次計算 `dp[i]` 時，我們將其記錄至 `mem[i]` ，以便之後使用。
2. 當再次需要計算 `dp[i]` 時，我們便可直接從 `mem[i]` 中獲取結果，從而避免重複計算該子問題。

程式碼如下所示：
~~~tabs
tab: C
```c
/* 記憶化搜尋 */
int dfs(int i, int *mem) {
    // 已知 dp[1] 和 dp[2] ，返回之
    if (i == 1 || i == 2)
        return i;
    // 若存在記錄 dp[i] ，則直接返回之
    if (mem[i] != -1)
        return mem[i];
    // dp[i] = dp[i-1] + dp[i-2]
    int count = dfs(i - 1, mem) + dfs(i - 2, mem);
    // 記錄 dp[i]
    mem[i] = count;
    return count;
}

/* 爬樓梯：記憶化搜尋 */
int climbingStairsDFSMem(int n) {
    // mem[i] 記錄爬到第 i 階的方案總數，-1 代表無記錄
    int *mem = (int *)malloc((n + 1) * sizeof(int));
    for (int i = 0; i <= n; i++) {
        mem[i] = -1;
    }
    int result = dfs(n, mem);
    free(mem);
    return result;
}
```

tab: C++
```cpp
/* 記憶化搜尋 */
int dfs(int i, vector<int> &mem) {
    // 已知 dp[1] 和 dp[2] ，返回之
    if (i == 1 || i == 2)
        return i;
    // 若存在記錄 dp[i] ，則直接返回之
    if (mem[i] != -1)
        return mem[i];
    // dp[i] = dp[i-1] + dp[i-2]
    int count = dfs(i - 1, mem) + dfs(i - 2, mem);
    // 記錄 dp[i]
    mem[i] = count;
    return count;
}

/* 爬樓梯：記憶化搜尋 */
int climbingStairsDFSMem(int n) {
    // mem[i] 記錄爬到第 i 階的方案總數，-1 代表無記錄
    vector<int> mem(n + 1, -1);
    return dfs(n, mem);
}
```

tab: Python
```python
def dfs(i: int, mem: list[int]) -> int:
    """記憶化搜尋"""
    # 已知 dp[1] 和 dp[2] ，返回之
    if i == 1 or i == 2:
        return i
    # 若存在記錄 dp[i] ，則直接返回之
    if mem[i] != -1:
        return mem[i]
    # dp[i] = dp[i-1] + dp[i-2]
    count = dfs(i - 1, mem) + dfs(i - 2, mem)
    # 記錄 dp[i]
    mem[i] = count
    return count

def climbing_stairs_dfs_mem(n: int) -> int:
    """爬樓梯：記憶化搜尋"""
    # mem[i] 記錄爬到第 i 階的方案總數，-1 代表無記錄
    mem = [-1] * (n + 1)
    return dfs(n, mem)
```
~~~

**經過記憶化處理後，所有重疊子問題都只需計算一次，時間複雜度最佳化至** $O(n)$ ，這是一個巨大的飛躍。

#### Dynamic Programming
**記憶化搜尋是一種「從頂至底」的方法**：我們從原問題（根節點）開始，遞迴地將較大子問題分解為較小子問題，直至解已知的最小子問題（葉節點）。之後，透過回溯逐層收集子問題的解，構建出原問題的解。

與之相反，**動態規劃是一種「從底至頂」的方法**：從最小子問題的解開始，迭代地構建更大子問題的解，直至得到原問題的解。

由於動態規劃不包含回溯過程，因此只需使用迴圈迭代實現，無須使用遞迴。在以下程式碼中，我們初始化一個陣列 `dp` 來儲存子問題的解，它起到了與記憶化搜尋中陣列 `mem` 相同的記錄作用：
~~~tabs
tab: C
```c
/* 爬樓梯：動態規劃 */
int climbingStairsDP(int n) {
    if (n == 1 || n == 2)
        return n;
    // 初始化 dp 表，用於儲存子問題的解
    int *dp = (int *)malloc((n + 1) * sizeof(int));
    // 初始狀態：預設最小子問題的解
    dp[1] = 1;
    dp[2] = 2;
    // 狀態轉移：從較小子問題逐步求解較大子問題
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    int result = dp[n];
    free(dp);
    return result;
}
```

tab: C++
```cpp
/* 爬樓梯：動態規劃 */
int climbingStairsDP(int n) {
    if (n == 1 || n == 2)
        return n;
    // 初始化 dp 表，用於儲存子問題的解
    vector<int> dp(n + 1);
    // 初始狀態：預設最小子問題的解
    dp[1] = 1;
    dp[2] = 2;
    // 狀態轉移：從較小子問題逐步求解較大子問題
    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

tab: Python
```python
def climbing_stairs_dp(n: int) -> int:
    """爬樓梯：動態規劃"""
    if n == 1 or n == 2:
        return n
    # 初始化 dp 表，用於儲存子問題的解
    dp = [0] * (n + 1)
    # 初始狀態：預設最小子問題的解
    dp[1], dp[2] = 1, 2
    # 狀態轉移：從較小子問題逐步求解較大子問題
    for i in range(3, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]
```
~~~
與回溯演算法一樣，動態規劃也使用「狀態」概念來表示問題求解的特定階段，每個狀態都對應一個子問題以及相應的區域性最優解。例如，爬樓梯問題的狀態定義為當前所在樓梯階數 $i$ 。

根據以上內容，我們可以總結出動態規劃的常用術語：
- 將陣列 `dp` 稱為 dp 表，`dp[i]` 表示狀態 $i$ 對應子問題的解。
- 將最小子問題對應的狀態（第 $1$ 階和第 $2$ 階樓梯）稱為初始狀態。
- 將遞推公式 $dp[i]=dp[i−1]+dp[i−2]$ 稱為狀態轉移方程。
#####  Space optimization
**由於 `dp[i]` 只與 `dp[i−1]` 和 `dp[i−2]` 有關，因此我們無須使用一個陣列 `dp` 來儲存所有子問題的解**，而只需兩個變數滾動前進即可。程式碼如下所示：

~~~tabs
tab: C
```c
/* 爬樓梯：空間最佳化後的動態規劃 */
int climbingStairsDPComp(int n) {
    if (n == 1 || n == 2)
        return n;
    int a = 1, b = 2;
    for (int i = 3; i <= n; i++) {
        int tmp = b;
        b = a + b;
        a = tmp;
    }
    return b;
}
```

tab: C++
```cpp
/* 爬樓梯：空間最佳化後的動態規劃 */
int climbingStairsDPComp(int n) {
    if (n == 1 || n == 2)
        return n;
    int a = 1, b = 2;
    for (int i = 3; i <= n; i++) {
        int tmp = b;
        b = a + b;
        a = tmp;
    }
    return b;
}
```

tab: Python
```python
def climbing_stairs_dp_comp(n: int) -> int:
    """爬樓梯：空間最佳化後的動態規劃"""
    if n == 1 or n == 2:
        return n
    a, b = 1, 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b
```
~~~

觀察以上程式碼，由於省去了陣列 `dp` 佔用的空間，因此空間複雜度從 $O(n)$ 降至 $O(1)$ 。
在動態規劃問題中，==當前狀態往往僅與前面有限個狀態有關，這時我們可以只保留必要的狀態，透過「降維」來節省記憶體空間==。**這種空間最佳化技巧被稱為「滾動變數」或「滾動陣列」**。

---
## Characteristics of DP Problems
動態規劃常用來求解最最佳化問題，它們不僅包含重疊子問題，還具有另外兩大特性
1. 最優子結構
2. 無後效性

### Optimal substructure
我們對爬樓梯問題稍作改動：
> [!QUESTION] 爬樓梯最小代價
> 給定一個樓梯，你每步可以上 $1$ 階或者 $2$ 階，每一階樓梯上都貼有一個非負整數，表示你在該臺階所需要付出的代價。給定一個非負整數陣列 `cost` ，其中 `cost[i]` 表示在第 $i$ 個臺階需要付出的代價，`cost[0]` 為地面（起始點）。
> 請計算最少需要付出多少代價才能到達頂部？

設 $dp[i]$ 為爬到第 $i$ 階累計付出的代價，由於第 $i$ 階只可能從 $i−1$ 階或 $i−2$ 階走來，因此 $dp[i]$ 只可能等於 $dp[i−1]+cost[i]$ 或 $dp[i−2]+cost[i]$ 。為了儘可能減少代價，我們應該選擇兩者中較小的那一個：
$$ dp[i]= \min (dp[i-1], dp[i-2]]) + cost[i] $$
這便可以引出最優子結構的含義：**原問題的最優解是從子問題的最優解構建得來的。**

那麼，爬樓梯題目有沒有最優子結構呢？它的目標是求解方案數量，看似是一個計數問題，但如果換一種問法：「求解最大方案數量」。我們意外地發現，**雖然題目修改前後是等價的，但最優子結構浮現出來了**：第 $n$ 階最大方案數量等於第 $n−1$ 階和第 $n−2$ 階最大方案數量之和。所以說，最優子結構的解釋方式比較靈活，且在不同問題中會有不同的含義。
~~~tabs
tab: C
```c
/* 爬樓梯最小代價：空間最佳化後的動態規劃 */
int minCostClimbingStairsDPComp(int cost[], int costSize) {
    int n = costSize - 1;
    if (n == 1 || n == 2)
        return cost[n];
    int a = cost[1], b = cost[2];
    for (int i = 3; i <= n; i++) {
        int tmp = b;
        b = myMin(a, tmp) + cost[i];
        a = tmp;
    }
    return b;
}
```

tab: C++
```cpp
/* 爬樓梯最小代價：空間最佳化後的動態規劃 */
int minCostClimbingStairsDPComp(vector<int> &cost) {
    int n = cost.size() - 1;
    if (n == 1 || n == 2)
        return cost[n];
    int a = cost[1], b = cost[2];
    for (int i = 3; i <= n; i++) {
        int tmp = b;
        b = min(a, tmp) + cost[i];
        a = tmp;
    }
    return b;
}
```

tab: Python
```python
def min_cost_climbing_stairs_dp_comp(cost: list[int]) -> int:
    """爬樓梯最小代價：空間最佳化後的動態規劃"""
    n = len(cost) - 1
    if n == 1 or n == 2:
        return cost[n]
    a, b = cost[1], cost[2]
    for i in range(3, n + 1):
        a, b = b, min(a, b) + cost[i]
    return b
```
~~~

### Statelessness
無後效性是動態規劃能夠有效解決問題的重要特性之一，其定義為：**給定一個確定的狀態，它的未來發展只與當前狀態有關，而與過去經歷的所有狀態無關**。

以爬樓梯問題為例，給定狀態 $i$ ，它會發展出狀態 $i+1$ 和狀態 $i+2$ ，分別對應跳 $1$ 步和跳 $2$ 步。在做出這兩種選擇時，我們無須考慮狀態 $i$ 之前的狀態，它們對狀態 $i$ 的未來沒有影響。然而，如果我們給爬樓梯問題新增一個約束，情況就不一樣了：
> [!QUESTION] 帶約束爬樓梯
> 給定一個共有 $n$ 階的樓梯，你每步可以上 $1$ 階或者 $2$ 階，**但不能連續兩輪跳 $1$ 階**，請問有多少種方案可以爬到樓頂？

在該問題中，如果上一輪是跳 1 階上來的，那麼下一輪就必須跳 2 階。這意味著，**下一步選擇不能由當前狀態（當前所在樓梯階數）獨立決定，還和前一個狀態（上一輪所在樓梯階數）有關**。

不難發現，此問題已不滿足無後效性，狀態轉移方程也失效了。為此，我們需要擴展狀態定義：**狀態 $[i,j]$ 表示處在第 $i$ 階並且上一輪跳了 $j$ 階**，其中 $j ∈ \{1,2\}$ 。此狀態定義有效地區分了上一輪跳了 $1$ 階還是 $2$ 階，我們可以據此判斷當前狀態是從何而來的：
- 當上一輪跳了 $1$ 階時，上上一輪只能選擇跳 2 階，即 $dp[i,1]$ 只能從 $dp[i−1,2]$ 轉移過來。
- 當上一輪跳了 $2$ 階時，上上一輪可選擇跳 $1$ 階或跳 $2$ 階，即 $dp[i,2]$ 可以從 $dp[i−2,1]$ 或 $dp[i−2,2]$ 轉移過來。

在該定義下，$dp[i,j]$ 表示狀態 $[i,j]$ 對應的方案數。此時狀態轉移方程為：
$$
\begin{cases}
dp[i, 1] &=& dp[i-1, 2]\\
dp[i, 2] &=& dp[i-2, 1] + dp[i-2, 2]
\end{cases}
$$
在上面的案例中，由於僅需多考慮前面一個狀態，因此我們仍然可以透過擴展狀態定義，使得問題重新滿足無後效性。

但是，實際上，許多複雜的組合最佳化問題（例如行商問題）都不滿足無後效性。對於這類問題，我們通常會選擇使用其他方法，例如啟發式搜尋、遺傳演算法、強化學習等，從而在有限時間內得到可用的區域性最優解。

---
## DP Problem-Solving Approach
