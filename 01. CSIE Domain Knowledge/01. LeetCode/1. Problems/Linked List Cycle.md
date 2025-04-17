#Easy 
## Problem Description
Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

### Examples
#### 1
![400px](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)
> **Input:** `head = [3,2,0,-4] (pos = 1)`
> **Output:** `true`
> **Explanation:** `There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).`
#### 2
![200px](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)
> **Input:** `head = [1,2], pos = 0`
> **Output:** `true`
> **Explanation:** `There is a cycle in the linked list, where the tail connects to the 0th node.`
#### 3
> **Input:** `head = [1], pos = -1`
> **Output:** `false`
> **Explanation:** `There is no cycle in the linked list.`

### Constraints
- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

### Follow-up
Can you solve it using `O(1)` (i.e. constant) memory?

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - 

---
## 我的答案
### Attempt 1
~~~tabs
tab: Python
```python
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        slow = head
        fast = head
        if head is None: return False


        while (fast is not None and fast.next is not None):
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        
        return False
```
~~~

#### Result
Accepted
- Runtime: $43$ ms, beats <font color="#c0504d">25.01%</font>
- Memory: 19.44 MB, beats <font color="#9bbb59">84.80%</font>

---
## 解題思路

### [[Two Pointers]]
使用快慢指標遍歷鏈結串列，直到兩個指標重疊或是快指標指向 `null`。