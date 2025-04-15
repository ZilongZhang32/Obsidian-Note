## Introduction
堆疊（Stack）是一種遵循先入後出邏輯的線性資料結構。

我們可以將堆疊類比為桌面上的一疊盤子，如果想取出底部的盤子，則需要先將上面的盤子依次移走。我們將盤子替換為各種型別的元素（如整數、字元、物件等），就得到了堆疊這種資料結構。

我們把堆積疊元素的頂部稱為「堆疊頂（Top of the stack）」，底部稱為「堆疊底（Bottom of the stack）」。將把元素新增到堆疊頂的操作叫作「入堆疊（push）」，刪除堆疊頂元素的操作叫作「出堆疊（pop）」。

## Common Operations
> [!CAUTION] 由於 C 未內建堆疊，以下常見操作不提供 C 的程式碼。
### Initializing
~~~~tabs

tab: C++
```cpp
stack<int> stack;
```
tab: Python
```python
stack: list[int] = []
```
~~~~

以下三個操作的時間複雜度皆為$O(1)$。通常情況下，我們可以直接使用程式語言內建的堆疊類別。然而，某些語言可能沒有專門提供堆疊類別，這時我們可以將該語言的「陣列」或「鏈結串列」當作堆疊來使用，並在程式邏輯上忽略與堆疊無關的操作。
### Push an element
~~~~tabs

tab: C++
```cpp
stack.push(1);
stack.push(3);
stack.push(2);
stack.push(5);
stack.push(4);
```
tab: Python
```python
stack.append(1)
stack.append(3)
stack.append(2)
stack.append(5)
stack.append(4)
```
~~~~

### Pop the top element
~~~~tabs

tab: C++
```cpp
stack.pop(); // C++ 的 pop() 無回傳值
```
tab: Python
```python
pop: int = stack.pop()
```
~~~~

### Access the top element
~~~~tabs

tab: C++
```cpp
int top = stack.top();
```
tab: Python
```python
peek: int = stack[-1]
```
~~~~

## Comparison of the Two Implementations
### Array based
~~~tabs
tab: C++
```cpp
/* 基於鏈結串列實現的堆疊 */
class LinkedListStack {
  private:
    ListNode *stackTop; // 將頭節點作為堆疊頂
    int stkSize;        // 堆疊的長度

  public:
    LinkedListStack() {
        stackTop = nullptr;
        stkSize = 0;
    }

    ~LinkedListStack() {
        // 走訪鏈結串列刪除節點，釋放記憶體
        freeMemoryLinkedList(stackTop);
    }

    /* 獲取堆疊的長度 */
    int size() {
        return stkSize;
    }

    /* 判斷堆疊是否為空 */
    bool isEmpty() {
        return size() == 0;
    }

    /* 入堆疊 */
    void push(int num) {
        ListNode *node = new ListNode(num);
        node->next = stackTop;
        stackTop = node;
        stkSize++;
    }

    /* 出堆疊 */
    int pop() {
        int num = top();
        ListNode *tmp = stackTop;
        stackTop = stackTop->next;
        // 釋放記憶體
        delete tmp;
        stkSize--;
        return num;
    }

    /* 訪問堆疊頂元素 */
    int top() {
        if (isEmpty())
            throw out_of_range("堆疊為空");
        return stackTop->val;
    }

    /* 將 List 轉化為 Array 並返回 */
    vector<int> toVector() {
        ListNode *node = stackTop;
        vector<int> res(size());
        for (int i = res.size() - 1; i >= 0; i--) {
            res[i] = node->val;
            node = node->next;
        }
        return res;
    }
};
```

tab: Python
```python
class LinkedListStack:
    """基於鏈結串列實現的堆疊"""

    def __init__(self):
        """建構子"""
        self._peek: ListNode | None = None
        self._size: int = 0

    def size(self) -> int:
        """獲取堆疊的長度"""
        return self._size

    def is_empty(self) -> bool:
        """判斷堆疊是否為空"""
        return self._size == 0

    def push(self, val: int):
        """入堆疊"""
        node = ListNode(val)
        node.next = self._peek
        self._peek = node
        self._size += 1

    def pop(self) -> int:
        """出堆疊"""
        num = self.peek()
        self._peek = self._peek.next
        self._size -= 1
        return num

    def peek(self) -> int:
        """訪問堆疊頂元素"""
        if self.is_empty():
            raise IndexError("堆疊為空")
        return self._peek.val

    def to_list(self) -> list[int]:
        """轉化為串列用於列印"""
        arr = []
        node = self._peek
        while node:
            arr.append(node.val)
            node = node.next
        arr.reverse()
        return arr
```
~~~

|      | Array based | Linked List based  |
| ---- | ----------- | :----------------- |
| 支持操作 | 完全支持        | 完全支持               |
| 時間效率 | 平均效率更高      | 可以提供更加穩定的效率表現      |
| 空間效率 | 可能造成一定的空間浪費 | 需要額外儲存指標，佔用的空間相對較大 |

### Supported operations
兩種實現都支持堆疊定義中的各項操作。陣列實現額外支持隨機訪問，但這已超出了堆疊的定義範疇，因此一般不會用到。

### Time efficiency
在基於陣列的實現中，入堆疊和出堆疊操作都在預先分配好的連續記憶體中進行，具有很好的快取本地性，因此效率較高。然而，如果入堆疊時超出陣列容量，會觸發擴容機制，導致該次入堆疊操作的時間複雜度變為 $O(n)$ 。

在基於鏈結串列的實現中，鏈結串列的擴容非常靈活，不存在上述陣列擴容時效率降低的問題。但是，入堆疊操作需要初始化節點物件並修改指標，因此效率相對較低。不過，如果入堆疊元素本身就是節點物件，那麼可以省去初始化步驟，從而提高效率。

綜上所述，當入堆疊與出堆疊操作的元素是基本資料型別時，例如 `int` 或 `double` ，我們可以得出以下結論：
- 基於陣列實現的堆疊在觸發擴容時效率會降低，但由於擴容是低頻操作，因此平均效率更高。
- 基於鏈結串列實現的堆疊可以提供更加穩定的效率表現。

### Space efficiency
在初始化串列時，系統會為串列分配“初始容量”，該容量可能超出實際需求；並且，擴容機制通常是按照特定倍率（例如 2 倍）進行擴容的，擴容後的容量也可能超出實際需求。因此，**基於陣列實現的堆疊可能造成一定的空間浪費**。然而，由於鏈結串列節點需要額外儲存指標，**因此鏈結串列節點佔用的空間相對較大**。

綜上，我們不能簡單地確定哪種實現更加節省記憶體，需要針對具體情況進行分析。

## Typical Applications
1. **瀏覽器中的後退與前進、軟體中的撤銷與反撤銷**
	每當我們開啟新的網頁，瀏覽器就會對上一個網頁執行入堆疊，這樣我們就可以通過後退操作回到上一個網頁。後退操作實際上是在執行出堆疊。如果要同時支持後退和前進，那麼需要兩個堆疊來配合實現。
2. **程式記憶體管理**
	每次呼叫函式時，系統都會在堆疊頂新增一個堆疊幀，用於記錄函式的上下文資訊。在遞迴函式中，向下遞推階段會不斷執行入堆疊操作，而向上回溯階段則會不斷執行出堆疊操作。