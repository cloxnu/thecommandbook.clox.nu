---
title: 排序
---

# 排序

[排序算法](https://zh.wikipedia.org/wiki/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95) 是一种能将一串资料依照特定排序方式进行排列的一种算法。

## 堆排序

[堆排序](https://zh.wikipedia.org/wiki/%E5%A0%86%E6%8E%92%E5%BA%8F) (Heapsort) 是指利用堆这种数据结构所设计的一种排序算法。堆排序的平均和最差情况时间均为 {{< katex >}}O(n \log n){{< /katex >}}，空间为 {{< katex >}}O(1){{< /katex >}}

{{< tabs "heap-sort" >}}
{{< tab "递归形式" >}}

```python
def heap_sort(nums: list) -> list:
    def adjust(heap: list, start, end):
        left = start * 2 + 1
        right = left + 1
        if left >= end: return
        max_child = right if right < end and heap[left] < heap[right] else left  # 选取最大的子结点
        if heap[start] < heap[max_child]:
            heap[start], heap[max_child] = heap[max_child], heap[start]
            adjust(heap, max_child, end)

    # 从最后一个结点的父结点开始建堆
    for i in reversed(range(len(nums) // 2)):
        adjust(nums, i, len(nums))
    # 将最大结点交换至最后并调整堆
    for i in reversed(range(len(nums))):
        nums[0], nums[i] = nums[i], nums[0]
        adjust(nums, 0, i)
    return nums
```

{{< /tab >}}
{{< tab "迭代形式" >}}

```python
def heap_sort_iter(nums: list) -> list:
    def adjust(heap: list, start, end):
        while start < end:
            left = start * 2 + 1
            right = left + 1
            if left >= end: break
            max_child = right if right < end and heap[left] < heap[right] else left  # 选取最大的子结点
            if heap[start] >= heap[max_child]: break
            heap[start], heap[max_child] = heap[max_child], heap[start]
            start = max_child

    # 从最后一个结点的父结点开始建堆
    for i in reversed(range(len(nums) // 2)):
        adjust(nums, i, len(nums))
    # 将最大结点交换至最后并调整堆
    for i in reversed(range(len(nums))):
        nums[0], nums[i] = nums[i], nums[0]
        adjust(nums, 0, i)
    return nums
```

{{< /tab >}}
{{< /tabs >}}


## 归并排序

[归并排序](https://zh.wikipedia.org/wiki/%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F) (Merge sort) 是创建在归并操作上的一种有效的排序算法。归并排序的平均和最差情况时间均为 {{< katex >}}O(n \log n){{< /katex >}}，空间为 {{< katex >}}O(n){{< /katex >}}

类似二叉树的 **后序** 遍历，先 recur 左右，再 visit 自己，所以迭代采用树的后序遍历模板

{{< tabs "merge-sort" >}}
{{< tab "递归形式" >}}

```python
def merge_sort(nums: list) -> list:
    def merge(l1: list, l2: list) -> list:
        res = []
        while l1 and l2:
            res.append(l1.pop(0) if l1[0] < l2[0] else l2.pop(0))
        res.extend(l1 if l1 else l2)
        return res

    def recur(nums: list) -> list:
        if len(nums) <= 1:
            return nums
        mid = len(nums) // 2
        return merge(recur(nums[:mid]), recur(nums[mid:]))

    return recur(nums)
```

{{< /tab >}}
{{< tab "迭代形式" >}}

```python
def merge_sort_iter(nums: list) -> list:
    def merge(l1: list, l2: list) -> list:
        res = []
        while l1 and l2:
            res.append(l1.pop(0) if l1[0] < l2[0] else l2.pop(0))
        res.extend(l1 if l1 else l2)
        return res

    # 类似树的后序遍历，stack 里存 index 而不是 list 是因为这里每层迭代都需要上一次的结果，而不是缓存下来
    stack = [(0, len(nums))]
    res = []
    while stack:
        left, right = stack.pop()
        if left >= right - 1: continue
        mid = left + (right - left) // 2
        res.append((left, mid, right))
        stack.append((left, mid))  # 先入栈左半段
        stack.append((mid, right))  # 再入栈右半段

    for left, mid, right in reversed(res):
        nums[left:right] = merge(nums[left:mid], nums[mid:right])
    return nums
```

{{< /tab >}}
{{< /tabs >}}


## 快速排序

[快速排序](https://zh.wikipedia.org/wiki/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F) (Quicksort)，又称分区交换排序 (partition-exchange sort)。此方法为原地快排，平均时间为 {{< katex >}}O(n \log n){{< /katex >}}，最差情况 {{< katex >}}O(n^2){{< /katex >}}，空间为 {{< katex >}}O(1){{< /katex >}}

类似二叉树的 **前序** 遍历，先 visit 自己，再 recur 左右，所以迭代用栈替代即可

{{< tabs "quick-sort" >}}
{{< tab "递归形式" >}}

```python
def quick_sort(nums: list):
    def partition(left, right):
        store = left
        for i in range(left, right):
            if nums[i] <= nums[right]:
                nums[store], nums[i] = nums[i], nums[store]
                store += 1
        nums[store], nums[right] = nums[right], nums[store]
        return store

    def recur(left, right):
        if left >= right: return
        store = partition(left, right)
        recur(left, store - 1)
        recur(store + 1, right)

    recur(0, len(nums) - 1)
    return nums
```

{{< /tab >}}
{{< tab "迭代形式" >}}

```python
def quick_sort_iter(nums: list):
    def partition(left, right):
        store = left
        for i in range(left, right):
            if nums[i] <= nums[right]:
                nums[store], nums[i] = nums[i], nums[store]
                store += 1
        nums[store], nums[right] = nums[right], nums[store]
        return store

    stack = [(0, len(nums) - 1)]
    while stack:
        left, right = stack.pop()
        if left >= right: continue
        store = partition(left, right)
        stack.append((store + 1, right))
        stack.append((left, store - 1))

    return nums
```

{{< /tab >}}
{{< /tabs >}}

