## Introduction
陣列（Array）是一種線性資料結構，其將相同型別的元素儲存在連續的記憶體空間中。我們將元素在陣列中的位置稱為該元素的索引（Index）。

## Common Operations
### Initializing
我們可以根據需求選用陣列的兩種初始化方式：無初始值、給定初始值。在未指定初始值的情況下，大多數程式語言會將陣列中的所有元素初始化為 $0$。
~~~tabs

tab: C
```c
/* 初始化陣列 */
int arr[5] = { 0 }; // { 0, 0, 0, 0, 0 }
int nums[5] = { 1, 3, 2, 5, 4 };
```

tab: C++
```cpp
/* 初始化陣列 */
// 儲存在堆疊上
int arr[5];
int nums[5] = { 1, 3, 2, 5, 4 };
// 儲存在堆積上（需要手動釋放空間）
int* arr1 = new int[5];
int* nums1 = new int[5] { 1, 3, 2, 5, 4 };
```

tab: Python
```python
# 初始化陣列
arr: list[int] = [0] * 5  # [ 0, 0, 0, 0, 0 ]
nums: list[int] = [1, 3, 2, 5, 4]
```
~~~

### Accessing elements
陣列元素被儲存在連續的記憶體空間中，這意味著計算陣列元素的記憶體位址非常容易。給定陣列記憶體位址（首元素記憶體位址）和某個元素的索引。在陣列中訪問元素非常高效，我們可以在 $O(1)$ 時間內隨機訪問陣列中的任意一個元素。
~~~tabs
tab: C
```c
/* 隨機訪問元素 */
int randomAccess(int *nums, int size) {
    // 在區間 [0, size) 中隨機抽取一個數字
    int randomIndex = rand() % size;
    // 獲取並返回隨機元素
    int randomNum = nums[randomIndex];
    return randomNum;
}
```

tab: C++
```cpp
/* 隨機訪問元素 */
int randomAccess(int *nums, int size) {
    // 在區間 [0, size) 中隨機抽取一個數字
    int randomIndex = rand() % size;
    // 獲取並返回隨機元素
    int randomNum = nums[randomIndex];
    return randomNum;
}
```

tab: Python
```python
def random_access(nums: list[int]) -> int:
    """隨機訪問元素"""
    # 在區間 [0, len(nums)-1] 中隨機抽取一個數字
    random_index = random.randint(0, len(nums) - 1)
    # 獲取並返回隨機元素
    random_num = nums[random_index]
    return random_num
```
~~~
> [!note]- 為什麼陣列的元素索引是從 $0$ 開始？
> 從位址計算公式的角度看，**索引本質上是記憶體位址的偏移量**。首個元素的位址偏移量是 $0$ ，因此它的索引為 $0$ 是合理的。
> 
> | Array |  1  |  3  |  2  |  5  |  4  |
> | :---: | :-: | :-: | :-: | :-: | :-: |
> | Element Index |  `0`  |  `1`  |  `2`  |  `3`  |  `4`  |
> | Memory Address | `000`  | `004`  | `008`  | `012`  | `016`  |
> Element length = $4$
> 補充：元素記憶體位置 ＝ 陣列記憶體位置 ＋ 元素長度 ✕ 元素索引。
> 得到該元素記憶體位址後，我們便可直接訪問該元素。

### Inserting elements
陣列元素在記憶體中是**緊緊相鄰的**，它們之間沒有空間再存放任何資料。

如果想在陣列中間插入一個元素，則需要將該元素之後的所有元素都向後移動一位，之後再把元素賦值給該索引。
~~~tabs
tab: C
```c
/* 在陣列的索引 index 處插入元素 num */
void insert(int *nums, int size, int num, int index) {
    // 把索引 index 以及之後的所有元素向後移動一位
    for (int i = size - 1; i > index; i--) {
        nums[i] = nums[i - 1];
    }
    // 將 num 賦給 index 處的元素
    nums[index] = num;
}
```

tab: C++
```cpp
/* 在陣列的索引 index 處插入元素 num */
void insert(int *nums, int size, int num, int index) {
    // 把索引 index 以及之後的所有元素向後移動一位
    for (int i = size - 1; i > index; i--) {
        nums[i] = nums[i - 1];
    }
    // 將 num 賦給 index 處的元素
    nums[index] = num;
}
```

tab: Python
```python
def insert(nums: list[int], num: int, index: int):
    """在陣列的索引 index 處插入元素 num"""
    # 把索引 index 以及之後的所有元素向後移動一位
    for i in range(len(nums) - 1, index, -1):
        nums[i] = nums[i - 1]
    # 將 num 賦給 index 處的元素
    nums[index] = num
```
~~~

> [!NOTE] 容易忽略的細節 
> 由於陣列的長度是固定的，因此插入一個元素必定會**導致陣列尾部元素丟失**。我們將這個問題的解決方案留在[[List]]中討論。

### Removing elements
若想刪除索引 $i$ 處的元素，則需要把索引 $i$ 之後的元素都向前移動一位。
刪除元素完成後，原先末尾的元素變得“無意義”了，所以我們無須特意去修改它。
~~~~tabs

tab: C
```c
/* 刪除索引 index 處的元素 */
// 注意：stdio.h 佔用了 remove 關鍵詞
void removeItem(int *nums, int size, int index) {
    // 把索引 index 之後的所有元素向前移動一位
    for (int i = index; i < size - 1; i++) {
        nums[i] = nums[i + 1];
    }
}
```
tab: C++
```cpp
/* 刪除索引 index 處的元素 */
void remove(int *nums, int size, int index) {
    // 把索引 index 之後的所有元素向前移動一位
    for (int i = index; i < size - 1; i++) {
        nums[i] = nums[i + 1];
    }
}
```
tab: Python
```python
def remove(nums: list[int], index: int):
    """刪除索引 index 處的元素"""
    # 把索引 index 之後的所有元素向前移動一位
    for i in range(index, len(nums) - 1):
        nums[i] = nums[i + 1]
```
~~~~

> [!NOTE] 陣列的插入與刪除操作有以下缺點
> 1. **時間複雜度高**：陣列的插入和刪除的平均時間複雜度均為 $O(n)$ ，其中 $n$ 為陣列長度。
> 2.  **丟失元素**：由於陣列的長度不可變，因此在插入元素後，超出陣列長度範圍的元素會丟失。
> 3. **記憶體浪費**：我們可以初始化一個比較長的陣列，只用前面一部分，這樣在插入資料時，丟失的末尾元素都是無意義的，但這樣做會造成部分記憶體空間浪費。

### Traversing
在大多數程式語言中，我們既可以透過索引走訪陣列，也可以直接走訪獲取陣列中的每個元素。
~~~tabs
tab: C
```c
/* 走訪陣列 */
void traverse(int *nums, int size) {
    int count = 0;
    // 透過索引走訪陣列
    for (int i = 0; i < size; i++) {
        count += nums[i];
    }
}
```

tab: C++
```cpp
/* 走訪陣列 */
void traverse(int *nums, int size) {
    int count = 0;
    // 透過索引走訪陣列
    for (int i = 0; i < size; i++) {
        count += nums[i];
    }
    // 直接走訪陣列元素
    // 注：C沒有此種(foreach)語法
    for (int i : nums) {
	    count += i;
    }
}
```

tab: Python
```python
def traverse(nums: list[int]):
    """走訪陣列"""
    count = 0
    # 透過索引走訪陣列
    for i in range(len(nums)):
        count += nums[i]
    # 直接走訪陣列元素
    for num in nums:
        count += num
    # 同時走訪資料索引和元素
    for i, num in enumerate(nums):
        count += nums[i]
        count += num
```
~~~

### Finding elements
在陣列中查詢指定元素需要走訪陣列，每輪判斷元素值是否匹配，若匹配則輸出對應索引。
因為陣列是線性資料結構，所以上述查詢操作被稱為「線性查詢」。
~~~~tabs

tab: C
```c
/* 在陣列中查詢指定元素 */
int find(int *nums, int size, int target) {
    for (int i = 0; i < size; i++) {
        if (nums[i] == target)
            return i;
    }
    return -1;
}
```
tab: C++
```cpp
/* 在陣列中查詢指定元素 */
int find(int *nums, int size, int target) {
    for (int i = 0; i < size; i++) {
        if (nums[i] == target)
            return i;
    }
    return -1;
}
```
tab: Python
```python
def find(nums: list[int], target: int) -> int:
    """在陣列中查詢指定元素"""
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1
```
~~~~

### Expanding
在複雜的系統環境中，程式難以保證陣列之後的記憶體空間是可用的，從而無法安全地擴展陣列容量。因此在大多數程式語言中，**陣列的長度是不可變的**。

如果我們希望擴容陣列，則需重新建立一個更大的陣列，然後把原陣列元素依次複製到新陣列。這是一個 $O(n)$ 的操作，在陣列很大的情況下非常耗時。
~~~tabs
~~~~~~tabs

tab: C
```c
/* 擴展陣列長度 */
int *extend(int *nums, int size, int enlarge) {
    // 初始化一個擴展長度後的陣列
    int *res = (int *)malloc(sizeof(int) * (size + enlarge));
    // 將原陣列中的所有元素複製到新陣列
    for (int i = 0; i < size; i++) {
        res[i] = nums[i];
    }
    // 初始化擴展後的空間
    for (int i = size; i < size + enlarge; i++) {
        res[i] = 0;
    }
    // 返回擴展後的新陣列
    return res;
}
```
tab: C++
```cpp
/* 擴展陣列長度 */
int *extend(int *nums, int size, int enlarge) {
    // 初始化一個擴展長度後的陣列
    int *res = new int[size + enlarge];
    // 將原陣列中的所有元素複製到新陣列
    for (int i = 0; i < size; i++) {
        res[i] = nums[i];
    }
    // 釋放記憶體
    delete[] nums;
    // 返回擴展後的新陣列
    return res;
}
```
tab: Python
```python
def extend(nums: list[int], enlarge: int) -> list[int]:
    """擴展陣列長度"""
    # 初始化一個擴展長度後的陣列
    res = [0] * (len(nums) + enlarge)
    # 將原陣列中的所有元素複製到新陣列
    for i in range(len(nums)):
        res[i] = nums[i]
    # 返回擴展後的新陣列
    return res
```
~~~~~~


## Pros and Cons
### Pros
1. **空間效率高**
	陣列為資料分配了連續的記憶體塊，無須額外的結構開銷。
2. **支持隨機訪問**
	陣列允許在 O(1) 時間內訪問任何元素。
3.  **快取區域性**
	當訪問陣列元素時，計算機不僅會載入它，還會快取其周圍的其他資料，從而藉助高速快取來提升後續操作的執行速度。
### Cons
1. **插入與刪除效率低**
	當陣列中元素較多時，插入與刪除操作需要移動大量的元素。
2. **長度不可變**
	陣列在初始化後長度就固定了，擴容陣列需要將所有資料複製到新陣列，開銷很大。
3. **空間浪費**
	如果陣列分配的大小超過實際所需，那麼多餘的空間就被浪費了。

## Typical Applications
1. **隨機訪問**
	 如果我們想隨機抽取一些樣本，那麼可以用陣列儲存，並生成一個隨機序列，根據索引實現隨機抽樣。
2. **[[Sorting|排序]]和[[Search|搜尋]]**
	陣列是排序和搜尋演算法最常用的資料結構。快速排序、合併排序、二分搜尋等都主要在陣列上進行。
3. **查詢表**
	當需要快速查詢一個元素或其對應關係時，可以使用陣列作為查詢表。假如我們想實現字元到 ASCII 碼的對映，則可以將字元的 ASCII 碼值作為索引，對應的元素存放在陣列中的對應位置。
4. **[[Machine Learning|機器學習]]**
	神經網路中大量使用了向量、矩陣、張量之間的線性代數運算，這些資料都是以陣列的形式構建的。陣列是神經網路程式設計中最常使用的資料結構。
5. ==**資料結構實現**==
	==陣列可以用於實現[[Stack]]、[[Queue]]、[[Hash Table]]、[[Heap]]、[[Graph]]等資料結構。例如，圖的Adjacency Matrix（鄰接矩陣）表示實際上是一個二維陣列。==

## Q & A
> [!QUESTION]- 陣列儲存在堆疊上和儲存在堆積上，對時間效率和空間效率是否有影響？
> 儲存在堆疊上和堆積上的陣列都被儲存在連續記憶體空間內，資料操作效率基本一致。然而，堆疊和堆積具有各自的特點，從而導致以下不同點。
> 
> 1. 分配和釋放效率
> 	堆疊是一塊較小的記憶體，分配由編譯器自動完成；而堆積記憶體相對更大，可以在程式碼中動態分配，更容易碎片化。因此，堆積上的分配和釋放操作通常比堆疊上的慢。
> 2. 大小限制
> 	堆疊記憶體相對較小，堆積的大小一般受限於可用記憶體。因此堆積更加適合儲存大型陣列。
> 3. 靈活性
> 	堆疊上的陣列的大小需要在編譯時確定，而堆積上的陣列的大小可以在執行時動態確定。

> [!QUESTION]- 為什麼陣列要求相同型別的元素，而在鏈結串列中卻沒有強調相同型別呢？
> 鏈結串列由節點組成，節點之間透過引用（指標）連線，各個節點可以儲存不同型別的資料，例如 `int`、`double`、`string`、`object` 等。
> 相對地，陣列元素則必須是相同型別的，這樣才能透過計算偏移量來獲取對應元素位置。例如，陣列同時包含 `int` 和 `long` 兩種型別，單個元素分別佔用 4 位元組和 8 位元組 ，此時就不能用以下公式計算偏移量了，因為陣列中包含了兩種「元素長度」。
> 元素記憶體位置 ＝ 陣列記憶體位置 （首元素記憶體位址）＋ 元素長度 ✕ 元素索引。

