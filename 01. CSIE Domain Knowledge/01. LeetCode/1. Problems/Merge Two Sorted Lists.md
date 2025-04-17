#Easy 
## Problem Description
You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return _the head of the merged linked list_.

### Examples
#### 1
> **Input:** `list1 = [1,2,4], list2 = [1,3,4]`
> **Output:** `[1,1,2,3,4,4]`
#### 2
> **Input:** `list1 = [], list2 = []`
> **Output:** `[]`
#### 3
> **Input:** nums = `list1 = [], list2 = [0]`
> **Output:** `[0]`

### Constraints
- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.

### Follow-up
None

## Hints
No hints for this problem.

## Topics
> [!WARNING]- 查看相關主題可能會影響你的思考、思路。建議先構思好一種解法之後再點開來查看。
> - [[Linked List]]
> - [[Recursion]]

---
## 我的答案
### Attempt 1
~~~tabs
tab: Python
```python
def mergeTwoLists(self, list1, list2):
    """
    :type list1: Optional[ListNode]
    :type list2: Optional[ListNode]
    :rtype: Optional[ListNode]
    """
    # dummy is an in-place list merging operator
    dummy = ListNode(val=-1,next=None)
    # curr tracks megring process
    curr = dummy
	### main pattern
    # compare the values of heads of two lists
    while list1 is not None and list2 is not None:
        if list1.val <= list2.val:
            curr.next = list1
            list1 = list1.next
        else:
            curr.next = list2
            list2 = list2.next

        curr = curr.next

    # if one of the list is empty
    # concat the other to the main list
    if list1 is None:
        curr.next = list2
    else:
        curr.next = list1
	### end main pattern
	
    return dummy.next
```
~~~

#### Result
Accepted
- Runtime: $0$ ms, beats 100.00%
- Memory: $12.55$ MB, beats 46.56%

---
## 解題思路
### In-place iterative merge
即 [[#Attempt 1]] 所使用的思路。
使用兩個額外指標：`dummy` 與 `current`。`dummy` 用於建立新鏈結串列，而 `current` 則是追蹤目前合併鏈結串列的進度。
合併時，比較兩個子鏈結串列的 `head.value` （數值大小），將數值較小的優先合併到新鏈結串列中。而當其中一個子鏈結串列為空時，只需要將剩下的另一個子鏈結串列直接合併至新鏈結串列即可。
- 時間複雜度：$O(n+m)$
- 空煎複雜度：$O(1)$
