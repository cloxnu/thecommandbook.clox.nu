---
title: 链表
---

# 链表

[链表](https://zh.wikipedia.org/wiki/%E9%93%BE%E8%A1%A8) (Linked list) 是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针 (Pointer)。链表的程序定义如下：

```python
class ListNode:
    def __init__(self, x, next=None):
        self.val = x
        self.next = next
```

## 1. 环形链表

检测链表中的环

[141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```
1 -> 2 -> 3 -> 4
|         |
7 <- 6 <- 5
```

快慢指针，相撞即有环

```python
def hasCycle(head: ListNode) -> bool:
    slow = fast = head
    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next
        if fast == slow:
            return True
    return False
```

## 2. 环形链表 II

检测链表中的环，并输出入环的第一个结点，无环返回 None

[142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

先快慢指针检测是否有环，然后将 ptr 放到 head，同时开始 ptr 和 slow，相撞的点即为入环点

证明：设入环点前长度为 {{< katex >}}a{{< /katex >}}，slow 走过的路程为 {{< katex >}}a + b{{< /katex >}}，fast 走过的路程为 {{< katex >}}a + n(b + c) + b{{< /katex >}}（{{< katex >}}n{{< /katex >}} 为圈数，{{< katex >}}b + c{{< /katex >}} 为环长），fast 的路程一定是 slow 的两倍，所以 {{< katex >}}a + n(b + c) + b = 2a + 2b{{< /katex >}}，所以 {{< katex >}}a = (n - 1)(b + c) + c{{< /katex >}}

```python
def detectCycle(head: ListNode) -> ListNode or None:
    slow = fast = head
    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next
        if fast == slow:
            ptr = head
            while ptr != slow:
                slow = slow.next
                ptr = ptr.next
            return ptr
    return None
```

## 3. 相交链表

找到两个单链表相交的起始结点

[160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```
1 -> 2 -> 3
          |
4 -> 5 -> 6 -> 7
```

双指针，相遇，时间 {{< katex >}}O(n){{< /katex >}} 空间 {{< katex >}}O(1){{< /katex >}}

```python
def getIntersectionNode(headA: ListNode, headB: ListNode) -> ListNode or None:
    nodeA, nodeB = headA, headB
    while nodeA != nodeB:
        nodeA = nodeA.next if nodeA else headB
        nodeB = nodeB.next if nodeB else headA
    return nodeA
```

## 4. 链表的中间结点

找出单链表的中间结点

[876. 链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

```
1 -> 2 -> 3 -> 4 -> 5
          |
```

快慢指针，快的到链表尾，慢的即在中点

```python
def middleNode(head: ListNode) -> ListNode:
    slow = fast = head
    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next
    return slow
```

## 5. 反转链表

反转一个单链表

[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

双指针

```python
def reverseList(head: ListNode) -> ListNode:
    pre, curr = None, head
    while curr:
        curr.next, pre, curr = pre, curr, curr.next
    return pre
```

## 6. K 个一组反转链表

困难；将一个链表每 k 个结点一组进行反转，返回反转后的链表

```
input: 1 -> 2 -> 3 -> 4 -> 5;  k = 2
output: 2 -> 1 -> 4 -> 3 -> 5
```

时间 {{< katex >}}O(n){{< /katex >}} 空间 {{< katex >}}O(1){{< /katex >}}

```python
def reverseKGroup(head: ListNode, k: int) -> ListNode:
    def reverse(one_head: ListNode, one_tail: ListNode) -> (ListNode, ListNode):
        pre, curr = None, one_head
        while pre != one_tail:
            curr.next, pre, curr = pre, curr, curr.next
        return one_tail, one_head

    pre = dummy = ListNode(0, next=head)
    curr = head
    while curr:
        for _ in range(k - 1):
            curr = curr.next
            if curr is None:
                return dummy.next
        one_head, one_tail, curr = pre.next, curr, curr.next  # 确定下一步要 reverse 的头尾
        one_head, one_tail = reverse(one_head, one_tail)  # reverse
        pre.next, one_tail.next = one_head, curr  # 连接
        pre = one_tail  # 把 pre 移动到下一次的位置
    return dummy.next
```

## 7. 合并两个有序链表

```
input: 1 -> 2 -> 4; 1 -> 3 -> 4
output: 1 -> 1 -> 2 -> 3 -> 4 -> 4
```

```python
def mergeTwoLists(l1: ListNode, l2: ListNode) -> ListNode:
    n1 = dummy = ListNode(0, next=l1)
    n2 = l2
    while n1.next and n2:
        if n1.next.val >= n2.val:
            n1.next, n2 = n2, n1.next
        n1 = n1.next
    if n2:
        n1.next = n2
    return dummy.next
```

## 8. 合并 K 个有序链表

```
input: 1 -> 4 -> 5; 1 -> 3 -> 4; 2 -> 6
output: 1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6
```

```python
def mergeKLists(lists: list) -> ListNode:
    def merge2Lists(l1: ListNode, l2: ListNode) -> ListNode:
        n1 = dummy = ListNode(0, next=l1)
        n2 = l2
        while n1.next and n2:
            if n1.next.val >= n2.val:
                n1.next, n2 = n2, n1.next
            n1 = n1.next
        if n2:
            n1.next = n2
        return dummy.next

    def recur(ls: list) -> ListNode:
        if len(ls) == 0:
            return None
        if len(ls) == 1:
            return ls[0]
        mid = len(ls) // 2
        return merge2Lists(recur(ls[:mid]), recur(ls[mid:]))

    return recur(lists)
```

