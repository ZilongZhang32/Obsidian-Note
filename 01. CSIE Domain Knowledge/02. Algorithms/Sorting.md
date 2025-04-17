排序演算法（Sorting algorithm）用於對一組資料按照特定順序進行排列。排序演算法有著廣泛的應用，因為有序資料通常能夠被更高效地查詢、分析和處理。排序演算法中的資料型別可以是整數、浮點數、字元或字串等。排序的判斷規則可根據需求設定：
- 數字大小
- 字元 ASCII 碼順序
	- 字典序
- 其它自定義規則

```md
1. sort numbers by its size
[7,3,2,6,1,5,0,4] 
-> [0,1,2,3,4,5,6,7]

2. sort strings by lexicographic order
['Kobe','Durant','Curry','Lebron','Harden']
-> ['Curry','Durant','Harden','Kobe','Lebron']

3. sort by custom rules
['B-','A','A+','C','A-','B+','B']
-> ['C','B-','B','B+','A-','A','A+']
```

# How to Evaluate Sortings
- 執行效率（Efficinency）
	我們期望排序演算法的時間複雜度儘量低，且總體操作數量較少（時間複雜度中的常數項變小）。對於大資料量的情況，執行效率顯得尤為重要。
- 就地性（In-place）
	顧名思義，原地排序透過在原陣列上直接操作實現排序，無須藉助額外的輔助陣列，從而節省記憶體。通常情況下，原地排序的資料搬運操作較少，執行速度也更快。
- 穩定性（Stability）
	穩定排序在完成排序後，相等元素在陣列中的相對順序不發生改變。穩定排序是多級排序場景的必要條件。假設我們有一個儲存學生資訊的表格，第 1 列和第 2 列分別是姓名和年齡。在這種情況下，非穩定排序可能導致輸入資料的有序性喪失：
```md
# 輸入資料是按照姓名排序好的
# (name, age)
  ('A', 19)
  ('B', 18)
  ('C', 21)
  ('D', 19)
  ('E', 23)

# 假設使用非穩定排序演算法按年齡排序串列，
# 結果中 ('D', 19) 和 ('A', 19) 的相對位置改變，
# 輸入資料按姓名排序的性質丟失
  ('B', 18)
  ('D', 19)
  ('A', 19)
  ('C', 21)
  ('E', 23)
```
- 自適應性（Adaptability）
	自適應排序能夠利用輸入資料已有的順序資訊來減少計算量，達到更優的時間效率。自適應排序演算法的最佳時間複雜度通常優於平均時間複雜度
- 是否基於比較
	基於比較的排序依賴比較運算子（$<$、$=$、$>$）來判斷元素的相對順序，從而排序整個陣列，理論最優時間複雜度為 $O(n\log ⁡n)$ 。而非比較排序不使用比較運算子，時間複雜度可達 $O(n)$ ，但其通用性相對較差。

最理想的排序演算法需要**執行快、原地、穩定、自適應、通用性好**。顯然，迄今為止尚未發現兼具以上所有特性的排序演算法。因此，在選擇排序演算法時，需要根據具體的資料特點和問題需求來決定。

---
# Selection Sort
## Introduction
選擇排序（Selection Sort）的工作原理非常簡單：開啟一個迴圈，每輪從未排序區間選擇最小的元素，將其放到已排序區間的末尾。

設陣列的長度為 $n$ ，選擇排序的演算法流程如下：
1. 初始狀態下，所有元素未排序，即未排序（索引）區間為 $[0,n−1]$ 。
2. 選取區間 $[0,n−1]$ 中的最小元素，將其與索引 $0$ 處的元素交換。完成後，陣列前 $1$ 個元素已排序。
3. 選取區間 $[1,n−1]$ 中的最小元素，將其與索引 $1$ 處的元素交換。完成後，陣列前 $2$ 個元素已排序。
4. 以此類推。經過 $n−1$ 輪選擇與交換後，陣列前 $n−1$ 個元素已排序。
5. 僅剩的一個元素必定是最大元素，無須排序，因此陣列排序完成。

## Code
~~~tabs
tab: C
```c
/* 選擇排序 */
void selectionSort(int nums[], int n) {
    // 外迴圈：未排序區間為 [i, n-1]
    for (int i = 0; i < n - 1; i++) {
        // 內迴圈：找到未排序區間內的最小元素
        int k = i;
        for (int j = i + 1; j < n; j++) {
            if (nums[j] < nums[k])
                k = j; // 記錄最小元素的索引
        }
        // 將該最小元素與未排序區間的首個元素交換
        int temp = nums[i];
        nums[i] = nums[k];
        nums[k] = temp;
    }
}
```

tab: C++
```cpp
/* 選擇排序 */
void selectionSort(vector<int> &nums) {
    int n = nums.size();
    // 外迴圈：未排序區間為 [i, n-1]
    for (int i = 0; i < n - 1; i++) {
        // 內迴圈：找到未排序區間內的最小元素
        int k = i;
        for (int j = i + 1; j < n; j++) {
            if (nums[j] < nums[k])
                k = j; // 記錄最小元素的索引
        }
        // 將該最小元素與未排序區間的首個元素交換
        swap(nums[i], nums[k]);
    }
}
```

tab: Python
```python
def selection_sort(nums: list[int]):
    """選擇排序"""
    n = len(nums)
    # 外迴圈：未排序區間為 [i, n-1]
    for i in range(n - 1):
        # 內迴圈：找到未排序區間內的最小元素
        k = i
        for j in range(i + 1, n):
            if nums[j] < nums[k]:
                k = j  # 記錄最小元素的索引
        # 將該最小元素與未排序區間的首個元素交換
        nums[i], nums[k] = nums[k], nums[i]
```
~~~

##  Characteristics
- **時間複雜度為 $O(n^2)$、非自適應排序**
	外迴圈共 $n−1$ 輪，第一輪的未排序區間長度為 $n$ ，最後一輪的未排序區間長度為 $2$ ，即各輪外迴圈分別包含 $n$、$n−1$、$\cdots$、$3$、$2$ 輪內迴圈，求和為 $\frac{(n−1)(n+2)}{2}$ 。
- **空間複雜度為 $O(1)$、原地排序**
	指標 $i$ 和 $j$ 使用常數大小的額外空間。
- **非穩定排序**
	如下所示，元素 `nums[i]` 有可能被交換至與其相等的元素的右邊，導致兩者的相對順序發生改變。
```md
unsorted
[4@,4#, 3,1,5,2]

after first iteration of sorting
[1, 4#, 3, 4@ ,5,2]
```

---
# Bubble Sort
## Introduction
泡沫排序（Bubble sort）透過連續地比較與交換相鄰元素實現排序。這個過程就像氣泡從底部升到頂部一樣，因此得名泡沫排序。冒泡過程可以利用元素交換操作來模擬：從陣列最左端開始向右走訪，依次比較相鄰元素大小，如果「左元素 $>$ 右元素」就交換二者。走訪完成後，最大的元素會被移動到陣列的最右端。設陣列的長度為 $n$ ，泡沫排序的步驟如下：
1. 首先，對 $n$ 個元素執行「冒泡」，**將陣列的最大元素交換至正確位置**。
2. 接下來，對剩餘 $n−1$ 個元素執行「冒泡」，**將第二大元素交換至正確位置**。
3. 以此類推，經過 $n−1$ 輪“冒泡”後，**前 $n−1$ 大的元素都被交換至正確位置**。
4. 僅剩的一個元素必定是最小元素，無須排序，因此陣列排序完成。

## Code
~~~tabs
tab: C
```c
/* 泡沫排序 */
void bubbleSort(int nums[], int size) {
    // 外迴圈：未排序區間為 [0, i]
    for (int i = size - 1; i > 0; i--) {
        // 內迴圈：將未排序區間 [0, i] 中的最大元素交換至該區間的最右端
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                int temp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = temp;
            }
        }
    }
}
```

tab: C++
```cpp
/* 泡沫排序 */
void bubbleSort(vector<int> &nums) {
    // 外迴圈：未排序區間為 [0, i]
    for (int i = nums.size() - 1; i > 0; i--) {
        // 內迴圈：將未排序區間 [0, i] 中的最大元素交換至該區間的最右端
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                // 交換 nums[j] 與 nums[j + 1]
                // 這裡使用了 std::swap() 函式
                swap(nums[j], nums[j + 1]);
            }
        }
    }
}
```

tab: Python
```python
def bubble_sort(nums: list[int]):
    """泡沫排序"""
    n = len(nums)
    # 外迴圈：未排序區間為 [0, i]
    for i in range(n - 1, 0, -1):
        # 內迴圈：將未排序區間 [0, i] 中的最大元素交換至該區間的最右端
        for j in range(i):
            if nums[j] > nums[j + 1]:
                # 交換 nums[j] 與 nums[j + 1]
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
```
~~~

## Optimization
我們發現，如果某輪「冒泡」中沒有執行任何交換操作，說明陣列已經完成排序，可直接返回結果。因此，可以增加一個標誌位 `flag` 來監測這種情況，一旦出現就立即返回。經過最佳化，泡沫排序的最差時間複雜度和平均時間複雜度仍為 $O(n^2)$ ；但當輸入陣列完全有序時，可達到最佳時間複雜度 $O(n)$ 。
~~~tabs
tab: C
```c
/* 泡沫排序（標誌最佳化）*/
void bubbleSortWithFlag(int nums[], int size) {
    // 外迴圈：未排序區間為 [0, i]
    for (int i = size - 1; i > 0; i--) {
        bool flag = false;
        // 內迴圈：將未排序區間 [0, i] 中的最大元素交換至該區間的最右端
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                int temp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = temp;
                flag = true;
            }
        }
        if (!flag)
            break;
    }
}
```

tab: C++
```cpp
/* 泡沫排序（標誌最佳化）*/
void bubbleSortWithFlag(vector<int> &nums) {
    // 外迴圈：未排序區間為 [0, i]
    for (int i = nums.size() - 1; i > 0; i--) {
        bool flag = false; // 初始化標誌位
        // 內迴圈：將未排序區間 [0, i] 中的最大元素交換至該區間的最右端
        for (int j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                // 交換 nums[j] 與 nums[j + 1]
                // 這裡使用了 std::swap() 函式
                swap(nums[j], nums[j + 1]);
                flag = true; // 記錄交換元素
            }
        }
        if (!flag)
            break; // 此輪“冒泡”未交換任何元素，直接跳出
    }
}
```

tab: Python
```python
def bubble_sort_with_flag(nums: list[int]):
    """泡沫排序（標誌最佳化）"""
    n = len(nums)
    # 外迴圈：未排序區間為 [0, i]
    for i in range(n - 1, 0, -1):
        flag = False  # 初始化標誌位
        # 內迴圈：將未排序區間 [0, i] 中的最大元素交換至該區間的最右端
        for j in range(i):
            if nums[j] > nums[j + 1]:
                # 交換 nums[j] 與 nums[j + 1]
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
                flag = True  # 記錄交換元素
        if not flag:
            break  # 此輪“冒泡”未交換任何元素，直接跳出
```
~~~

## Characteristics
- **時間複雜度為 $O(n^2)$、自適應排序**
	各輪「冒泡」走訪的陣列長度依次為 $n−1$、$n−2$、$\cdots$、$2$、$1$ ，總和為 $\frac{n(n−1)}{2}$ 。在引入 `flag` 最佳化後，最佳時間複雜度可達到 $O(n)$。
- **空間複雜度為 $O(1)$、原地排序**
	指標 $i$ 和 $j$ 使用常數大小的額外空間。
- **穩定排序**
	由於在「冒泡」中遇到相等元素不交換。

---
# Insertion Sort
## Introduction
插入排序（Insertion sort）是一種簡單的排序演算法，它的工作原理與手動整理一副牌的過程非常相似。具體來說，我們在未排序區間選擇一個基準元素，將該元素與其左側已排序區間的元素逐一比較大小，並將該元素插入到正確的位置。設基準元素為 `base` ，我們需要將從目標索引到 `base` 之間的所有元素向右移動一位，然後將 `base` 賦值給目標索引。插入排序的整體流程如下：
1. 初始狀態下，陣列的第 1 個元素已完成排序。
2. 選取陣列的第 2 個元素作為 `base` ，將其插入到正確位置後，**陣列的前 2 個元素已排序**。
3. 選取第 3 個元素作為 `base` ，將其插入到正確位置後，**陣列的前 3 個元素已排序**。
4. 以此類推，在最後一輪中，選取最後一個元素作為 `base` ，將其插入到正確位置後，**所有元素均已排序**。

## Code
~~~tabs
tab: C
```c
/* 插入排序 */
void insertionSort(int nums[], int size) {
    // 外迴圈：已排序區間為 [0, i-1]
    for (int i = 1; i < size; i++) {
        int base = nums[i], j = i - 1;
        // 內迴圈：將 base 插入到已排序區間 [0, i-1] 中的正確位置
        while (j >= 0 && nums[j] > base) {
            // 將 nums[j] 向右移動一位
            nums[j + 1] = nums[j];
            j--;
        }
        // 將 base 賦值到正確位置
        nums[j + 1] = base;
    }
}
```

tab: C++
```cpp
/* 插入排序 */
void insertionSort(vector<int> &nums) {
    // 外迴圈：已排序區間為 [0, i-1]
    for (int i = 1; i < nums.size(); i++) {
        int base = nums[i], j = i - 1;
        // 內迴圈：將 base 插入到已排序區間 [0, i-1] 中的正確位置
        while (j >= 0 && nums[j] > base) {
            nums[j + 1] = nums[j]; // 將 nums[j] 向右移動一位
            j--;
        }
        nums[j + 1] = base; // 將 base 賦值到正確位置
    }
}
```

tab: Python
```python
def insertion_sort(nums: list[int]):
    """插入排序"""
    # 外迴圈：已排序區間為 [0, i-1]
    for i in range(1, len(nums)):
        base = nums[i]
        j = i - 1
        # 內迴圈：將 base 插入到已排序區間 [0, i-1] 中的正確位置
        while j >= 0 and nums[j] > base:
            nums[j + 1] = nums[j]  # 將 nums[j] 向右移動一位
            j -= 1
        nums[j + 1] = base  # 將 base 賦值到正確位置
```
~~~

## Characteristics
- **時間複雜度為 $O(n^2)$、自適應排序**
	在最差情況下，每次插入操作分別需要迴圈 $n−1$、$n−2$、$\cdots$、$2$、$1$ 次，求和得到 $\frac{n(n−1)}{2}$ ，因此時間複雜度為 $O(n^2)$ 。在遇到有序資料時，插入操作會提前終止。當輸入陣列完全有序時，插入排序達到最佳時間複雜度 $O(n)$ 。
- **空間複雜度為 $O(1)$、原地排序**
	指標 $i$ 和 $j$ 使用常數大小的額外空間。
- **穩定排序**
	在插入操作過程中，我們會將元素插入到相等元素的右側，不會改變它們的順序。

> [!TIP] 關於插入排序的優勢
> 插入排序的時間複雜度為 $O(n^2)$ ，而我們即將學習的快速排序的時間複雜度為 $O(n\log ⁡n)$ 。儘管插入排序的時間複雜度更高，**但在資料量較小的情況下，插入排序通常更快**。
> 
> 這個結論與線性查詢和二分搜尋的適用情況的結論類似。快速排序這類 $O(n\log ⁡n)$ 的演算法屬於基於分治策略的排序演算法，往往包含更多單元計算操作。而在資料量較小時，$n^2$ 和 $n\log ⁡n$ 的數值比較接近，複雜度不佔主導地位，每輪中的單元操作數量起到決定性作用。
> 
> 實際上，許多程式語言（例如 Java）的內建排序函式採用了插入排序，大致思路為：對於長陣列，採用基於分治策略的排序演算法，例如快速排序；對於短陣列，直接使用插入排序。
> 
> 雖然泡沫排序、選擇排序和插入排序的時間複雜度都為 $O(n^2)$ ，但在實際情況中，**插入排序的使用頻率顯著高於泡沫排序和選擇排序**，主要有以下原因：
> - 泡沫排序基於元素交換實現，需要藉助一個臨時變數，共涉及 3 個單元操作；插入排序基於元素賦值實現，僅需 1 個單元操作。因此，**泡沫排序的計算開銷通常比插入排序更高**。
> - 選擇排序在任何情況下的時間複雜度都為 $O(n^2)$ 。**如果給定一組部分有序的資料，插入排序通常比選擇排序效率更高**。
> - 選擇排序不穩定，無法應用於多級排序。

---
# Quick Sort
## Introduction
快速排序（quick sort）是一種基於[[Divide and Conquer|分治策略]]的排序演算法，執行高效，應用廣泛。
快速排序的核心操作是「哨兵劃分」，其目標是：選擇陣列中的某個元素作為「基準數」，將所有小於基準數的元素移到其左側，而大於基準數的元素移到其右側。具體來說，哨兵劃分的流程如下：
1. 選取陣列最左端元素作為基準數，初始化兩個指標 `i` 和 `j` 分別指向陣列的兩端。
2. 設定一個迴圈，在每輪中使用 `i`（`j`）分別尋找第一個比基準數大（小）的元素，然後交換這兩個元素。
3. 迴圈執行步驟 `2.` ，直到 `i` 和 `j` 相遇時停止，最後將基準數交換至兩個子陣列的分界線。

哨兵劃分完成後，原陣列被劃分成三部分：左子陣列、基準數、右子陣列，且滿足「左子陣列任意元素 $≤$ 基準數 $≤$ 右子陣列任意元素」。因此，我們接下來只需對這兩個子陣列進行排序

> [!INFO] 快速排序的分治策略
> 哨兵劃分的實質是將一個較長陣列的排序問題簡化為兩個較短陣列的排序問題。

而快速排序的整體流程如下：
1. 首先，對原陣列執行一次「哨兵劃分」，得到未排序的左子陣列和右子陣列。
2. 然後，對左子陣列和右子陣列分別遞迴執行「哨兵劃分」。
3. 持續遞迴，直至子陣列長度為 1 時終止，從而完成整個陣列的排序。

## Code
~~~~tabs

tab: C
```c
/* 元素交換 */
void swap(int nums[], int i, int j) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}

/* 哨兵劃分 */
int partition(int nums[], int left, int right) {
    // 以 nums[left] 為基準數
    int i = left, j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left]) {
            j--; // 從右向左找首個小於基準數的元素
        }
        while (i < j && nums[i] <= nums[left]) {
            i++; // 從左向右找首個大於基準數的元素
        }
        // 交換這兩個元素
        swap(nums, i, j);
    }
    // 將基準數交換至兩子陣列的分界線
    swap(nums, i, left);
    // 返回基準數的索引
    return i;
}

/* 快速排序 */
void quickSort(int nums[], int left, int right) {
    // 子陣列長度為 1 時終止遞迴
    if (left >= right) {
        return;
    }
    // 哨兵劃分
    int pivot = partition(nums, left, right);
    // 遞迴左子陣列、右子陣列
    quickSort(nums, left, pivot - 1);
    quickSort(nums, pivot + 1, right);
}
```
tab: C++
```cpp
/* 哨兵劃分 */
int partition(vector<int> &nums, int left, int right) {
    // 以 nums[left] 為基準數
    int i = left, j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left])
            j--;                // 從右向左找首個小於基準數的元素
        while (i < j && nums[i] <= nums[left])
            i++;                // 從左向右找首個大於基準數的元素
        swap(nums[i], nums[j]); // 交換這兩個元素
    }
    swap(nums[i], nums[left]);  // 將基準數交換至兩子陣列的分界線
    return i;                   // 返回基準數的索引
}

/* 快速排序 */
void quickSort(vector<int> &nums, int left, int right) {
    // 子陣列長度為 1 時終止遞迴
    if (left >= right)
        return;
    // 哨兵劃分
    int pivot = partition(nums, left, right);
    // 遞迴左子陣列、右子陣列
    quickSort(nums, left, pivot - 1);
    quickSort(nums, pivot + 1, right);
}
```
tab: Python
```python
def partition(self, nums: list[int], left: int, right: int) -> int:
    """哨兵劃分"""
    # 以 nums[left] 為基準數
    i, j = left, right
    while i < j:
        while i < j and nums[j] >= nums[left]:
            j -= 1  # 從右向左找首個小於基準數的元素
        while i < j and nums[i] <= nums[left]:
            i += 1  # 從左向右找首個大於基準數的元素
        # 元素交換
        nums[i], nums[j] = nums[j], nums[i]
    # 將基準數交換至兩子陣列的分界線
    nums[i], nums[left] = nums[left], nums[i]
    return i  # 返回基準數的索引

def quick_sort(self, nums: list[int], left: int, right: int):
    """快速排序"""
    # 子陣列長度為 1 時終止遞迴
    if left >= right:
        return
    # 哨兵劃分
    pivot = self.partition(nums, left, right)
    # 遞迴左子陣列、右子陣列
    self.quick_sort(nums, left, pivot - 1)
    self.quick_sort(nums, pivot + 1, right)
```
~~~~

## Characteristics
- **時間複雜度為 $O(n\log ⁡n)$、非自適應排序**
	在平均情況下，哨兵劃分的遞迴層數為 $\log ⁡n$ ，每層中的總迴圈數為 $n$ ，總體使用 $O(n\log ⁡n)$ 時間。在最差情況下，每輪哨兵劃分操作都將長度為 $n$ 的陣列劃分為長度為 $0$ 和 $n−1$ 的兩個子陣列，此時遞迴層數達到 $n$ ，每層中的迴圈數為 $n$ ，總體使用 $O(n^2)$ 時間。
- **空間複雜度為 $O(n)$、原地排序**
	在輸入陣列完全倒序的情況下，達到最差遞迴深度 $n$ ，使用 $O(n)$ 堆疊幀空間。排序操作是在原陣列上進行的，未藉助額外陣列。
- **非穩定排序**
	在哨兵劃分的最後一步，基準數可能會被交換至相等元素的右側。

## Why Quick Sort So Fast?
從名稱上就能看出，快速排序在效率方面應該具有一定的優勢。儘管快速排序的平均時間複雜度與「[[#Merge Sort|合併排序]]」和「[[#Heap Sort|堆積排序]]」相同，但通常快速排序的效率更高，主要有以下原因：
- **出現最差情況的機率很低**
	雖然快速排序的最差時間複雜度為 $O(n^2)$ ，沒有合併排序穩定，但在絕大多數情況下，快速排序能在 $O(n\log ⁡n)$ 的時間複雜度下執行。
- **快取使用效率高**
	在執行哨兵劃分操作時，系統可將整個子陣列載入到快取，因此訪問元素的效率較高。而像「[[#Heap Sort|堆積排序]]」這類演算法需要跳躍式訪問元素，從而缺乏這一特性。
- **複雜度的常數係數小**
	在上述三種演算法中，快速排序的比較、賦值、交換等操作的總數量最少。這與「[[#Insertion Sort|插入排序]]」比「[[#Bubble Sort|泡沫排序]]」更快的原因類似。

## Optimization
### Pivot optimization
**快速排序在某些輸入下的時間效率可能降低**。舉一個極端例子，假設輸入陣列是完全倒序的，由於我們選擇最左端元素作為基準數，那麼在哨兵劃分完成後，基準數被交換至陣列最右端，導致左子陣列長度為 $n−1$、右子陣列長度為 $0$ 。如此遞迴下去，每輪哨兵劃分後都有一個子陣列的長度為 $0$ ，分治策略失效，快速排序退化為「[[#Bubble Sort|泡沫排序]]」的近似形式。

為了儘量避免這種情況發生，**我們可以最佳化哨兵劃分中的基準數的選取策略**。例如，我們可以隨機選取一個元素作為基準數。然而，如果運氣不佳，每次都選到不理想的基準數，效率仍然不盡如人意。

需要注意的是，程式語言通常生成的是「偽隨機數」。如果我們針對偽隨機數序列構建一個特定的測試樣例，那麼快速排序的效率仍然可能劣化。

為了進一步改進，我們可以在陣列中選取三個候選元素（通常為陣列的首、尾、中點元素），**並將這三個候選元素的中位數作為基準數**。這樣一來，基準數「既不太小也不太大」的機率將大幅提升。當然，我們還可以選取更多候選元素，以進一步提高演算法的穩健性。採用這種方法後，時間複雜度劣化至 $O(n^2)$ 的機率大大降低。

~~~tabs
tab: C
```c
/* 選取三個候選元素的中位數 */
int medianThree(int nums[], int left, int mid, int right) {
    int l = nums[left], m = nums[mid], r = nums[right];
    if ((l <= m && m <= r) || (r <= m && m <= l))
        return mid; // m 在 l 和 r 之間
    if ((m <= l && l <= r) || (r <= l && l <= m))
        return left; // l 在 m 和 r 之間
    return right;
}

/* 哨兵劃分（三數取中值） */
int partitionMedian(int nums[], int left, int right) {
    // 選取三個候選元素的中位數
    int med = medianThree(nums, left, (left + right) / 2, right);
    // 將中位數交換至陣列最左端
    swap(nums, left, med);
    // 以 nums[left] 為基準數
    int i = left, j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left])
            j--; // 從右向左找首個小於基準數的元素
        while (i < j && nums[i] <= nums[left])
            i++;          // 從左向右找首個大於基準數的元素
        swap(nums, i, j); // 交換這兩個元素
    }
    swap(nums, i, left); // 將基準數交換至兩子陣列的分界線
    return i;            // 返回基準數的索引
}
```

tab: C++
```cpp
/* 選取三個候選元素的中位數 */
int medianThree(vector<int> &nums, int left, int mid, int right) {
    int l = nums[left], m = nums[mid], r = nums[right];
    if ((l <= m && m <= r) || (r <= m && m <= l))
        return mid; // m 在 l 和 r 之間
    if ((m <= l && l <= r) || (r <= l && l <= m))
        return left; // l 在 m 和 r 之間
    return right;
}

/* 哨兵劃分（三數取中值） */
int partition(vector<int> &nums, int left, int right) {
    // 選取三個候選元素的中位數
    int med = medianThree(nums, left, (left + right) / 2, right);
    // 將中位數交換至陣列最左端
    swap(nums[left], nums[med]);
    // 以 nums[left] 為基準數
    int i = left, j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left])
            j--;                // 從右向左找首個小於基準數的元素
        while (i < j && nums[i] <= nums[left])
            i++;                // 從左向右找首個大於基準數的元素
        swap(nums[i], nums[j]); // 交換這兩個元素
    }
    swap(nums[i], nums[left]);  // 將基準數交換至兩子陣列的分界線
    return i;                   // 返回基準數的索引
}
```

tab: Python
```python
def median_three(self, nums: list[int], left: int, mid: int, right: int) -> int:
    """選取三個候選元素的中位數"""
    l, m, r = nums[left], nums[mid], nums[right]
    if (l <= m <= r) or (r <= m <= l):
        return mid  # m 在 l 和 r 之間
    if (m <= l <= r) or (r <= l <= m):
        return left  # l 在 m 和 r 之間
    return right

def partition(self, nums: list[int], left: int, right: int) -> int:
    """哨兵劃分（三數取中值）"""
    # 以 nums[left] 為基準數
    med = self.median_three(nums, left, (left + right) // 2, right)
    # 將中位數交換至陣列最左端
    nums[left], nums[med] = nums[med], nums[left]
    # 以 nums[left] 為基準數
    i, j = left, right
    while i < j:
        while i < j and nums[j] >= nums[left]:
            j -= 1  # 從右向左找首個小於基準數的元素
        while i < j and nums[i] <= nums[left]:
            i += 1  # 從左向右找首個大於基準數的元素
        # 元素交換
        nums[i], nums[j] = nums[j], nums[i]
    # 將基準數交換至兩子陣列的分界線
    nums[i], nums[left] = nums[left], nums[i]
    return i  # 返回基準數的索引
```
~~~

### Tail recursion optimization
**在某些輸入下，快速排序可能佔用空間較多**。以完全有序的輸入陣列為例，設遞迴中的子陣列長度為 $m$ ，每輪哨兵劃分操作都將產生長度為 $0$ 的左子陣列和長度為 $m−1$ 的右子陣列，這意味著每一層遞迴呼叫減少的問題規模非常小（只減少一個元素），遞迴樹的高度會達到 $n−1$ ，此時需要佔用 $O(n)$ 大小的堆疊幀空間。

為了防止堆疊幀空間的累積，我們可以在每輪哨兵排序完成後，比較兩個子陣列的長度，**僅對較短的子陣列進行遞迴**。由於較短子陣列的長度不會超過 $n/2$ ，因此這種方法能確保遞迴深度不超過 $\log ⁡n$ ，從而將最差空間複雜度最佳化至 $O(\log ⁡n)$。

~~~tabs
tab: C
```c
/* 快速排序（尾遞迴最佳化） */
void quickSortTailCall(int nums[], int left, int right) {
    // 子陣列長度為 1 時終止
    while (left < right) {
        // 哨兵劃分操作
        int pivot = partition(nums, left, right);
        // 對兩個子陣列中較短的那個執行快速排序
        if (pivot - left < right - pivot) {
            // 遞迴排序左子陣列
            quickSortTailCall(nums, left, pivot - 1);
            // 剩餘未排序區間為 [pivot + 1, right]
            left = pivot + 1;
        } else {
            // 遞迴排序右子陣列
            quickSortTailCall(nums, pivot + 1, right);
            // 剩餘未排序區間為 [left, pivot - 1]
            right = pivot - 1;
        }
    }
}
```

tab: C++
```cpp
/* 快速排序（尾遞迴最佳化） */
void quickSort(vector<int> &nums, int left, int right) {
    // 子陣列長度為 1 時終止
    while (left < right) {
        // 哨兵劃分操作
        int pivot = partition(nums, left, right);
        // 對兩個子陣列中較短的那個執行快速排序
        if (pivot - left < right - pivot) {
            quickSort(nums, left, pivot - 1); // 遞迴排序左子陣列
            left = pivot + 1;                 // 剩餘未排序區間為 [pivot + 1, right]
        } else {
            quickSort(nums, pivot + 1, right); // 遞迴排序右子陣列
            right = pivot - 1;                 // 剩餘未排序區間為 [left, pivot - 1]
        }
    }
}
```

tab: Python
```python
def quick_sort(self, nums: list[int], left: int, right: int):
    """快速排序（尾遞迴最佳化）"""
    # 子陣列長度為 1 時終止
    while left < right:
        # 哨兵劃分操作
        pivot = self.partition(nums, left, right)
        # 對兩個子陣列中較短的那個執行快速排序
        if pivot - left < right - pivot:
            self.quick_sort(nums, left, pivot - 1)  # 遞迴排序左子陣列
            left = pivot + 1  # 剩餘未排序區間為 [pivot + 1, right]
        else:
            self.quick_sort(nums, pivot + 1, right)  # 遞迴排序右子陣列
            right = pivot - 1  # 剩餘未排序區間為 [left, pivot - 1]
```
~~~
---
# Merge Sort
## Introduction
合併排序（Merge sort）是一種基於分治策略的排序演算法，包含「劃分」和「合併」階段：
1. **劃分階段**：透過遞迴不斷地將陣列從中點處分開，將長陣列的排序問題轉換為短陣列的排序問題。
2. **合併階段**：當子陣列長度為 $1$ 時終止劃分，開始合併，持續地將左右兩個較短的有序陣列合併為一個較長的有序陣列，直至結束。

```md
            [7,3,2,6,0,1,5,4]          |} 1. Divide phase
        [7,3,2,6]        [0,1,5,4]     |}
     [7,3]     [2,6]  [0,1]     [5,4]  |}
    [7] [3]   [2] [6][0] [1]   [5] [4] <!--trigger loop break--->
	 [3,7]     [2,6]  [0,1]     [4,5]  |} 2. Merge phase
		[2,3,6,7]        [0,1,4,5]     |}
			[0,1,2,3,4,5,6,7]          |}
```

演算法流程如上所示：
1. 「劃分階段」
	從頂至底遞迴地將陣列從中點切分為兩個子陣列。
	1.  計算陣列中點 `mid` ，遞迴劃分左子陣列（區間 `[left, mid]` ）和右子陣列（區間 `[mid + 1, right]` ）。
	2. 遞迴執行步驟 `1.` ，直至子陣列區間長度為 1 時終止。
2. 「合併階段」
	從底至頂地將左子陣列和右子陣列合併為一個有序陣列。需要注意的是，從長度為 1 的子陣列開始合併，合併階段中的每個子陣列都是有序的。

觀察發現，合併排序與二元樹後序走訪的遞迴順序是一致的。
- **後序走訪**：先遞迴左子樹，再遞迴右子樹，最後處理根節點。
- **合併排序**：先遞迴左子陣列，再遞迴右子陣列，最後處理合併。

## Code 
~~~tabs
tab: C
```c
/* 合併左子陣列和右子陣列 */
void merge(int *nums, int left, int mid, int right) {
    // 左子陣列區間為 [left, mid], 右子陣列區間為 [mid+1, right]
    // 建立一個臨時陣列 tmp ，用於存放合併後的結果
    int tmpSize = right - left + 1;
    int *tmp = (int *)malloc(tmpSize * sizeof(int));
    // 初始化左子陣列和右子陣列的起始索引
    int i = left, j = mid + 1, k = 0;
    // 當左右子陣列都還有元素時，進行比較並將較小的元素複製到臨時陣列中
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j]) {
            tmp[k++] = nums[i++];
        } else {
            tmp[k++] = nums[j++];
        }
    }
    // 將左子陣列和右子陣列的剩餘元素複製到臨時陣列中
    while (i <= mid) {
        tmp[k++] = nums[i++];
    }
    while (j <= right) {
        tmp[k++] = nums[j++];
    }
    // 將臨時陣列 tmp 中的元素複製回原陣列 nums 的對應區間
    for (k = 0; k < tmpSize; ++k) {
        nums[left + k] = tmp[k];
    }
    // 釋放記憶體
    free(tmp);
}

/* 合併排序 */
void mergeSort(int *nums, int left, int right) {
    // 終止條件
    if (left >= right)
        return; // 當子陣列長度為 1 時終止遞迴
    // 劃分階段
    int mid = left + (right - left) / 2;    // 計算中點
    mergeSort(nums, left, mid);      // 遞迴左子陣列
    mergeSort(nums, mid + 1, right); // 遞迴右子陣列
    // 合併階段
    merge(nums, left, mid, right);
}
```

tab: C++
```cpp
/* 合併左子陣列和右子陣列 */
void merge(vector<int> &nums, int left, int mid, int right) {
    // 左子陣列區間為 [left, mid], 右子陣列區間為 [mid+1, right]
    // 建立一個臨時陣列 tmp ，用於存放合併後的結果
    vector<int> tmp(right - left + 1);
    // 初始化左子陣列和右子陣列的起始索引
    int i = left, j = mid + 1, k = 0;
    // 當左右子陣列都還有元素時，進行比較並將較小的元素複製到臨時陣列中
    while (i <= mid && j <= right) {
        if (nums[i] <= nums[j])
            tmp[k++] = nums[i++];
        else
            tmp[k++] = nums[j++];
    }
    // 將左子陣列和右子陣列的剩餘元素複製到臨時陣列中
    while (i <= mid) {
        tmp[k++] = nums[i++];
    }
    while (j <= right) {
        tmp[k++] = nums[j++];
    }
    // 將臨時陣列 tmp 中的元素複製回原陣列 nums 的對應區間
    for (k = 0; k < tmp.size(); k++) {
        nums[left + k] = tmp[k];
    }
}

/* 合併排序 */
void mergeSort(vector<int> &nums, int left, int right) {
    // 終止條件
    if (left >= right)
        return; // 當子陣列長度為 1 時終止遞迴
    // 劃分階段
    int mid = left + (right - left) / 2;    // 計算中點
    mergeSort(nums, left, mid);      // 遞迴左子陣列
    mergeSort(nums, mid + 1, right); // 遞迴右子陣列
    // 合併階段
    merge(nums, left, mid, right);
}
```

tab: Python
```python
def merge(nums: list[int], left: int, mid: int, right: int):
    """合併左子陣列和右子陣列"""
    # 左子陣列區間為 [left, mid], 右子陣列區間為 [mid+1, right]
    # 建立一個臨時陣列 tmp ，用於存放合併後的結果
    tmp = [0] * (right - left + 1)
    # 初始化左子陣列和右子陣列的起始索引
    i, j, k = left, mid + 1, 0
    # 當左右子陣列都還有元素時，進行比較並將較小的元素複製到臨時陣列中
    while i <= mid and j <= right:
        if nums[i] <= nums[j]:
            tmp[k] = nums[i]
            i += 1
        else:
            tmp[k] = nums[j]
            j += 1
        k += 1
    # 將左子陣列和右子陣列的剩餘元素複製到臨時陣列中
    while i <= mid:
        tmp[k] = nums[i]
        i += 1
        k += 1
    while j <= right:
        tmp[k] = nums[j]
        j += 1
        k += 1
    # 將臨時陣列 tmp 中的元素複製回原陣列 nums 的對應區間
    for k in range(0, len(tmp)):
        nums[left + k] = tmp[k]

def merge_sort(nums: list[int], left: int, right: int):
    """合併排序"""
    # 終止條件
    if left >= right:
        return  # 當子陣列長度為 1 時終止遞迴
    # 劃分階段
    mid = (left + right) // 2 # 計算中點
    merge_sort(nums, left, mid)  # 遞迴左子陣列
    merge_sort(nums, mid + 1, right)  # 遞迴右子陣列
    # 合併階段
    merge(nums, left, mid, right)
```
~~~

## Characteristics
- **時間複雜度為 $O(n\log ⁡n)$、非自適應排序**
	劃分產生高度為 log⁡n 的遞迴樹，每層合併的總操作數量為 n ，因此總體時間複雜度為 $O(n\log ⁡n)$ 。
- **空間複雜度為 O(n)、非原地排序**
	遞迴深度為 $\log ⁡n$ ，使用 $O(\log ⁡n)$ 大小的堆疊幀空間。合併操作需要藉助輔助陣列實現，使用 $O(n)$ 大小的額外空間。
- **穩定排序**
	在合併過程中，相等元素的次序保持不變。

## Sort Linked Lists
對於鏈結串列，合併排序相較於其他排序演算法具有顯著優勢，**可以將鏈結串列排序任務的空間複雜度最佳化至 $O(1)$** 。
- **劃分階段**
	可以使用「迭代」替代「遞迴」來實現鏈結串列劃分工作，從而省去遞迴使用的堆疊幀空間。
- **合併階段**
	在鏈結串列中，節點增刪操作僅需改變引用（指標）即可實現，因此合併階段（將兩個短有序鏈結串列合併為一個長有序鏈結串列）無須建立額外鏈結串列。

具體實現細節比較複雜，有興趣的話，可以參照：
- 

---
# Heap Sort
> [!WARNING] Disclaimer
> 閱讀本節之前，請先確保已參閱以下內容：
> - [[Binary Tree]]
> - [[Heap]]

## Introduction
堆積排序（Heap Sort）是一種基於堆積資料結構實現的高效排序演算法。我們可以利用已經學過的“建堆積操作”和“元素出堆積操作”實現堆積排序。

1. 輸入陣列並建立小頂堆積，此時最小元素位於堆積頂。
2. 不斷執行出堆積操作，依次記錄出堆積元素，即可得到從小到大排序的序列。

以上方法雖然可行，但需要藉助一個額外陣列來儲存彈出的元素，比較浪費空間。在實際中，我們通常使用一種更加優雅的實現方式。

設陣列的長度為 $n$ ，堆積排序的流程如圖下：
1. 輸入陣列並建立大頂堆積。完成後，最大元素位於堆積頂。
2. 將堆積頂元素（第一個元素）與堆積底元素（最後一個元素）交換。完成交換後，堆積的長度減 $1$ ，已排序元素數量加 $1$ 。
3. 從堆積頂元素開始，從頂到底執行堆積化操作（sift down）。完成堆積化後，堆積的性質得到修復。
4. 迴圈執行第 `2.` 步和第 `3.` 步。迴圈 $n−1$ 輪後，即可完成陣列排序。

> [!INFO] 實際上，元素出堆積操作中也包含第 `2.` 步和第 `3.` 步，只是多了一個彈出元素的步驟。

## Code
在程式碼實現中，我們使用了與「堆積」章節相同的從頂至底堆積化 `sift_down()` 函式。值得注意的是，由於堆積的長度會隨著提取最大元素而減小，因此我們需要給 `sift_down()` 函式新增一個長度參數 $n$ ，用於指定堆積的當前有效長度。

~~~tabs
tab: C
```c
/* 堆積的長度為 n ，從節點 i 開始，從頂至底堆積化 */
void siftDown(int nums[], int n, int i) {
    while (1) {
        // 判斷節點 i, l, r 中值最大的節點，記為 ma
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        int ma = i;
        if (l < n && nums[l] > nums[ma])
            ma = l;
        if (r < n && nums[r] > nums[ma])
            ma = r;
        // 若節點 i 最大或索引 l, r 越界，則無須繼續堆積化，跳出
        if (ma == i) {
            break;
        }
        // 交換兩節點
        int temp = nums[i];
        nums[i] = nums[ma];
        nums[ma] = temp;
        // 迴圈向下堆積化
        i = ma;
    }
}

/* 堆積排序 */
void heapSort(int nums[], int n) {
    // 建堆積操作：堆積化除葉節點以外的其他所有節點
    for (int i = n / 2 - 1; i >= 0; --i) {
        siftDown(nums, n, i);
    }
    // 從堆積中提取最大元素，迴圈 n-1 輪
    for (int i = n - 1; i > 0; --i) {
        // 交換根節點與最右葉節點（交換首元素與尾元素）
        int tmp = nums[0];
        nums[0] = nums[i];
        nums[i] = tmp;
        // 以根節點為起點，從頂至底進行堆積化
        siftDown(nums, i, 0);
    }
}
```

tab: C++
```cpp
/* 堆積的長度為 n ，從節點 i 開始，從頂至底堆積化 */
void siftDown(vector<int> &nums, int n, int i) {
    while (true) {
        // 判斷節點 i, l, r 中值最大的節點，記為 ma
        int l = 2 * i + 1;
        int r = 2 * i + 2;
        int ma = i;
        if (l < n && nums[l] > nums[ma])
            ma = l;
        if (r < n && nums[r] > nums[ma])
            ma = r;
        // 若節點 i 最大或索引 l, r 越界，則無須繼續堆積化，跳出
        if (ma == i) {
            break;
        }
        // 交換兩節點
        swap(nums[i], nums[ma]);
        // 迴圈向下堆積化
        i = ma;
    }
}

/* 堆積排序 */
void heapSort(vector<int> &nums) {
    // 建堆積操作：堆積化除葉節點以外的其他所有節點
    for (int i = nums.size() / 2 - 1; i >= 0; --i) {
        siftDown(nums, nums.size(), i);
    }
    // 從堆積中提取最大元素，迴圈 n-1 輪
    for (int i = nums.size() - 1; i > 0; --i) {
        // 交換根節點與最右葉節點（交換首元素與尾元素）
        swap(nums[0], nums[i]);
        // 以根節點為起點，從頂至底進行堆積化
        siftDown(nums, i, 0);
    }
}
```

tab: Python
```python
def sift_down(nums: list[int], n: int, i: int):
    """堆積的長度為 n ，從節點 i 開始，從頂至底堆積化"""
    while True:
        # 判斷節點 i, l, r 中值最大的節點，記為 ma
        l = 2 * i + 1
        r = 2 * i + 2
        ma = i
        if l < n and nums[l] > nums[ma]:
            ma = l
        if r < n and nums[r] > nums[ma]:
            ma = r
        # 若節點 i 最大或索引 l, r 越界，則無須繼續堆積化，跳出
        if ma == i:
            break
        # 交換兩節點
        nums[i], nums[ma] = nums[ma], nums[i]
        # 迴圈向下堆積化
        i = ma

def heap_sort(nums: list[int]):
    """堆積排序"""
    # 建堆積操作：堆積化除葉節點以外的其他所有節點
    for i in range(len(nums) // 2 - 1, -1, -1):
        sift_down(nums, len(nums), i)
    # 從堆積中提取最大元素，迴圈 n-1 輪
    for i in range(len(nums) - 1, 0, -1):
        # 交換根節點與最右葉節點（交換首元素與尾元素）
        nums[0], nums[i] = nums[i], nums[0]
        # 以根節點為起點，從頂至底進行堆積化
        sift_down(nums, i, 0)
```
~~~

## Characteristics
- **時間複雜度為 $O(nlog⁡n)$、非自適應排序**
	建堆積操作使用 $O(n)$ 時間。從堆積中提取最大元素的時間複雜度為 $O(\log ⁡n)$ ，共迴圈 $n−1$ 輪。
- **空間複雜度為 $O(1)$、原地排序**
	幾個指標變數使用 $O(1)$ 空間。元素交換和堆積化操作都是在原陣列上進行的。
- **非穩定排序**
	在交換堆積頂元素和堆積底元素時，相等元素的相對位置可能發生變化。

---
# Bucket Sort
## Introduction
前述幾種排序演算法都屬於「基於比較的排序演算法」，它們透過比較元素間的大小來實現排序。此類排序演算法的時間複雜度無法超越 $O(n\log ⁡n)$ 。接下來，我們將探討幾種「非比較排序演算法」，它們的時間複雜度可以達到線性階。

桶排序（Bucket sort）是分治策略的一個典型應用。它透過設定一些具有大小順序的桶，每個桶對應一個數據範圍，將資料平均分配到各個桶中；然後，在每個桶內部分別執行排序；最終按照桶的順序將所有資料合併。

考慮一個長度為 n 的陣列，其元素是範圍 $[0,1)$ 內的浮點數。桶排序的流程如下：
1. 初始化 $k$ 個桶，將 $n$ 個元素分配到 $k$ 個桶中。
2. 對每個桶分別執行排序（這裡採用程式語言的內建排序函式）。
3. 按照桶從小到大的順序合併結果。

## Code
~~~tabs
tab: C
```c
/* 桶排序 */
void bucketSort(float nums[], int n) {
    int k = n / 2;                                 // 初始化 k = n/2 個桶
    int *sizes = malloc(k * sizeof(int));          // 記錄每個桶的大小
    float **buckets = malloc(k * sizeof(float *)); // 動態陣列的陣列（桶）
    // 為每個桶預分配足夠的空間
    for (int i = 0; i < k; ++i) {
        buckets[i] = (float *)malloc(n * sizeof(float));
        sizes[i] = 0;
    }
    // 1. 將陣列元素分配到各個桶中
    for (int i = 0; i < n; ++i) {
        int idx = (int)(nums[i] * k);
        buckets[idx][sizes[idx]++] = nums[i];
    }
    // 2. 對各個桶執行排序
    for (int i = 0; i < k; ++i) {
        qsort(buckets[i], sizes[i], sizeof(float), compare);
    }
    // 3. 合併排序後的桶
    int idx = 0;
    for (int i = 0; i < k; ++i) {
        for (int j = 0; j < sizes[i]; ++j) {
            nums[idx++] = buckets[i][j];
        }
        // 釋放記憶體
        free(buckets[i]);
    }
}
```

tab: C++
```cpp
/* 桶排序 */
void bucketSort(vector<float> &nums) {
    // 初始化 k = n/2 個桶，預期向每個桶分配 2 個元素
    int k = nums.size() / 2;
    vector<vector<float>> buckets(k);
    // 1. 將陣列元素分配到各個桶中
    for (float num : nums) {
        // 輸入資料範圍為 [0, 1)，使用 num * k 對映到索引範圍 [0, k-1]
        int i = num * k;
        // 將 num 新增進桶 bucket_idx
        buckets[i].push_back(num);
    }
    // 2. 對各個桶執行排序
    for (vector<float> &bucket : buckets) {
        // 使用內建排序函式，也可以替換成其他排序演算法
        sort(bucket.begin(), bucket.end());
    }
    // 3. 走訪桶合併結果
    int i = 0;
    for (vector<float> &bucket : buckets) {
        for (float num : bucket) {
            nums[i++] = num;
        }
    }
}
```

tab: Python
```python
def bucket_sort(nums: list[float]):
    """桶排序"""
    # 初始化 k = n/2 個桶，預期向每個桶分配 2 個元素
    k = len(nums) // 2
    buckets = [[] for _ in range(k)]
    # 1. 將陣列元素分配到各個桶中
    for num in nums:
        # 輸入資料範圍為 [0, 1)，使用 num * k 對映到索引範圍 [0, k-1]
        i = int(num * k)
        # 將 num 新增進桶 i
        buckets[i].append(num)
    # 2. 對各個桶執行排序
    for bucket in buckets:
        # 使用內建排序函式，也可以替換成其他排序演算法
        bucket.sort()
    # 3. 走訪桶合併結果
    i = 0
    for bucket in buckets:
        for num in bucket:
            nums[i] = num
            i += 1
```
~~~

## Characteristics
桶排序適用於處理體量很大的資料。例如，輸入資料包含 100 萬個元素，由於空間限制，系統記憶體無法一次性載入所有資料。此時，可以將資料分成 1000 個桶，然後分別對每個桶進行排序，最後將結果合併。

- **時間複雜度為 $O(n+k)$** 
	假設元素在各個桶內平均分佈，那麼每個桶內的元素數量為 $frac{n}{k}$ 。假設排序單個桶使用 $O(\frac{n}{k}\log \frac{⁡n}{k})$ 時間，則排序所有桶使用 $O(n\log \frac{⁡n}{k})$ 時間。**當桶數量 $k$ 比較大時，時間複雜度則趨向於 $O(n)$** 。合併結果時需要走訪所有桶和元素，花費 $O(n+k)$ 時間。在最差情況下，所有資料被分配到一個桶中，且排序該桶使用 $O(n^2)$ 時間。
- **空間複雜度為 $O(n+k)$、非原地排序**
	需要藉助 $k$ 個桶和總共 $n$ 個元素的額外空間。
- 桶排序是否穩定取決於排序桶內元素的演算法是否穩定。

## Achieve Even Distribution
桶排序的時間複雜度理論上可以達到 ＄ ，**關鍵在於將元素均勻分配到各個桶中**，因為實際資料往往不是均勻分佈的。
### Example
我們想要將淘寶上的所有商品按價格範圍平均分配到 10 個桶中，但商品價格分佈不均，低於 100 元的非常多，高於 1000 元的非常少。若將價格區間平均劃分為 10 個，各個桶中的商品數量差距會非常大。

為實現平均分配，我們可以先設定一條大致的分界線，將資料粗略地分到 3 個桶中。**分配完畢後，再將商品較多的桶繼續劃分為 3 個桶，直至所有桶中的元素數量大致相等**。在此範例中，這種方法本質上是建立一棵遞迴樹，目標是讓葉節點的值儘可能平均。當然，不一定要每輪將資料劃分為 3 個桶，具體劃分方式可根據資料特點靈活選擇。如果我們提前知道商品價格的機率分佈，**則可以根據資料機率分佈設定每個桶的價格分界線**。值得注意的是，資料分佈並不一定需要特意統計，也可以根據資料特點採用某種機率模型進行近似。
我們假設商品價格服從常態分佈，這樣就可以合理地設定價格區間，從而將商品平均分配到各個桶中。

---
# Counting Sort
## Introduction
計數排序（Counting sort）透過統計元素數量來實現排序，通常應用於整數陣列。

## Example: A simple implementation
先來看一個簡單的例子。給定一個長度為 $n$ 的陣列 `nums` ，其中的元素都是「非負整數」，計數排序的整體流程下：
```md
unsorted
nums: [1,0,1,2,0,4,0,2,2,4]
---
index  :  0 1 2 3 4
counter: [3,2,3,0,3]
---
sorted
nums: [0,0,0,1,1,2,2,2,4,4]
```
1. 走訪陣列，找出其中的最大數字，記為 $m$ ，然後建立一個長度為 $m+1$ 的輔助陣列 `counter` 。
2. **藉助 `counter` 統計 `nums` 中各數字的出現次數**，其中 `counter[num]` 對應數字 `num` 的出現次數。統計方法很簡單，只需走訪 `nums`（設當前數字為 `num`），每輪將 `counter[num]` 增加 $1$ 即可。
3. **由於 `counter` 的各個索引天然有序，因此相當於所有數字已經排序好了**。接下來，我們走訪 `counter` ，根據各數字出現次數從小到大的順序填入 `nums` 即可。

看到這邊，你可能會發現 Counting Sort 跟 [[#Bucket Sort]] 有非常多相像之處。
> [!INFO] 從桶排序的角度看，我們可以將計數排序中的計數陣列 `counter` 的每個索引視為一個桶，將統計數量的過程看作將各個元素分配到對應的桶中。本質上，計數排序是桶排序在整型資料下的一個特例。

但是，如果輸入的資料形態並非數字的話，上述方法（`3.`）就不管用了。因此，對於非數字形態的輸入，我們首先計算 `counter` 的「前綴和」。索引 `i` 處的前綴和 `prefix[i]` 等於陣列前 `i` 個元素之和：
$$\text{prefix}[i] = \sum_{j=0}^{i} \text{counter}[j] $$
```md
index  :   0  1  2  3  4
counter: [ 3, 2, 3, 0, 4]
prefix:  [ 3, 5, 8, 8,10]

prefix[0] = counter[0]
prefix[1] = counter[0] + counter[1]
prefix[2] = counter[0] + counter[1] + counter[2]
.
.
.
```

**前綴和具有明確的意義，`prefix[num] - 1` 代表元素 `num` 在結果陣列 `res` 中最後一次出現的索引**。這個資訊非常關鍵，因為它告訴我們各個元素應該出現在結果陣列的哪個位置。接下來，我們倒序走訪原陣列 `nums` 的每個元素 `num` ，在每輪迭代中執行以下兩步：
1. 將 `num` 填入陣列 `res` 的索引 `prefix[num] - 1` 處。
2. 令前綴和 `prefix[num]` 減小 $1$ ，從而得到下次放置 `num` 的索引。

走訪完成後，陣列 `res` 中就是排序好的結果，最後使用 `res` 覆蓋原陣列 `nums` 即可。

## Code
以下是完整版（可應對不同形態輸入的版本）
~~~tabs
tab: C
```c
/* 計數排序 */
// 完整實現，可排序物件，並且是穩定排序
void countingSort(int nums[], int size) {
    // 1. 統計陣列最大元素 m
    int m = 0;
    for (int i = 0; i < size; i++) {
        if (nums[i] > m) {
            m = nums[i];
        }
    }
    // 2. 統計各數字的出現次數
    // counter[num] 代表 num 的出現次數
    int *counter = calloc(m, sizeof(int));
    for (int i = 0; i < size; i++) {
        counter[nums[i]]++;
    }
    // 3. 求 counter 的前綴和，將“出現次數”轉換為“尾索引”
    // 即 counter[num]-1 是 num 在 res 中最後一次出現的索引
    for (int i = 0; i < m; i++) {
        counter[i + 1] += counter[i];
    }
    // 4. 倒序走訪 nums ，將各元素填入結果陣列 res
    // 初始化陣列 res 用於記錄結果
    int *res = malloc(sizeof(int) * size);
    for (int i = size - 1; i >= 0; i--) {
        int num = nums[i];
        res[counter[num] - 1] = num; // 將 num 放置到對應索引處
        counter[num]--;              // 令前綴和自減 1 ，得到下次放置 num 的索引
    }
    // 使用結果陣列 res 覆蓋原陣列 nums
    memcpy(nums, res, size * sizeof(int));
    // 5. 釋放記憶體
    free(res);
    free(counter);
}
```

tab: C++
```cpp
/* 計數排序 */
// 完整實現，可排序物件，並且是穩定排序
void countingSort(vector<int> &nums) {
    // 1. 統計陣列最大元素 m
    int m = 0;
    for (int num : nums) {
        m = max(m, num);
    }
    // 2. 統計各數字的出現次數
    // counter[num] 代表 num 的出現次數
    vector<int> counter(m + 1, 0);
    for (int num : nums) {
        counter[num]++;
    }
    // 3. 求 counter 的前綴和，將“出現次數”轉換為“尾索引”
    // 即 counter[num]-1 是 num 在 res 中最後一次出現的索引
    for (int i = 0; i < m; i++) {
        counter[i + 1] += counter[i];
    }
    // 4. 倒序走訪 nums ，將各元素填入結果陣列 res
    // 初始化陣列 res 用於記錄結果
    int n = nums.size();
    vector<int> res(n);
    for (int i = n - 1; i >= 0; i--) {
        int num = nums[i];
        res[counter[num] - 1] = num; // 將 num 放置到對應索引處
        counter[num]--;              // 令前綴和自減 1 ，得到下次放置 num 的索引
    }
    // 使用結果陣列 res 覆蓋原陣列 nums
    nums = res;
}
```

tab: Python
```python
def counting_sort(nums: list[int]):
    """計數排序"""
    # 完整實現，可排序物件，並且是穩定排序
    # 1. 統計陣列最大元素 m
    m = max(nums)
    # 2. 統計各數字的出現次數
    # counter[num] 代表 num 的出現次數
    counter = [0] * (m + 1)
    for num in nums:
        counter[num] += 1
    # 3. 求 counter 的前綴和，將“出現次數”轉換為“尾索引”
    # 即 counter[num]-1 是 num 在 res 中最後一次出現的索引
    for i in range(m):
        counter[i + 1] += counter[i]
    # 4. 倒序走訪 nums ，將各元素填入結果陣列 res
    # 初始化陣列 res 用於記錄結果
    n = len(nums)
    res = [0] * n
    for i in range(n - 1, -1, -1):
        num = nums[i]
        res[counter[num] - 1] = num  # 將 num 放置到對應索引處
        counter[num] -= 1  # 令前綴和自減 1 ，得到下次放置 num 的索引
    # 使用結果陣列 res 覆蓋原陣列 nums
    for i in range(n):
        nums[i] = res[i]
```
~~~

## Characteristics
- **時間複雜度為 $O(n+m)$、非自適應排序**
	涉及走訪 `nums` 和走訪 `counter` ，都使用線性時間。一般情況下 $n≫m$ ，時間複雜度趨於 $O(n)$ 。
- **空間複雜度為 $O(n+m)$、非原地排序**
	藉助了長度分別為 $n$ 和 $m$ 的陣列 `res` 和 `counter` 。
- **穩定排序**
	由於向 `res` 中填充元素的順序是「從右向左」的，因此倒序走訪 `nums` 可以避免改變相等元素之間的相對位置，從而實現穩定排序。實際上，正序走訪 `nums` 也可以得到正確的排序結果，但結果是非穩定的。

## Limitations
看到這裡，你也許會覺得計數排序非常巧妙，僅透過統計數量就可以實現高效的排序。然而，使用計數排序的前置條件相對較為嚴格。

- **計數排序只適用於非負整數**
	若想將其用於其他型別的資料，需要確保這些資料可以轉換為非負整數，並且在轉換過程中不能改變各個元素之間的相對大小關係。例如，對於包含負數的整數陣列，可以先給所有數字加上一個常數，將全部數字轉化為正數，排序完成後再轉換回去。
- **計數排序適用於資料量大但資料範圍較小的情況**
	比如，在上述示例中 $m$ 不能太大，否則會佔用過多空間。而當 $n≪m$ 時，計數排序使用 $O(m)$ 時間，可能比 $O(n\log ⁡n)$ 的排序演算法還要慢。

---
# Radix Sort
## Introduction
假設我們需要對 $n=10^6$ 個學號進行排序，而學號是一個 $8$ 位數字，這意味著資料範圍 $m=10^8$ 非常大，使用[[#Counting Sort|計數排序]]需要分配大量記憶體空間，而基數排序可以避免這種情況。

基數排序（Radix sort）的核心思想與計數排序一致，也透過統計個數來實現排序。在此基礎上，基數排序利用數字各位之間的遞進關係，依次對每一位進行排序，從而得到最終的排序結果。

以學號資料為例，假設數字的最低位是第 $1$ 位，最高位是第 $8$ 位，基數排序的流程如下：
1. 初始化位數 $k=1$ 。
2. 對學號的第 $k$ 位執行「計數排序」。完成後，資料會根據第 $k$ 位從小到大排序。
3. 將 $k$ 增加 $1$ ，然後返回步驟 `2.` 繼續迭代，直到所有位都排序完成後結束。

```md                
k = 1     ->  .......!  ->  ......!.  -> ...  ->  !.......
10546151      35663510      35663510              10546151
35663510      88906420      88906420              30524779
42865989      10546151      82060337              34862445
34862445      72429244      72429244              35663510
81883077      34862445      34862445              42865989
88906420      63832996      10546151              63832996
72429244      81883077      81883077              72429244
30524779      82060337      30524779              81883077
82060337      42865989      42865989              82060337
63832996      30524779      63832996              88906420
```

對於一個 $d$ 進位制的數字 $x$ ，要獲取其第 $k$ 位 $x_k$ ，可以使用以下計算公式：
$$x_k = \lfloor\frac{x}{d^{k-1}}\rfloor\mod d$$
其中 $⌊a⌋$ 表示對浮點數 $a$ 向下取整，而 $\mod d$ 表示對 $d$ 取模（取餘）。

## Code
基數排序有使用到[[#Counting Sort|計數排序]]的程式碼。
~~~tabs
tab: C
```c
/* 獲取元素 num 的第 k 位，其中 exp = 10^(k-1) */
int digit(int num, int exp) {
    // 傳入 exp 而非 k 可以避免在此重複執行昂貴的次方計算
    return (num / exp) % 10;
}

/* 計數排序（根據 nums 第 k 位排序） */
void countingSortDigit(int nums[], int size, int exp) {
    // 十進位制的位範圍為 0~9 ，因此需要長度為 10 的桶陣列
    int *counter = (int *)malloc((sizeof(int) * 10));
    memset(counter, 0, sizeof(int) * 10); // 初始化為 0 以支持後續記憶體釋放
    // 統計 0~9 各數字的出現次數
    for (int i = 0; i < size; i++) {
        // 獲取 nums[i] 第 k 位，記為 d
        int d = digit(nums[i], exp);
        // 統計數字 d 的出現次數
        counter[d]++;
    }
    // 求前綴和，將“出現個數”轉換為“陣列索引”
    for (int i = 1; i < 10; i++) {
        counter[i] += counter[i - 1];
    }
    // 倒序走訪，根據桶內統計結果，將各元素填入 res
    int *res = (int *)malloc(sizeof(int) * size);
    for (int i = size - 1; i >= 0; i--) {
        int d = digit(nums[i], exp);
        int j = counter[d] - 1; // 獲取 d 在陣列中的索引 j
        res[j] = nums[i];       // 將當前元素填入索引 j
        counter[d]--;           // 將 d 的數量減 1
    }
    // 使用結果覆蓋原陣列 nums
    for (int i = 0; i < size; i++) {
        nums[i] = res[i];
    }
    // 釋放記憶體
    free(res);
    free(counter);
}

/* 基數排序 */
void radixSort(int nums[], int size) {
    // 獲取陣列的最大元素，用於判斷最大位數
    int max = INT32_MIN;
    for (int i = 0; i < size; i++) {
        if (nums[i] > max) {
            max = nums[i];
        }
    }
    // 按照從低位到高位的順序走訪
    for (int exp = 1; max >= exp; exp *= 10)
        // 對陣列元素的第 k 位執行計數排序
        // k = 1 -> exp = 1
        // k = 2 -> exp = 10
        // 即 exp = 10^(k-1)
        countingSortDigit(nums, size, exp);
}
```

tab: C++
```cpp
/* 獲取元素 num 的第 k 位，其中 exp = 10^(k-1) */
int digit(int num, int exp) {
    // 傳入 exp 而非 k 可以避免在此重複執行昂貴的次方計算
    return (num / exp) % 10;
}

/* 計數排序（根據 nums 第 k 位排序） */
void countingSortDigit(vector<int> &nums, int exp) {
    // 十進位制的位範圍為 0~9 ，因此需要長度為 10 的桶陣列
    vector<int> counter(10, 0);
    int n = nums.size();
    // 統計 0~9 各數字的出現次數
    for (int i = 0; i < n; i++) {
        int d = digit(nums[i], exp); // 獲取 nums[i] 第 k 位，記為 d
        counter[d]++;                // 統計數字 d 的出現次數
    }
    // 求前綴和，將“出現個數”轉換為“陣列索引”
    for (int i = 1; i < 10; i++) {
        counter[i] += counter[i - 1];
    }
    // 倒序走訪，根據桶內統計結果，將各元素填入 res
    vector<int> res(n, 0);
    for (int i = n - 1; i >= 0; i--) {
        int d = digit(nums[i], exp);
        int j = counter[d] - 1; // 獲取 d 在陣列中的索引 j
        res[j] = nums[i];       // 將當前元素填入索引 j
        counter[d]--;           // 將 d 的數量減 1
    }
    // 使用結果覆蓋原陣列 nums
    for (int i = 0; i < n; i++)
        nums[i] = res[i];
}

/* 基數排序 */
void radixSort(vector<int> &nums) {
    // 獲取陣列的最大元素，用於判斷最大位數
    int m = *max_element(nums.begin(), nums.end());
    // 按照從低位到高位的順序走訪
    for (int exp = 1; exp <= m; exp *= 10)
        // 對陣列元素的第 k 位執行計數排序
        // k = 1 -> exp = 1
        // k = 2 -> exp = 10
        // 即 exp = 10^(k-1)
        countingSortDigit(nums, exp);
}
```

tab: Python
```python
def digit(num: int, exp: int) -> int:
    """獲取元素 num 的第 k 位，其中 exp = 10^(k-1)"""
    # 傳入 exp 而非 k 可以避免在此重複執行昂貴的次方計算
    return (num // exp) % 10

def counting_sort_digit(nums: list[int], exp: int):
    """計數排序（根據 nums 第 k 位排序）"""
    # 十進位制的位範圍為 0~9 ，因此需要長度為 10 的桶陣列
    counter = [0] * 10
    n = len(nums)
    # 統計 0~9 各數字的出現次數
    for i in range(n):
        d = digit(nums[i], exp)  # 獲取 nums[i] 第 k 位，記為 d
        counter[d] += 1  # 統計數字 d 的出現次數
    # 求前綴和，將“出現個數”轉換為“陣列索引”
    for i in range(1, 10):
        counter[i] += counter[i - 1]
    # 倒序走訪，根據桶內統計結果，將各元素填入 res
    res = [0] * n
    for i in range(n - 1, -1, -1):
        d = digit(nums[i], exp)
        j = counter[d] - 1  # 獲取 d 在陣列中的索引 j
        res[j] = nums[i]  # 將當前元素填入索引 j
        counter[d] -= 1  # 將 d 的數量減 1
    # 使用結果覆蓋原陣列 nums
    for i in range(n):
        nums[i] = res[i]

def radix_sort(nums: list[int]):
    """基數排序"""
    # 獲取陣列的最大元素，用於判斷最大位數
    m = max(nums)
    # 按照從低位到高位的順序走訪
    exp = 1
    while exp <= m:
        # 對陣列元素的第 k 位執行計數排序
        # k = 1 -> exp = 1
        # k = 2 -> exp = 10
        # 即 exp = 10^(k-1)
        counting_sort_digit(nums, exp)
        exp *= 10
```
~~~

## Characteristics
相較於計數排序，基數排序適用於數值範圍較大的情況，**但前提是資料必須可以表示為固定位數的格式，且位數不能過大**。例如，浮點數不適合使用基數排序，因為其位數 $k$ 過大，可能導致時間複雜度 $O(nk)≫O(n^2)$ 。
- **時間複雜度為 $O(nk)$、非自適應排序**
	設資料量為 $n$、資料為 $d$ 進位制、最大位數為 $k$ ，則對某一位執行計數排序使用 $O(n+d)$ 時間，排序所有 $k$ 位使用 $O((n+d)k)$ 時間。通常情況下，$d$ 和 $k$ 都相對較小，時間複雜度趨向 $O(n)$ 。
- **空間複雜度為 $O(n+d)$、非原地排序**
	與計數排序相同，基數排序需要藉助長度為 n 和 d 的陣列 `res` 和 `counter` 。
- **穩定排序**
	當計數排序穩定時，基數排序也穩定；當計數排序不穩定時，基數排序無法保證得到正確的排序結果。

---
# Comparison on Common Sorting Algorithms
![[sorting_algorithms_comparison.png]]

以上為常見通用，較為基礎的排序法，但這些排序法再某些時候可能無法達到我們想要任務，像是在特定時間內與有限空間內完成排序。因此，以下將介紹進階的排序演算法。

---
# TimSort
## Introduction
TimSort 當初被 Tim Peters 選中進而實作出來是因為當時 Python 2.2 以前採用的 [[Sorting#Quick Sort|快速排序]] 引發的以下幾個問題：
1. [[Sorting#Quick Sort|快速排序]] 是一個不穩定的排序法，這意味著排序後相同的元素的相對位置可能會改變。 這在實際應用場景中會是個災難，在一些現實情況是必須允許有相同的元素存在，但是又必須保持他們的相對位置不變。例如排序時，如果資料之間擁有相同的 `key`，那麼排序後的結果就會變得不可預測。
2. 理論科學家通常並不是很在意時間複雜度 $O$ 記號的常數項，但在實際應用中這些常數項往往就是影響效能的關鍵。 [[Sorting#Quick Sort|快速排序]] 在面對快要排序好的資料時，效能表現會變得很差，最壞情況下的時間複雜度是 $O(n^2)$，而這樣的資料在實際應用中是很常見的。
3. 理論測試資料往往是隨機的，但這在實際應用中並不是很常見，實際應用中的資料往往有一定的規律性，這就會導致理論科學家測試的結果和實際應用的效能表現有很大的差異。

TimSort 的核心思想是將資料分成多個小塊，然後對這些小塊使用 [[Sorting#Insertion Sort|插入排序]] 進行排序，最後再將這些小塊使用 [[Sorting#Merge Sort|合併排序]] 合併成一個大塊。這樣的好處是：
1. 因材施教：[[Sorting#Insertion Sort|插入排序]] 在面對小塊資料時效能表現出色，而 [[Sorting#Merge Sort|合併排序]] 在面對處理大規模資料時效率更高。TimSort 巧妙地結合了這兩種演算法，根據資料的特性自動切換，發揮它們各自的長處。
2. 保證效率：無論在最佳、平均還是最壞情況下，其時間複雜度都能保證在 $O(n\log n)$ 。這使得它成為一種高效且可靠的排序演算法。
3. 穩定性：TimSort 是一種穩定的排序演算法，這意味著相等元素的相對位置在排序前後不會改變。這一特性在某些應用場景中非常重要。
4. 適應性：能夠根據資料的分佈特點自動調整策略。當面對部分已經排序的資料時，它能夠充分利用資料的有序性，從而獲得更好的性能表現。

演算法可以分成以下幾個部分：
1. 分割資料：將資料分割成多個小塊，每個小塊的大小為 `minrun`，通常取 $32$ 或 $64$。
2. [[Sorting#Insertion Sort|插入排序]]：對每個小塊應用插入排序，使其成為一個有序的小塊。
3. [[Sorting#Merge Sort|合併排序]]：使用合併排序將這些有序的小塊逐步合併，直到整個資料集完全有序。
4. 重複合併（Galloping)：重複步驟 3，直到所有的小塊都合併成一個完整的有序資料集。
![[Drawing - TimSort|100%x500]]

## Merging and Galloping in TimSort
TimSort 透過一個「**運行棧（Stack of Runs）**」來管理待合併的 runs，並根據**特定條件**來決定何時合併。這些條件主要是為了**避免合併過程中產生不平衡的合併開銷**，讓時間複雜度維持在 $O(n \log n)$。
TimSort 設定了兩個**合併條件**，它們分別是：
1. $X > Y + Z$（避免不平衡合併）
	設 $X$, $Y$, $Z$ 為棧頂的三個 run 的長度，其中 $X$ 是最新加入的 run，$Y$ 是次新的 run，$Z$ 是更舊的 run。
	TimSort 保證「較長的 run 不會被短 run 擠壓到合併時機」，也就是：
	$X > Y + Z$， **如果這個條件不滿足**（也就是 $X ≤ Y + Z$），則需要合併 $Y$ 和 $Z$，這樣可以讓合併過程更平衡，避免長度相差過大的 run 進行非必要的合併。
2. $Y > X$（避免過小的 run 與大 run 直接合併）
	**當 $Y ≤ X$ 時，TimSort 會優先合併 $Y$ 和 $Z$（如果 $Z$ 存在）**，而非直接合併 $X$ 和 $Y$。
	這樣可以**避免小 run 被大 run 提前合併，導致非必要的計算開銷**。

**為什麼這樣合併效率更高？**
1. 避免小的 run 影響大 run 的合併順序
	如果直接用最簡單的兩兩合併，可能會造成**長度相差過大的 runs 直接合併**，導致較長的 run 反覆被小 run 影響。透過 $X > Y + Z$ 和 $Y > X$ 這兩條規則，可以**確保 run 合併過程始終保持平衡**。
2. 避免最差情況退化成 $O(n^2)$
	若 run 合併過程不平衡，可能導致某些 run 被頻繁合併，增加額外的計算成本。TimSort 透過這些規則確保合併數量大致維持在 $\log(n)$ 次，避免不必要的計算。

### Example
假設 TimSort 將 list 分割成 $30$ 個 runs，長度如下：
```md
run1: 5, run2: 10, run3: 15, run4: 20, run5: 25, ..., run30: 150
```
這 $30$ 個 runs 已經完成插入排序，現在進入合併階段。
#### 步驟 1：建構「運行棧」（Stack of Runs)
TimSort 會用 **棧（stack）** 來維護目前尚未合併的 runs。棧的初始狀態如下：
```md
[run1, run2, run3, ..., run30]
```
此時，每當加入一個新的 run，TimSort 會檢查**是否違反合併條件**，如果違反，就進行合併。

#### 步驟 2：檢查 X > Y + Z
假設我們現在有：
```md
[run28: 120, run29: 130, run30: 150]
```

則檢查是否滿足條件：
```python
150 > 130 + 120 ？ #False
```

條件**不滿足，因此合併 run28 和 run29**：
```md
[run27: ..., (run28+run29): 250, run30: 150]
```
並繼續檢查新的 `run28+run29` 是否滿足條件，若仍不滿足則繼續合併。

#### 步驟 3：檢查 Y > X
如果 $Y$ 的大小比 $X$ 小，那麼就會合併 $Y$ 和 $Z$，而不是直接合併 $X$ 和 $Y$，以避免產生極端情況。以步驟 2 的結果為例，如果 `run27` 的大小大於 `run28+run29`，則 TimSort 演算法會合併 `run28+run29` 與 `run30`。但是，假設今天狀況如下：
```md
run28: 120, run29: 130, run30: 150
```

> [!INFO] 需要注意的是，這時候雖然滿足 $Y > X$ ，**但是 $Z > Y$ ，因此會將較小的兩個runs `run28` 與 `run29` 優先合併。因為這樣可以讓合併過程更加均衡。**

## Code
以下僅提供 Python 範例程式碼，此段程式碼範例僅為簡易實作版本，提供讀者參考其運作原理。實際上，Python 中的 TimSort 是以 C 實作而成。
~~~tabs
tab: Python
```python
def insertion_sort(arr, left, right):  
    for i in range(left + 1, right + 1):  
        key = arr[i]  
        j = i - 1  
        while j >= left and arr[j] > key:  
            arr[j + 1] = arr[j]  
            j -= 1  
        arr[j + 1] = key  
  
  
def merge(arr, left, mid, right):  
    left_arr = arr[left:mid + 1]  
    right_arr = arr[mid + 1:right + 1]  
    i = j = 0  
    k = left  
  
    while i < len(left_arr) and j < len(right_arr):  
        if left_arr[i] <= right_arr[j]:  
            arr[k] = left_arr[i]  
            i += 1  
        else:  
            arr[k] = right_arr[j]  
            j += 1  
        k += 1  
  
    while i < len(left_arr):  
        arr[k] = left_arr[i]  
        i += 1  
        k += 1  
  
    while j < len(right_arr):  
        arr[k] = right_arr[j]  
        j += 1  
        k += 1  
  
  
def timsort(arr):  
    min_run = 64  
    n = len(arr)  
  
    for i in range(0, n, min_run):  
        insertion_sort(arr, i, min(i + min_run - 1, n - 1))  
  
    size = min_run  
    while size < n:  
        for start in range(0, n, size * 2):  
            mid = start + size - 1  
            end = min(start + size * 2 - 1, n - 1)  
            merge(arr, start, mid, end)  
        size *= 2  
  
    return arr
```
~~~

## Characteristics
- 時間複雜度、自適應
	最佳情形 $O(n)$，最差情形 $O(n\ log n)$，平均 $O(n \log n)$
- 空間複雜度 $O(n)$、非原地
- 穩定排序

---
# Cyclic Sort


---

# Q & A
> [!QUESTION]- 排序演算法穩定性在什麼情況下是必需的？
在現實中，我們有可能基於物件的某個屬性進行排序。例如，學生有姓名和身高兩個屬性，我們希望實現一個多級排序：先按照姓名進行排序，得到 `(A, 180) (B, 185) (C, 170) (D, 170)` ；再對身高進行排序。由於排序演算法不穩定，因此可能得到 `(D, 170) (C, 170) (A, 180) (B, 185)` 。
> 
可以發現，學生 D 和 C 的位置發生了交換，姓名的有序性被破壞了，而這是我們不希望看到的。

> [!QUESTION]- 在[[#Quick Sort|快速排序]]中，哨兵劃分中「從右往左查詢」與「從左往右查詢」的順序可以交換嗎？
> 哨兵劃分 `partition()` 的最後一步是交換 `nums[left]` 和 `nums[i]` 。完成交換後，基準數左邊的元素都 `<=` 基準數，**這就要求最後一步交換前 `nums[left] >= nums[i]` 必須成立**。假設我們先“從左往右查詢”，那麼如果找不到比基準數更大的元素，**則會在 `i == j` 時跳出迴圈，此時可能 `nums[j] == nums[i] > nums[left]`**。也就是說，此時最後一步交換操作會把一個比基準數更大的元素交換至陣列最左端，導致哨兵劃分失敗。
> 舉個例子，給定陣列 `[0, 0, 0, 0, 1]` ，如果先「從左向右查詢」，哨兵劃分後陣列為 `[1, 0, 0, 0, 0]` ，這個結果是不正確的。
> 再深入思考一下，如果我們選擇 `nums[right]` 為基準數，那麼正好反過來，必須先「從左往右查詢」。

