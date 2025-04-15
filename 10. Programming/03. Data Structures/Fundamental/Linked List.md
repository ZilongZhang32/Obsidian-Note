## Introduction
鏈結串列是一種線性資料結構，其中的每個元素都是一個節點物件，各個節點透過「引用」相連線。引用記錄了下一個節點的記憶體位址，透過它可以從當前節點訪問到下一個節點。

記憶體空間是所有程式的公共資源，在一個複雜的系統執行環境下，空閒的記憶體空間可能散落在記憶體各處。我們知道，儲存陣列的記憶體空間必須是連續的，而當陣列非常大時，記憶體可能無法提供如此大的連續空間。此時鏈結串列（Linked List）的靈活性優勢就體現出來了。

鏈結串列的設計使得各個節點可以分散儲存在記憶體各處，它們的記憶體位址無須連續。

## Common Operations
### Initializing
建立鏈結串列分為兩步，第一步是初始化各個節點物件，第二步是構建節點之間的引用關係。初始化完成後，我們就可以從鏈結串列的頭節點出發，透過引用指向 `next` 依次訪問所有節點。
~~~~tabs

tab: C
```c
/* 初始化鏈結串列 1 -> 3 -> 2 -> 5 -> 4 */
// 初始化各個節點
ListNode* n0 = newListNode(1);
ListNode* n1 = newListNode(3);
ListNode* n2 = newListNode(2);
ListNode* n3 = newListNode(5);
ListNode* n4 = newListNode(4);
// 構建節點之間的引用
n0->next = n1;
n1->next = n2;
n2->next = n3;
n3->next = n4;
```
tab: C++
```cpp
/* 初始化鏈結串列 1 -> 3 -> 2 -> 5 -> 4 */
// 初始化各個節點
ListNode* n0 = new ListNode(1);
ListNode* n1 = new ListNode(3);
ListNode* n2 = new ListNode(2);
ListNode* n3 = new ListNode(5);
ListNode* n4 = new ListNode(4);
// 構建節點之間的引用
n0->next = n1;
n1->next = n2;
n2->next = n3;
n3->next = n4;
```
tab: Python
```python
# 初始化鏈結串列 1 -> 3 -> 2 -> 5 -> 4
# 初始化各個節點
n0 = ListNode(1)
n1 = ListNode(3)
n2 = ListNode(2)
n3 = ListNode(5)
n4 = ListNode(4)
# 構建節點之間的引用
n0.next = n1
n1.next = n2
n2.next = n3
n3.next = n4
```
~~~~
> [!NOTE] 
> 陣列整體是一個變數，比如陣列 `nums` 包含元素 `nums[0]` 和 `nums[1]` 等，而鏈結串列是由多個獨立的節點物件組成的。**我們通常將頭節點當作鏈結串列的代稱**，比如以上程式碼中的鏈結串列可記作鏈結串列 `n0` 。

### Inserting nodes
在鏈結串列中插入節點非常容易。假設我們想在相鄰的兩個節點 `n0` 和 `n1` 之間插入一個新節點 `P` ，**則只需改變兩個節點引用（指標）即可**，時間複雜度為 $O(1)$ 。

相比之下，在陣列中插入元素的時間複雜度為 $O(n)$ ，在資料量較大的情形下的效率較低。
~~~~tabs

tab: Pseudo Code
```md
假設節點`P`要插入在節點`n0->n1`之間，則
1. 先確認`n0`是否有指向`n1`
2. 令`P`指向`n1`
3. 令`n0`指向P
完成
```
tab: C
```c
/* 在鏈結串列的節點 n0 之後插入節點 P */
void insert(ListNode *n0, ListNode *P) {
    ListNode *n1 = n0->next;
    P->next = n1;
    n0->next = P;
}
```
tab: C++
```cpp
/* 在鏈結串列的節點 n0 之後插入節點 P */
void insert(ListNode *n0, ListNode *P) {
    ListNode *n1 = n0->next;
    P->next = n1;
    n0->next = P;
}
```
tab: Python
```python
def insert(n0: ListNode, P: ListNode):
    """在鏈結串列的節點 n0 之後插入節點 P"""
    n1 = n0.next
    P.next = n1
    n0.next = P
```
~~~~

### Removing nodes
刪除節點也非常方便，**只需改變一個節點的引用（指標）即可**。

請注意，儘管在刪除操作完成後節點 `P` 仍然指向 `n1` ，但實際上走訪此鏈結串列已經無法訪問到 `P` ，這意味著 `P` 已經不再屬於該鏈結串列了。
~~~~tabs

tab: Pseudo Code
```md
1. 先找出節點n0使得`n0->P`
2. 令`n0`指向`P`指向的節點`n1`
完成
```
tab: C
```c
/* 刪除鏈結串列的節點 n0 之後的首個節點 */
// 注意：stdio.h 佔用了 remove 關鍵詞
void removeItem(ListNode *n0) {
    if (!n0->next)
        return;
    // n0 -> P -> n1
    ListNode *P = n0->next;
    ListNode *n1 = P->next;
    n0->next = n1;
    // 釋放記憶體
    free(P);
}
```
tab: C++
```cpp
/* 刪除鏈結串列的節點 n0 之後的首個節點 */
void remove(ListNode *n0) {
    if (n0->next == nullptr)
        return;
    // n0 -> P -> n1
    ListNode *P = n0->next;
    ListNode *n1 = P->next;
    n0->next = n1;
    // 釋放記憶體
    delete P;
}
```
tab: Python
```python
def remove(n0: ListNode):
    """刪除鏈結串列的節點 n0 之後的首個節點"""
    if not n0.next:
        return
    # n0 -> P -> n1
    P = n0.next
    n1 = P.next
    n0.next = n1
```
~~~~

### Accessing nodes
**在鏈結串列中訪問節點的效率較低**。如上一節所述，我們可以在 $O(1)$ 時間下訪問陣列中的任意元素。鏈結串列則不然，程式需要從頭節點出發，逐個向後走訪，直至找到目標節點。也就是說，訪問鏈結串列的第 $i$ 個節點需要迴圈 $i−1$ 輪，時間複雜度為 $O(n)$ 。
~~~tabs
tab: C
```c
/* 訪問鏈結串列中索引為 index 的節點 */
ListNode *access(ListNode *head, int index) {
    for (int i = 0; i < index; i++) {
        if (head == NULL)
            return NULL;
        head = head->next;
    }
    return head;
}
```

tab: C++
```cpp
/* 訪問鏈結串列中索引為 index 的節點 */
ListNode *access(ListNode *head, int index) {
    for (int i = 0; i < index; i++) {
        if (head == nullptr)
            return nullptr;
        head = head->next;
    }
    return head;
}
```

tab: Python
```python
def access(head: ListNode, index: int) -> ListNode | None:
    """訪問鏈結串列中索引為 index 的節點"""
    for _ in range(index):
        if not head:
            return None
        head = head.next
    return head
```
~~~

### Finding nodes
走訪鏈結串列，查詢其中值為 `target` 的節點，輸出該節點在鏈結串列中的**索引**。此過程也屬於線性查詢。
~~~tabs
tab: C
```c
/* 在鏈結串列中查詢值為 target 的首個節點 */
int find(ListNode *head, int target) {
    int index = 0;
    while (head) {
        if (head->val == target)
            return index;
        head = head->next;
        index++;
    }
    return -1;
}
```

tab: C++
```cpp
/* 在鏈結串列中查詢值為 target 的首個節點 */
int find(ListNode *head, int target) {
    int index = 0;
    while (head != nullptr) {
        if (head->val == target)
            return index;
        head = head->next;
        index++;
    }
    return -1;
}
```

tab: Python
```python
def find(head: ListNode, target: int) -> int:
    """在鏈結串列中查詢值為 target 的首個節點"""
    index = 0
    while head:
        if head.val == target:
            return index
        head = head.next
        index += 1
    return -1
```
~~~

### Reversing a linked list
顧名思義，就是將整的鏈結串列以反序重新排列。注意：只有在非環形鏈結串列才有可能會使用到此操作。對於鏈結串列的種類，我們將在後續小節說明。
~~~tabs
tab: C++
```cpp
ListNode* fn(ListNode* head) {
    ListNode* curr = head;
    ListNode* prev = nullptr;
    while (curr != nullptr) {
        ListNode* nextNode = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextNode;
    }

    return prev;
}
```

tab: Python
```python
def fn(head):
    curr = head
    prev = None
    while curr:
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node 
        
    return prev
```
~~~

## Comparison

|       |               陣列               |             鏈結串列             |
| ----- | :----------------------------: | :--------------------------: |
| 儲存方式  |            連續記憶體空間             |           非連續記憶體空間           |
| 容量擴展  | 長度不可變<br>（需另外使用新的陣列並將資料複製至新陣列） | 可靈活拓展大小<br>（插入/刪除節點只需更改指標指向） |
| 記憶體效率 |         佔用較少（但可能浪費空間）          |        佔用較多（需額外儲存指標）         |
| 訪問元素  |     $\color{#B9E06A}O(1)$      |            $O(n)$            |
| 新增元素  |             $O(n)$             |    $\color{#B9E06A}O(1)$     |
| 刪除元素  |             $O(n)$             |    $\color{#B9E06A}O(1)$     |

## Types of Linked List
常見的鏈結串列型別包括三種。
### Singly Linked List
即前面介紹的普通鏈結串列。單向鏈結串列的節點包含值和指向下一節點的引用兩項資料。我們將首個節點稱為頭節點，將最後一個節點稱為尾節點，尾節點指向空 `None` 。
### Cicular Linked List
如果我們令單向鏈結串列的尾節點指向頭節點（首尾相接），則得到一個環形鏈結串列。在環形鏈結串列中，任意節點都可以視作頭節點。
### Doubly Linked List
與單向鏈結串列相比，雙向鏈結串列記錄了兩個方向的引用。雙向鏈結串列的節點定義同時包含指向後繼節點（下一個節點）和前驅節點（上一個節點）的引用（指標）。相較於單向鏈結串列，雙向鏈結串列更具靈活性，可以朝兩個方向走訪鏈結串列，但相應地也需要佔用更多的記憶體空間。
~~~tabs
tab: C
```c
/* 雙向鏈結串列節點結構體 */
typedef struct ListNode {
    int val;               // 節點值
    struct ListNode *next; // 指向後繼節點的指標
    struct ListNode *prev; // 指向前驅節點的指標
} ListNode;

/* 建構子 */
ListNode *newListNode(int val) {
    ListNode *node;
    node = (ListNode *) malloc(sizeof(ListNode));
    node->val = val;
    node->next = NULL;
    node->prev = NULL;
    return node;
}
```

tab: C++
```cpp
/* 雙向鏈結串列節點結構體 */
struct ListNode {
    int val;         // 節點值
    ListNode *next;  // 指向後繼節點的指標
    ListNode *prev;  // 指向前驅節點的指標
    ListNode(int x) : val(x), next(nullptr), prev(nullptr) {}  // 建構子
};
```

tab: Python
```python
class ListNode:
    """雙向鏈結串列節點類別"""
    def __init__(self, val: int):
        self.val: int = val                # 節點值
        self.next: ListNode | None = None  # 指向後繼節點的引用
        self.prev: ListNode | None = None  # 指向前驅節點的引用
```
~~~

> [!QUESTION] 思考一下
> 1. 在環形鏈結串列與雙向鏈結串列中如何插入節點與刪除節點？
> 2. 如果執行上部動作時牽涉到 `head` 或是 `tail` 時要如何處理？

## Dummy Node (Sentinel Node)
啞節點（前哨節點）是一個虛擬節點，不儲存任何資料。在單向鏈結串列中，是用來指向 `head` 的節點，以避免在操作時因為某些意外丟失 `head` 導致丟失整個鏈結串列；在雙向鏈結串列中，Dummy Node 則是分別指向 `head` 與 `tail` 。
Dummy Node的另一種主要用途則是快速的更改 `head` 與 `tail`，相較一般方法可省去不少步驟。

## Fast & Slow Pointers
一種使用 [[Two Pointers]] 的技巧，快慢指標中的「快慢」指的是指標向前移動的步長，每次移動的步長較大即為快，步長較小即為慢，常用的快慢指標一般是在單向鏈表中讓快指標每次向前移動$2$，慢指標則每次向前移動$1$。
~~~tabs
tab: C++
```cpp
int fn(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    int ans = 0;

    while (fast != nullptr && fast->next != nullptr) {
        // do logic
        slow = slow->next;
        fast = fast->next->next;
    }

    return ans;
}
```

tab: Python
```python
def fn(head):
    slow = head
    fast = head
    ans = 0

    while fast and fast.next:
        # do logic
        slow = slow.next
        fast = fast.next.next
    
    return ans
```
~~~

快慢指標在鏈結串列中的主要用途有以下幾點：
1. 快速找出未知長度單向鏈表的中間節點
	置兩個指標 `fast`、`slow` 都指向單向鏈表的 `head` 節點，其中 `fast` 的步長（移動速度）是 `slow` 的$2$倍，當 `fast` 指向 `tail` 節點的時候，`slow` 正好就在中間了。此方法可以有效避免多次尋訪。
2. 判斷單向鏈表是否有環
	利用快慢指標的原理，同樣設置兩個指標 `fast`、`slow` 都指向單向鏈表的頭節點，其中 `fast` 的移動速度是 `slow` 的$2$倍。如果 `fast = NULL`，說明該單向鏈結串列是以 `NULL` 結尾，不是環形；如果 `fast = slow`，則快指標追上慢指標，說明該鏈結串列是環形。

## Typical Applications
### Singly Linked List
單向鏈結串列通常用於實現堆疊、佇列、雜湊表和圖等資料結構。
- **[[Stack]]與[[Queue]]**
	當插入和刪除操作都在鏈結串列的一端進行時，它表現的特性為先進後出，對應堆疊；當插入操作在鏈結串列的一端進行，刪除操作在鏈結串列的另一端進行，它表現的特性為先進先出，對應佇列。
- **[[Hash Table]]**
	鏈式位址是解決雜湊衝突的主流方案之一，在該方案中，所有衝突的元素都會被放到一個鏈結串列中。
- **[[Graph]]**
	鄰接表是表示圖的一種常用方式，其中圖的每個頂點都與一個鏈結串列相關聯，鏈結串列中的每個元素都代表與該頂點相連的其他頂點。
### Circular Linked List
雙向鏈結串列常用於需要快速查詢前一個和後一個元素的場景。
- **高階資料結構**
	比如在紅黑樹、B樹中，我們需要訪問節點的父節點，這可以透過在節點中儲存一個指向父節點的引用來實現，類似於雙向鏈結串列。
- **瀏覽器歷史**
	在網頁瀏覽器中，當用戶點選前進或後退按鈕時，瀏覽器需要知道使用者訪問過的前一個和後一個網頁。雙向鏈結串列的特性使得這種操作變得簡單。
- **LRU 演算法**
	在快取淘汰（LRU）演算法中，我們需要快速找到最近最少使用的資料，以及支持快速新增和刪除節點。這時候使用雙向鏈結串列就非常合適。
### Doubly Linked List
環形鏈結串列常用於需要週期性操作的場景，比如作業系統的資源排程。
- **時間片輪轉排程演算法**
	在作業系統中，時間片輪轉排程演算法是一種常見的 CPU 排程演算法，它需要對一組程序進行迴圈。每個程序被賦予一個時間片，當時間片用完時，CPU 將切換到下一個程序。這種迴圈操作可以透過環形鏈結串列來實現。
- **資料緩衝區**
	在某些資料緩衝區的實現中，也可能會使用環形鏈結串列。比如在音訊、影片播放器中，資料流可能會被分成多個緩衝塊並放入一個環形鏈結串列，以便實現無縫播放。

## Q & A
> [!QUESTION]- 為什麼陣列要求相同型別的元素，而在鏈結串列中卻沒有強調相同型別呢？
> 鏈結串列由節點組成，節點之間透過引用（指標）連線，各個節點可以儲存不同型別的資料，例如 `int`、`double`、`string`、`object` 等。
> 相對地，陣列元素則必須是相同型別的，這樣才能透過計算偏移量來獲取對應元素位置。例如，陣列同時包含 `int` 和 `long` 兩種型別，單個元素分別佔用 4 位元組和 8 位元組 ，此時就不能用以下公式計算偏移量了，因為陣列中包含了兩種「元素長度」。
> `元素記憶體位址 = 陣列記憶體位址（首元素記憶體位址）+ 元素長度 * 元素索引`

> [!QUESTION]- 刪除節點 `P` 後，是否需要把 `P.next` 設為 `None` / `NULL` 呢？
> 不修改 `P.next` 也可以。從該鏈結串列的角度看，從頭節點走訪到尾節點已經不會遇到 `P` 了。這意味著節點 `P` 已經從鏈結串列中刪除了，此時節點 `P` 指向哪裡都不會對該鏈結串列產生影響。
> 從資料結構與演算法（做題）的角度看，不斷開沒有關係，只要保證程式的邏輯是正確的就行。從標準庫的角度看，斷開更加安全、邏輯更加清晰。如果不斷開，假設被刪除節點未被正常回收，那麼它會影響後繼節點的記憶體回收。
