## Introduction
串列（List）是一個抽象的資料結構概念，它==表示元素的有序集合==，支持元素訪問、修改、新增、刪除和走訪等操作，無須使用者考慮容量限制的問題。串列可以基於鏈結串列或陣列實現。

- 鏈結串列再本質上可以看作一個串列，其支援元素增刪查改操作，並且可以靈活動態調整其大小。
- 陣列也支持元素增刪查改，但由於其長度不可變，因此只能看作一個具有長度限制的串列。

當使用陣列實現串列時，**長度不可變的性質會導致串列的實用性降低**。這是因為我們通常無法事先確定需要儲存多少資料，從而難以選擇合適的串列長度。若長度過小，則很可能無法滿足使用需求；若長度過大，則會造成記憶體空間浪費。

為解決此問題，我們可以使用動態陣列（Dynamic Array）來實現串列。它繼承了陣列的各項優點，並且可以在程式執行過程中進行動態擴容。

實際上，**許多程式語言中的標準庫提供的串列是基於動態陣列實現的**，例如 Python 中的 `list` 、Java 中的 `ArrayList` 、C++ 中的 `vector` 和 C# 中的 `List` 等。在接下來的討論中，我們將把“串列”和“動態陣列”視為等同的概念。

## Common Operations
> [!CAUTION] 由於 C 未內建動態陣列，以下常見操作不提供 C 的程式碼。
### Initializing
我們通常使用「無初始值」和「有初始值」這兩種初始化方法。
~~~tabs
tab: C++
```cpp
/* 初始化串列 */
// 需注意，C++ 中 vector 即是本文描述的 nums
// 無初始值
vector<int> nums1;
// 有初始值
vector<int> nums = { 1, 3, 2, 5, 4 };
```

tab: Python
```python
# 初始化串列
# 無初始值
nums1: list[int] = []
# 有初始值
nums: list[int] = [1, 3, 2, 5, 4]
```
~~~

### Accessing elements
串列本質上是陣列，因此可以在 $O(1)$ 時間內訪問和更新元素，效率很高。
~~~tabs
tab: C++
```cpp
/* 訪問元素 */
int num = nums[1];  // 訪問索引 1 處的元素

/* 更新元素 */
nums[1] = 0;  // 將索引 1 處的元素更新為 0
```

tab: Python
```python
# 訪問元素
num: int = nums[1]  # 訪問索引 1 處的元素

# 更新元素
nums[1] = 0    # 將索引 1 處的元素更新為 0
```
~~~

### Inserting/Removing elements
相較於陣列，串列可以自由地新增與刪除元素。在串列尾部新增元素的時間複雜度為 $O(1)$ ，但插入和刪除元素的效率仍與陣列相同，時間複雜度為 $O(n)$ 。
~~~tabs
tab: C++
```cpp
/* 清空串列 */
nums.clear();

/* 在尾部新增元素 */
nums.push_back(1);
nums.push_back(3);
nums.push_back(2);
nums.push_back(5);
nums.push_back(4);

/* 在中間插入元素 */
nums.insert(nums.begin() + 3, 6);  // 在索引 3 處插入數字 6

/* 刪除元素 */
nums.erase(nums.begin() + 3);      // 刪除索引 3 處的元素
```

tab: Python
```python
# 清空串列
nums.clear()

# 在尾部新增元素
nums.append(1)
nums.append(3)
nums.append(2)
nums.append(5)
nums.append(4)

# 在中間插入元素
nums.insert(3, 6)  # 在索引 3 處插入數字 6

# 刪除元素
nums.pop(3)        # 刪除索引 3 處的元素
```
~~~

### Iterating
與陣列一樣，串列可以根據索引走訪，也可以直接走訪各元素。
~~~tabs
tab: C++
```cpp
/* 透過索引走訪串列 */
int count = 0;
for (int i = 0; i < nums.size(); i++) {
    count += nums[i];
}

/* 直接走訪串列元素 */
count = 0;
for (int num : nums) {
    count += num;
}
```

tab: Python
```python
# 透過索引走訪串列
count = 0
for i in range(len(nums)):
    count += nums[i]

# 直接走訪串列元素
for num in nums:
    count += num
```
~~~

### Concatenating lists
給定一個新串列 `nums1` ，我們可以將其拼接到原串列的頭部或尾部。
~~~tabs
tab: C++
```cpp
/* 拼接兩個串列 */
vector<int> nums1 = { 6, 8, 7, 10, 9 };
// 將串列 nums1 拼接到 nums 之後
nums.insert(nums.end(), nums1.begin(), nums1.end());
```

tab: Python
```python
# 拼接兩個串列
nums1: list[int] = [6, 8, 7, 10, 9]
nums += nums1  # 將串列 nums1 拼接到 nums 之後
```
~~~

### Sorting
就是將未整理的串列以升冪法排序。
排序完之後，就可以使用常見的[[Search#Binary Search|Binary Search]]、[[Two Pointers]]等演算法開始處理任務了。
~~~tabs
tab: C++
```cpp
/* 排序串列 */
sort(nums.begin(), nums.end());  // 排序後，串列元素從小到大排列
```

tab: Python
```python
# 排序串列
nums.sort()  # 排序後，串列元素從小到大排列
```
~~~

## Implemetation
許多程式語言內建了串列，例如 Java、C++、Python 等。它們的實現比較複雜，各個參數的設定也非常考究，例如初始容量、擴容倍數等。

> [!NOTE] 小試身手
> 為了加深對串列工作原理的理解，請嘗試實現一個簡易版串列。
> 設計時，除了基本操作之外，應該包括至少以下三個重點設計：
> - **初始容量**：選取一個合理的陣列初始容量。
> - **數量記錄**：宣告一個變數 `size` ，用於記錄串列當前元素數量，並隨著元素插入和刪除實時更新。根據此變數，我們可以定位串列尾部，以及判斷是否需要擴容。
> - **擴容機制**：若插入元素時串列容量已滿，則需要進行擴容。先根據擴容倍數建立一個更大的陣列，再將當前陣列的所有元素依次移動至新陣列。

#### Code Playground
```c
// C playground
// Last edited date:

```

```cpp
// C++ playground
// Last edited date:

```

```python
# Python playground
# Last edited date:
class MyList:
	def __init__(self):
		pass

	def size(self) -> int:
		pass

	def query(self, index: int):
		pass

	def insert(self, element, index: int):
		pass

	def remove(self, element, index: int):
		pass

	def update(self, element, index: int):
		pass

	def capacity(self) -> int:
		pass

	def expand_list(self):
		pass

	def to_array(self) -> list[]:
		pass
```

[點我查看參考答案](https://www.hello-algo.com/zh-hant/chapter_array_and_linkedlist/list/#432)

## Q & A
> [!QUESTION]- 在串列末尾新增元素是否時時刻刻都為 $O(1)$ ？
> 如果新增元素時超出串列長度，則需要先擴容串列再新增。系統會申請一塊新的記憶體，並將原串列的所有元素搬運過去，這時候時間複雜度就會是 $O(n)$ 。

> [!QUESTION]- 「串列的出現極大地提高了陣列的實用性，但可能導致部分記憶體空間浪費」，這裡的空間浪費是指額外增加的變數如容量、長度、擴容倍數所佔的記憶體嗎？
> 這裡的空間浪費主要有兩方面含義：
> - 串列都會設定一個初始長度，我們不一定需要用這麼多。
> - 為了防止頻繁擴容，擴容一般會乘以一個係數，比如 $×1.5$ 。這樣一來，也會出現很多空位，我們通常不能完全填滿它們。

> [!QUESTION]- C++ STL 裡面的 `std::list` 已經實現了雙向鏈結串列，但好像一些演算法書上不怎麼直接使用它，是不是因為有什麼侷限性呢？
> 一方面，我們往往更青睞使用陣列實現演算法，而只在必要時才使用鏈結串列，主要有兩個原因：
> - 空間開銷：由於每個元素需要兩個額外的指標（一個用於前一個元素，一個用於後一個元素），所以 `std::list` 通常比 `std::vector` 更佔用空間。
> - 快取不友好：由於資料不是連續存放的，因此 `std::list` 對快取的利用率較低。一般情況下，`std::vector` 的效能會更好。

> [!QUESTION]- 操作 `res = [[0]] * n` 生成了一個二維串列，其中每一個 `[0]` 都是獨立的嗎？
> 不是。此二維串列中，所有的 `[0]` 實際上是同一個物件的引用。如果我們修改其中一個元素，會發現所有的對應元素都會隨之改變。
> 如果希望二維串列中的每個 `[0]` 都是獨立的，可以使用 `res = [[0] for _ in range(n)]` 來實現。這種方式的原理是初始化了 n 個獨立的 `[0]` 串列物件。

> [!QUESTION]- 操作 `res = [0] * n` 生成了一個串列，其中每一個整數 0 都是獨立的嗎？
> 在該串列中，所有整數 0 都是同一個物件的引用。這是因為 Python 對小整數採用了快取池機制，以便最大化物件複用，從而提升效能。
> 雖然它們指向同一個物件，但我們仍然可以獨立修改串列中的每個元素，這是因為 Python 的整數是「不可變物件」。當我們修改某個元素時，實際上是切換為另一個物件的引用，而不是改變原有物件本身。
> 然而，當串列元素是「可變物件」時（例如串列、字典或類別例項等），修改某個元素會直接改變該物件本身，所有引用該物件的元素都會產生相同變化。
