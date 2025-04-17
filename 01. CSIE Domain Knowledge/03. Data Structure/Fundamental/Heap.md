閱讀此篇筆記之前，請先閱讀：
- [[Binary Tree]]
- [[Binary Search Tree]]

## Introduction
堆積（Heap）是一種滿足特定條件的完全二元樹，主要可分為兩種類型：
1. 小頂堆積（Min heap）：任意節點的值 $≤$ 其子節點的值。
2. 大頂堆積（Max heap）：任意節點的值 $≥$ 其子節點的值。

堆積作為完全二元樹的一個特例，具有以下特性：
- 最底層節點靠左填充，其他層的節點都被填滿。
- 我們將二元樹的根節點稱為「堆積頂（Heap top）」，將底層最靠右的節點稱為「堆積底（Heap bottom）」。
- 對於大頂堆積（小頂堆積），堆積頂元素（根節點）的值是最大（最小）的。

## Typical Applications
- **優先佇列**（Priority Queue）
	堆積通常作為實現優先佇列的首選資料結構，其入列和出列操作的時間複雜度均為 $O(\log ⁡n)$ ，而建堆積操作為 $O(n)$ ，這些操作都非常高效。
- **堆積排序**（Heap sort）
	給定一組資料，我們可以用它們建立一個堆積，然後不斷地執行元素出堆積操作，從而得到有序資料。然而，我們通常會使用一種更優雅的方式實現堆積排序，請參閱[[Sorting#Heap Sort|Heap Sort]]。
- **[[#Top-k Elements|獲取最大的 k 個元素（Top-k elements）]]**
	這是一個經典的演算法問題，同時也是一種典型應用，例如選擇熱度前 10 的新聞作為微博熱搜，選取銷量前 10 的商品等。

## Common Operations
> [!CAUTION] 由於 C 未內建堆疊類別，以下常見操作不提供 C 的程式碼。

需要指出的是，許多程式語言提供的是優先佇列（Priority Queue），這是一種抽象的資料結構，定義為具有優先順序排序的佇列。實際上，**堆積通常用於實現優先佇列，大頂堆積相當於元素按從大到小的順序出列的優先佇列**。從使用角度來看，我們可以將「優先佇列」和「堆積」看作等價的資料結構。
堆積的常用操作見下表，方法名需要根據程式語言來確定。

| 方法名         | 描述                                | 時間複雜度        |
| ----------- | --------------------------------- | ------------ |
| `push()`    | 元素入堆積                             | $O(\log ⁡n)$ |
| `pop()`     | 堆積頂元素出堆積                          | $O(\log ⁡n)$ |
| `peek()`    | 訪問堆積頂元素<br>（對於大 / 小頂堆積分別為最大 / 小值） | $O(1)$       |
| `size()`    | 獲取堆積的元素數量                         | $O(1)$       |
| `isEmpty()` | 判斷堆積是否為空                          | $O(1)$       |

在實際應用中，我們可以直接使用程式語言提供的堆積類別（或優先佇列類別）。
類似於排序演算法中的「從小到大排列」和「從大到小排列」，我們可以透過設定一個 `flag` 或修改 `Comparator` 實現「小頂堆積」與「大頂堆積」之間的轉換。

~~~tabs
tab: C++
```cpp
/* 初始化堆積 */
// 初始化小頂堆積
priority_queue<int, vector<int>, greater<int>> minHeap;
// 初始化大頂堆積
priority_queue<int, vector<int>, less<int>> maxHeap;

/* 元素入堆積 */
maxHeap.push(1);
maxHeap.push(3);
maxHeap.push(2);
maxHeap.push(5);
maxHeap.push(4);

/* 獲取堆積頂元素 */
int peek = maxHeap.top(); // 5

/* 堆積頂元素出堆積 */
// 出堆積元素會形成一個從大到小的序列
maxHeap.pop(); // 5
maxHeap.pop(); // 4
maxHeap.pop(); // 3
maxHeap.pop(); // 2
maxHeap.pop(); // 1

/* 獲取堆積大小 */
int size = maxHeap.size();

/* 判斷堆積是否為空 */
bool isEmpty = maxHeap.empty();

/* 輸入串列並建堆積 */
vector<int> input{1, 3, 2, 5, 4};
priority_queue<int, vector<int>, greater<int>> minHeap(input.begin(), input.end());
```

tab: Python
```python
# 初始化小頂堆積
min_heap, flag = [], 1
# 初始化大頂堆積
max_heap, flag = [], -1

# Python 的 heapq 模組預設實現小頂堆積
# 考慮將“元素取負”後再入堆積，這樣就可以將大小關係顛倒，從而實現大頂堆積
# 在本示例中，flag = 1 時對應小頂堆積，flag = -1 時對應大頂堆積

# 元素入堆積
heapq.heappush(max_heap, flag * 1)
heapq.heappush(max_heap, flag * 3)
heapq.heappush(max_heap, flag * 2)
heapq.heappush(max_heap, flag * 5)
heapq.heappush(max_heap, flag * 4)

# 獲取堆積頂元素
peek: int = flag * max_heap[0] # 5

# 堆積頂元素出堆積
# 出堆積元素會形成一個從大到小的序列
val = flag * heapq.heappop(max_heap) # 5
val = flag * heapq.heappop(max_heap) # 4
val = flag * heapq.heappop(max_heap) # 3
val = flag * heapq.heappop(max_heap) # 2
val = flag * heapq.heappop(max_heap) # 1

# 獲取堆積大小
size: int = len(max_heap)

# 判斷堆積是否為空
is_empty: bool = not max_heap

# 輸入串列並建堆積
min_heap: list[int] = [1, 3, 2, 5, 4]
heapq.heapify(min_heap)
```
~~~

## Implememtation
### Storage and representation
[[Binary Tree#Array Representation|「二元樹」章節講過，完全二元樹非常適合用陣列來表示]]。由於堆積正是一種完全二元樹，**因此我們將採用陣列來儲存堆積**。當使用陣列表示二元樹時，元素代表節點值，索引代表節點在二元樹中的位置。**節點指標透過索引對映公式來實現**。

給定索引 $i$ ，其左子節點的索引為 $2i+1$ ，右子節點的索引為 $2i+2$ ，父節點的索引為 $\lfloor (i−1)/2 \rfloor$，當索引越界時，表示空節點或節點不存在。我們可以將索引對映公式封裝成函式，方便後續使用。
~~~tabs
tab: C
```c
/* 獲取左子節點的索引 */
int left(MaxHeap *maxHeap, int i) {
    return 2 * i + 1;
}

/* 獲取右子節點的索引 */
int right(MaxHeap *maxHeap, int i) {
    return 2 * i + 2;
}

/* 獲取父節點的索引 */
int parent(MaxHeap *maxHeap, int i) {
    return (i - 1) / 2; // 向下取整
}
```

tab: C++
```cpp
/* 獲取左子節點的索引 */
int left(int i) {
    return 2 * i + 1;
}

/* 獲取右子節點的索引 */
int right(int i) {
    return 2 * i + 2;
}

/* 獲取父節點的索引 */
int parent(int i) {
    return (i - 1) / 2; // 向下整除
}
```

tab: Python
```python
def left(self, i: int) -> int:
    """獲取左子節點的索引"""
    return 2 * i + 1

def right(self, i: int) -> int:
    """獲取右子節點的索引"""
    return 2 * i + 2

def parent(self, i: int) -> int:
    """獲取父節點的索引"""
    return (i - 1) // 2  # 向下整除
```
~~~
### Accessing the top element
堆積頂元素即為二元樹的根節點，也就是串列的首個元素。

~~~tabs
tab: C
```c
/* 訪問堆積頂元素 */
int peek(MaxHeap *maxHeap) {
    return maxHeap->data[0];
}
```

tab: C++
```cpp
/* 訪問堆積頂元素 */
int peek() {
    return maxHeap[0];
}
```

tab: Python
```python
def peek(self) -> int:
    """訪問堆積頂元素"""
    return self.max_heap[0]
```
~~~

### Inserting an element
給定元素 `val` ，我們首先將其新增到堆積底。新增之後，由於 `val` 可能大於堆積中其他元素，堆積的成立條件可能已被破壞，**因此需要修復從插入節點到根節點的路徑上的各個節點**，這個操作被稱為堆積化（heapify）。

考慮從入堆積節點開始，**從底至頂執行堆積化**。我們比較插入節點與其父節點的值，如果插入節點更大，則將它們交換。然後繼續執行此操作，從底至頂修復堆積中的各個節點，直至越過根節點或遇到無須交換的節點時結束。

設節點總數為 $n$ ，則樹的高度為 $O(\log ⁡n)$ 。由此可知，堆積化操作的迴圈輪數最多為 $O(\log ⁡n)$ ，**元素入堆積操作的時間複雜度為 $O(\log ⁡n)$** 。
~~~tabs
tab: C
```c
/* 元素入堆積 */
void push(MaxHeap *maxHeap, int val) {
    // 預設情況下，不應該新增這麼多節點
    if (maxHeap->size == MAX_SIZE) {
        printf("heap is full!");
        return;
    }
    // 新增節點
    maxHeap->data[maxHeap->size] = val;
    maxHeap->size++;

    // 從底至頂堆積化
    siftUp(maxHeap, maxHeap->size - 1);
}

/* 從節點 i 開始，從底至頂堆積化 */
void siftUp(MaxHeap *maxHeap, int i) {
    while (true) {
        // 獲取節點 i 的父節點
        int p = parent(maxHeap, i);
        // 當“越過根節點”或“節點無須修復”時，結束堆積化
        if (p < 0 || maxHeap->data[i] <= maxHeap->data[p]) {
            break;
        }
        // 交換兩節點
        swap(maxHeap, i, p);
        // 迴圈向上堆積化
        i = p;
    }
}
```

tab: C++
```cpp
/* 元素入堆積 */
void push(int val) {
    // 新增節點
    maxHeap.push_back(val);
    // 從底至頂堆積化
    siftUp(size() - 1);
}

/* 從節點 i 開始，從底至頂堆積化 */
void siftUp(int i) {
    while (true) {
        // 獲取節點 i 的父節點
        int p = parent(i);
        // 當“越過根節點”或“節點無須修復”時，結束堆積化
        if (p < 0 || maxHeap[i] <= maxHeap[p])
            break;
        // 交換兩節點
        swap(maxHeap[i], maxHeap[p]);
        // 迴圈向上堆積化
        i = p;
    }
}
```

tab: Python
```python
def push(self, val: int):
    """元素入堆積"""
    # 新增節點
    self.max_heap.append(val)
    # 從底至頂堆積化
    self.sift_up(self.size() - 1)

def sift_up(self, i: int):
    """從節點 i 開始，從底至頂堆積化"""
    while True:
        # 獲取節點 i 的父節點
        p = self.parent(i)
        # 當“越過根節點”或“節點無須修復”時，結束堆積化
        if p < 0 or self.max_heap[i] <= self.max_heap[p]:
            break
        # 交換兩節點
        self.swap(i, p)
        # 迴圈向上堆積化
        i = p
```
~~~

### Removing the top element
堆積頂元素是二元樹的根節點，即串列首元素。如果我們直接從串列中刪除首元素，那麼二元樹中所有節點的索引都會發生變化，這將使得後續使用堆積化進行修復變得困難。為了儘量減少元素索引的變動，我們採用以下操作步驟：
1. 交換堆積頂元素與堆積底元素（交換根節點與最右葉節點）。
2. 交換完成後，將堆積底從串列中刪除（注意，由於已經交換，因此實際上刪除的是原來的堆積頂元素）。
3. 從根節點開始，**從頂至底執行堆積化**。

**「從頂至底堆積化」的操作方向與「從底至頂堆積化」相反**，我們將根節點的值與其兩個子節點的值進行比較，將最大的子節點與根節點交換。然後迴圈執行此操作，直到越過葉節點或遇到無須交換的節點時結束。與元素入堆積操作相似，堆積頂元素出堆積操作的時間複雜度也為 $O(\log ⁡n)$ 。
~~~tabs
tab: C
```c
/* 元素出堆積 */
int pop(MaxHeap *maxHeap) {
    // 判空處理
    if (isEmpty(maxHeap)) {
        printf("heap is empty!");
        return INT_MAX;
    }
    // 交換根節點與最右葉節點（交換首元素與尾元素）
    swap(maxHeap, 0, size(maxHeap) - 1);
    // 刪除節點
    int val = maxHeap->data[maxHeap->size - 1];
    maxHeap->size--;
    // 從頂至底堆積化
    siftDown(maxHeap, 0);

    // 返回堆積頂元素
    return val;
}

/* 從節點 i 開始，從頂至底堆積化 */
void siftDown(MaxHeap *maxHeap, int i) {
    while (true) {
        // 判斷節點 i, l, r 中值最大的節點，記為 max
        int l = left(maxHeap, i);
        int r = right(maxHeap, i);
        int max = i;
        if (l < size(maxHeap) && maxHeap->data[l] > maxHeap->data[max]) {
            max = l;
        }
        if (r < size(maxHeap) && maxHeap->data[r] > maxHeap->data[max]) {
            max = r;
        }
        // 若節點 i 最大或索引 l, r 越界，則無須繼續堆積化，跳出
        if (max == i) {
            break;
        }
        // 交換兩節點
        swap(maxHeap, i, max);
        // 迴圈向下堆積化
        i = max;
    }
}
```

tab: C++
```cpp
/* 元素出堆積 */
void pop() {
    // 判空處理
    if (isEmpty()) {
        throw out_of_range("堆積為空");
    }
    // 交換根節點與最右葉節點（交換首元素與尾元素）
    swap(maxHeap[0], maxHeap[size() - 1]);
    // 刪除節點
    maxHeap.pop_back();
    // 從頂至底堆積化
    siftDown(0);
}

/* 從節點 i 開始，從頂至底堆積化 */
void siftDown(int i) {
    while (true) {
        // 判斷節點 i, l, r 中值最大的節點，記為 ma
        int l = left(i), r = right(i), ma = i;
        if (l < size() && maxHeap[l] > maxHeap[ma])
            ma = l;
        if (r < size() && maxHeap[r] > maxHeap[ma])
            ma = r;
        // 若節點 i 最大或索引 l, r 越界，則無須繼續堆積化，跳出
        if (ma == i)
            break;
        swap(maxHeap[i], maxHeap[ma]);
        // 迴圈向下堆積化
        i = ma;
    }
}
```

tab: Python
```python
def pop(self) -> int:
    """元素出堆積"""
    # 判空處理
    if self.is_empty():
        raise IndexError("堆積為空")
    # 交換根節點與最右葉節點（交換首元素與尾元素）
    self.swap(0, self.size() - 1)
    # 刪除節點
    val = self.max_heap.pop()
    # 從頂至底堆積化
    self.sift_down(0)
    # 返回堆積頂元素
    return val

def sift_down(self, i: int):
    """從節點 i 開始，從頂至底堆積化"""
    while True:
        # 判斷節點 i, l, r 中值最大的節點，記為 ma
        l, r, ma = self.left(i), self.right(i), i
        if l < self.size() and self.max_heap[l] > self.max_heap[ma]:
            ma = l
        if r < self.size() and self.max_heap[r] > self.max_heap[ma]:
            ma = r
        # 若節點 i 最大或索引 l, r 越界，則無須繼續堆積化，跳出
        if ma == i:
            break
        # 交換兩節點
        self.swap(i, ma)
        # 迴圈向下堆積化
        i = ma
```
~~~

## Construct a Heap
在某些情況下，我們希望使用串列的所有元素來構建一個堆積，以下說明常見的兩個方法：
### By heap insertion
一言以蔽之，就是將元素一個一個插入到新建的堆疊裡。我們首先建立一個空堆積，然後走訪串列，依次對每個元素執行「入堆積操作」，即先將元素新增至堆積的尾部，再對該元素執行「從底至頂」堆積化。每當一個元素入堆積，堆積的長度就 $+1$。由於節點是從頂到底依次被新增進二元樹的，因此堆積是「自上而下」構建的。

設元素數量為 $n$ ，每個元素的入堆積操作使用 $O(\log ⁡n)$ 時間，因此該建堆積方法的時間複雜度為 $O(n\log⁡ n)$ 。但是，這個方式的效率並不夠好。

### By heapifying through traversal
實際上，我們可以實現一種更為高效的建堆積方法，共分為兩步：
1. 將串列所有元素原封不動地新增到堆積中，此時堆積的性質尚未得到滿足。
2. 倒序走訪堆積（層序走訪的倒序），依次對每個非葉節點執行“從頂至底堆積化”。

**每當堆積化一個節點後，以該節點為根節點的子樹就形成一個合法的子堆積**。而由於是倒序走訪，因此堆積是「自下而上」構建的。之所以選擇倒序走訪，是因為這樣能夠保證當前節點之下的子樹已經是合法的子堆積，這樣堆積化當前節點才是有效的。

值得說明的是，**由於葉節點沒有子節點，因此它們天然就是合法的子堆積，無須堆積化**。
~~~tabs
tab: C
```c
/* 建構子，根據切片建堆積 */
MaxHeap *newMaxHeap(int nums[], int size) {
    // 所有元素入堆積
    MaxHeap *maxHeap = (MaxHeap *)malloc(sizeof(MaxHeap));
    maxHeap->size = size;
    memcpy(maxHeap->data, nums, size * sizeof(int));
    for (int i = parent(maxHeap, size - 1); i >= 0; i--) {
        // 堆積化除葉節點以外的其他所有節點
        siftDown(maxHeap, i);
    }
    return maxHeap;
}
```

tab: C++
```cpp
/* 建構子，根據輸入串列建堆積 */
MaxHeap(vector<int> nums) {
    // 將串列元素原封不動新增進堆積
    maxHeap = nums;
    // 堆積化除葉節點以外的其他所有節點
    for (int i = parent(size() - 1); i >= 0; i--) {
        siftDown(i);
    }
}
```

tab: Python
```python
def __init__(self, nums: list[int]):
    """建構子，根據輸入串列建堆積"""
    # 將串列元素原封不動新增進堆積
    self.max_heap = nums
    # 堆積化除葉節點以外的其他所有節點
    for i in range(self.parent(self.size() - 1), -1, -1):
        self.sift_down(i)
```
~~~
#### Complexity analysis
我們來嘗試推算[[#By heapifying through traversal|第二種]]建堆積方法的時間複雜度：

- 假設完全二元樹的節點數量為 $n$ ，則葉節點數量為 $(n+1)/2$ ，其中 `/` 為向下整除。因此需要堆積化的節點數量為 $(n−1)/2$ 。
- 在從頂至底堆積化的過程中，每個節點最多堆積化到葉節點，因此最大迭代次數為二元樹高度 $\log ⁡n$ 。

將上述兩者相乘，可得到建堆積過程的時間複雜度為 $O(n\log ⁡n)$。**但這個估算結果並不準確，因為我們沒有考慮到二元樹底層節點數量遠多於頂層節點的性質**。

接下來我們來進行更為準確的計算。為了降低計算難度，假設給定一個節點數量為 $n$ 、高度為 $h$ 的「完美二元樹」，該假設不會影響計算結果的正確性。

![[analysis_of_perfect_binary_tree.png]]

如上圖所示，節點「從頂至底堆積化」的最大迭代次數等於該節點到葉節點的距離，而該距離正是「節點高度」。因此，我們可以對各層的「節點數量 $×$ 節點高度」求和，**得到所有節點的堆積化迭代次數的總和**。
$$T(h)=2^0 h + 2^1 (h-1) + 2^2 (h-2) + \cdots + 2^{(h-1)}×1$$
化簡上式需要藉助中學的數列知識，先將 $T(h)$ 乘以 $2$ ，得到：
$$\displaylines{\begin{aligned} T(h) &= 2^0 h + 2^1 (h-1) + 2^2 (h-2) + \cdots + 2^{(h-1)}×1 \\ 
2T(h) &= 2^1 h + 2^2 (h-1) + 2^3 (h-2) + \cdots + 2^h×1 \end{aligned}}$$
使用錯位相減法，用下式 $2T(h)$ 減去上式 $T(h)$ ，可得：
$$2T(h)-T(h)=T(h)=-2^0 h + 2^1 + 2^2 + \cdots + 2^{h-1} + 2^h$$
觀察上式，發現 $T(h)$ 是一個等比數列，可直接使用求和公式，得到時間複雜度為：
$$\displaylines{\begin{aligned} T(h) &= 2\frac{1-2^h}{1-2}-h \\ &= 2^{h+1} -h-2 \\ &= O(2^h) \end{aligned}}$$
其中，高度為 $h$ 的完美二元樹的節點數量為 $n=2^{h+1}−1$ ，可得複雜度為 $O(2^h)=O(n)$ 。以上推算表明，**輸入串列並建堆積的時間複雜度為 $O(n)$ ，非常高效**。

## Top-k Elements
在寫程式或是在 LeetCode、HackerRank 等平台刷題的時候，你可能會遇到類似於以下描述的問題：

> 給定一個長度為 `n` 的無序陣列 `nums` ，請返回陣列中最大的 `k` 個元素。

對於該問題，我們先介紹兩種思路比較直接的解法，再介紹效率更高的堆積解法。
### By iterative selection
進行 $k$ 輪走訪，分別在每輪中提取第 $1$、$2$、$\cdots$、$k$ 大的元素：
	實作方面，我們可以分別在每輪中提取最大的元素，將其從原先的陣列中搬移到另一個陣列中。
```md
n = 5
k = 3
	  #begin                                 #end
nums: [1,7,6,3,2]->[1,6,3,2]  ->[1,3,2]    ->[1,2]      
ans:  []         ->[7]        ->[6,7]      ->[3,6,7]    
```

該方法的時間複雜度為 $O(nk)$ 。此方法只適用於 $k≪n$ 的情況，因為當 $k$ 與 $n$ 比較接近時，其時間複雜度趨向於 $O(n^2)$ ，非常耗時。

> [!NOTE]
>  當 $k=n$ 時，我們可以得到完整的有序序列，此時等價於「[[Sorting#Selection Sort|選擇排序]]」演算法。

### By sorting
對陣列 `nums` 進行排序，再返回最右邊的 $k$ 個元素，時間複雜度為 $O(n\log ⁡n)$ 。
顯然，該方法「超額」完成任務了，因為我們只需找出最大的 $k$ 個元素即可，而不需要排序其他元素。
```md
n = 5
k = 3
				   #sort 
nums: [1,7,6,3,2]->[1,2,3,6,7]    
ans = nums[n-k:]
```

### By heap
我們可以基於堆積更加高效地解決 Top-k 問題，流程如下：
1. 初始化一個小頂堆積，其堆積頂元素最小。
2. 先將陣列的前 $k$ 個元素依次入堆積。
3. **從第 $k+1$ 個元素開始，若當前元素大於堆積頂元素，則將堆積頂元素出堆積，並將當前元素入堆積。**
4. 走訪完成後，堆積中儲存的就是最大的 $k$ 個元素。

~~~tabs
tab: <1>
```md
k = 3
i = 0
nums: [1,7,6,3,2]
	   ^
	   i
heap: [1]
# visualize heap
	1
```

tab: <2>
```md
k = 3
i = 1
nums: [1,7,6,3,2]
		 ^
		 i
heap: [1,7]
# visualize heap
	1
  /
 7
```

tab: <3>
```md
k = 3
i = 2
nums: [1,7,6,3,2]
		   ^
		   i
heap: [1,7,3]
# visualize heap
    1
  /   \
 7     6
```

tab: <4>
```md
k = 3
i = 3
nums: [1,7,6,3,2]
			 ^
			 i
heap: [1,7,6]
# visualize heap
    1
  /   \
 7     6
```
- **當前元素 $>$ top of the heap**

tab: <5>
```md
k = 3
i = 3
nums: [1,7,6,3,2]
			 ^
			 i
heap: [6,7]
# visualize heap
    6
  /   
 7     
```
- 當前元素 $>$ top of the heap
- **將 top of the heap 移除**

tab: <6>
```md
k = 3
i = 3
nums: [1,7,6,3,2]
			 ^
			 i
heap: [3,7,6]
# visualize heap
    3
  /   \
 7     6
```
- 當前元素 $>$ top of the heap
- 將 top of the heap 移除
- **將當前元素插入 heap**

tab: <7>
```md
k = 3
i = 4
nums: [1,7,6,3,2]
			   ^
			   i
heap: [3,7,6]
# visualize heap
    3
  /   \
 7     6
```
- **當前元素 $≤$ top of the heap**

tab: <8>
```md
k = 3
i = 4
nums: [1,7,6,3,2]
			   ^
			   i
heap: [3,7,6]
# visualize heap
    3
  /   \
 7     6
```
- 當前元素 $≤$ top of the heap
- **不動作**

tab: <9>
```md
heap: [3,7,6]
```
- 返回 `heap`
~~~

此方法總共執行了 $n$ 輪入堆積和出堆積，堆積的最大長度為 $k$ ，因此時間複雜度為 $O(n \log⁡k)$ 。該方法的效率很高，當 $k$ 較小時，時間複雜度趨向 $O(n)$ ；當 $k$ 較大時，時間複雜度不會超過 $O(n\log ⁡n)$ 。
另外，該方法適用於動態資料流的使用場景。在不斷加入資料時，我們可以持續維護堆積內的元素，從而實現最大的 $k$ 個元素的動態更新。
#### Example code
~~~tabs
tab: C
```c
/* 元素入堆積 */
void pushMinHeap(MaxHeap *maxHeap, int val) {
    // 元素取反
    push(maxHeap, -val);
}

/* 元素出堆積 */
int popMinHeap(MaxHeap *maxHeap) {
    // 元素取反
    return -pop(maxHeap);
}

/* 訪問堆積頂元素 */
int peekMinHeap(MaxHeap *maxHeap) {
    // 元素取反
    return -peek(maxHeap);
}

/* 取出堆積中元素 */
int *getMinHeap(MaxHeap *maxHeap) {
    // 將堆積中所有元素取反並存入 res 陣列
    int *res = (int *)malloc(maxHeap->size * sizeof(int));
    for (int i = 0; i < maxHeap->size; i++) {
        res[i] = -maxHeap->data[i];
    }
    return res;
}

/* 取出堆積中元素 */
int *getMinHeap(MaxHeap *maxHeap) {
    // 將堆積中所有元素取反並存入 res 陣列
    int *res = (int *)malloc(maxHeap->size * sizeof(int));
    for (int i = 0; i < maxHeap->size; i++) {
        res[i] = -maxHeap->data[i];
    }
    return res;
}

// 基於堆積查詢陣列中最大的 k 個元素的函式
int *topKHeap(int *nums, int sizeNums, int k) {
    // 初始化小頂堆積
    // 請注意：我們將堆積中所有元素取反，從而用大頂堆積來模擬小頂堆積
    int *empty = (int *)malloc(0);
    MaxHeap *maxHeap = newMaxHeap(empty, 0);
    // 將陣列的前 k 個元素入堆積
    for (int i = 0; i < k; i++) {
        pushMinHeap(maxHeap, nums[i]);
    }
    // 從第 k+1 個元素開始，保持堆積的長度為 k
    for (int i = k; i < sizeNums; i++) {
        // 若當前元素大於堆積頂元素，則將堆積頂元素出堆積、當前元素入堆積
        if (nums[i] > peekMinHeap(maxHeap)) {
            popMinHeap(maxHeap);
            pushMinHeap(maxHeap, nums[i]);
        }
    }
    int *res = getMinHeap(maxHeap);
    // 釋放記憶體
    delMaxHeap(maxHeap);
    return res;
}
```

tab: C++
```cpp
/* 基於堆積查詢陣列中最大的 k 個元素 */
priority_queue<int, vector<int>, greater<int>> topKHeap(vector<int> &nums, int k) {
    // 初始化小頂堆積
    priority_queue<int, vector<int>, greater<int>> heap;
    // 將陣列的前 k 個元素入堆積
    for (int i = 0; i < k; i++) {
        heap.push(nums[i]);
    }
    // 從第 k+1 個元素開始，保持堆積的長度為 k
    for (int i = k; i < nums.size(); i++) {
        // 若當前元素大於堆積頂元素，則將堆積頂元素出堆積、當前元素入堆積
        if (nums[i] > heap.top()) {
            heap.pop();
            heap.push(nums[i]);
        }
    }
    return heap;
}
```

tab: Python
```python
def top_k_heap(nums: list[int], k: int) -> list[int]:
    """基於堆積查詢陣列中最大的 k 個元素"""
    # 初始化小頂堆積
    heap = []
    # 將陣列的前 k 個元素入堆積
    for i in range(k):
        heapq.heappush(heap, nums[i])
    # 從第 k+1 個元素開始，保持堆積的長度為 k
    for i in range(k, len(nums)):
        # 若當前元素大於堆積頂元素，則將堆積頂元素出堆積、當前元素入堆積
        if nums[i] > heap[0]:
            heapq.heappop(heap)
            heapq.heappush(heap, nums[i])
    return heap
```
~~~

## Q & A
> [!QUESTION]- 資料結構的「堆積」與記憶體管理的「堆積」是同一個概念嗎？
> 兩者不是同一個概念，只是碰巧都叫「堆積」。計算機系統記憶體中的堆積是動態記憶體分配的一部分，程式在執行時可以使用它來儲存資料。程式可以請求一定量的堆積記憶體，用於儲存如物件和陣列等複雜結構。當這些資料不再需要時，程式需要釋放這些記憶體，以防止記憶體流失。相較於堆疊記憶體，堆積記憶體的管理和使用需要更謹慎，使用不當可能會導致記憶體流失和野指標等問題。
