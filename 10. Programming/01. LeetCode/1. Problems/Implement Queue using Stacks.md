#Easy 
## Problem Description
Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:
- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**
- You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

### Examples
#### 1
> **Input:** `["MyQueue", "push", "push", "peek", "pop", "empty"]`
> `[[], [1], [2], [], [], []]`
> **Output:** `[null, null, null, 1, 1, false]`
> **Explanation:** 
> `MyQueue myQueue = new MyQueue();`
> `myQueue.push(1); // queue is: [1]`
> `myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)`
> `myQueue.peek(); // return 1`
> `myQueue.pop(); // return 1, queue is [2]`
> `myQueue.empty(); // return false`


### Constraints
- `1 <= x <= 9`
- At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
- All the calls to `pop` and `peek` are valid.

### Follow-up
**Follow-up:** Can you implement the queue such that each operation is **[amortized](https://en.wikipedia.org/wiki/Amortized_analysis)** `O(1)` time complexity? In other words, performing `n` operations will take overall `O(n)` time even if one of those operations may take longer.

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - 

---
## 我的答案
### Attempt 1
~~~~tabs

tab: Python
```python
class MyQueue(object):
    def __init__(self):
        self.mainStack = []
        self.tempStack = []

    def push(self, x):
        self.mainStack.append(x)

    def pop(self):
        self.peek()
        return self.tempStack.pop()

    def peek(self):
        if not self.tempStack:
            while self.mainStack:
                self.tempStack.append(self.mainStack.pop())
        return self.tempStack[-1]

    def empty(self):
        return not self.mainStack and not self.tempStack
        
# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```
~~~~

#### Result
Accepted
- Runtime: $0$ ms, beats 100.00%
- Memory: 12.52 MB, beats 19.53%


---
## 解題思路
- **只能使用 “Stack”（堆疊） 來模擬 “Queue”（佇列） 的行為。**
	- 佇列（Queue）遵循 **FIFO（First-In-First-Out）** 原則。
	- 堆疊（Stack）遵循 **LIFO（Last-In-First-Out）** 原則。

由於 Stack 和 Queue 的行為不同，我們需要巧妙地設計方法來讓 Stack 模擬 Queue 的行為。

### Using Two Stacks
算是本題較為經典的解法。
使用 **兩個 Stack**（`stack_in` 和 `stack_out`）來實現 Queue：
1. `stack_in`（輸入堆疊）
	負責 **push(x) 操作**，新元素進來時都進入這個堆疊。
2. `stack_out`（輸出堆疊）
	負責 **pop() 和 peek() 操作**，確保 FIFO 順序。

