# 二叉树

[二叉树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%A0%91) (Binary tree) 是每个节点最多只有两个分支的树结构。其结构定义如下：

```python
class TreeNode:
    def __init__(self, value, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

    def visit(self):
        print(self.value, end=" ")  # 访问当前结点
```

## 遍历

[树的遍历](https://zh.wikipedia.org/wiki/%E6%A0%91%E7%9A%84%E9%81%8D%E5%8E%86) 是图的遍历的一种，指的是按照某种规则，不重复地访问某种树的所有节点的过程。

遍历可分为 **深度优先遍历** 和 **广度优先遍历**，深度优先遍历又分为 **前序遍历** (Pre-Order Traversal)、**中序遍历** (In-Order Traversal)、**后序遍历** (Post-Order Traversal)；而广度优先搜索则对应 **层序遍历**。

测试树如下：

```
     A
   /   \
  B     C
 / \   / \
D   E F   G
     / \
    H   I

正确的前序遍历： A B D E C F H I G
正确的中序遍历： D B E A H F I C G
正确的后序遍历： D E B H I F G C A
正确的层序遍历： A B C D E F G H I
```

```python
test = TreeNode('A', left=TreeNode('B', left=TreeNode('D'), right=TreeNode('E')),right=TreeNode('C', left=TreeNode('F', left=TreeNode('H'), right=TreeNode('I')), right=TreeNode('G')))
```

### 递归形式

#### 前序遍历

```python
def preorder(node: TreeNode):
    if node is None: return
    node.visit()  # 访问当前结点
    preorder(node.left)
    preorder(node.right)
```

#### 中序遍历

```python
def inorder(node: TreeNode):
    if node is None: return
    inorder(node.left)
    node.visit()  # 访问当前结点
    inorder(node.right)
```

#### 后序遍历

```python
def postorder(node: TreeNode):
    if node is None: return
    postorder(node.left)
    postorder(node.right)
    node.visit()  # 访问当前结点
```

### 迭代形式

#### 前序遍历 1：栈模拟递归

```python
def preorder_iter(root: TreeNode):
    if root is None: return
    stack = [root]
    while stack:
        node = stack.pop()
        if node is None: continue
        node.visit()  # 访问当前结点
        stack.append(node.right)  # 先入栈右子树
        stack.append(node.left)  # 再入栈左子树
```

#### 后序遍历 1：类前序遍历 1 + reverse：先用栈模拟，再利用双栈将 根-右-左 的结果反过来

```python
def postorder_iter(root: TreeNode):
    if root is None: return
    stack = [root]
    res = []
    while stack:
        node = stack.pop()
        if node is None: continue
        res.append(node)
        stack.append(node.left)  # 先入栈左子树
        stack.append(node.right)  # 再入栈右子树
    for node in reversed(res):
        node.visit()
```

#### 前序遍历 2

```python
def preorder_iter2(root: TreeNode):
    if root is None: return
    stack = []
    node = root
    while node or stack:
        while node:  # 先找最左的 node，路途依次入栈
            node.visit()  # 根
            stack.append(node)
            node = node.left  # 左
        node = stack.pop()
        node = node.right  # 右
```

#### 中序遍历

```python
def inorder_iter(root: TreeNode):
    if root is None: return
    stack = []
    node = root
    while node or stack:
        while node:  # 先找最左的 node，路途依次入栈
            stack.append(node)
            node = node.left  # 左
        node = stack.pop()
        node.visit()  # 根
        node = node.right  # 右
```

#### 后序遍历 2：类前序遍历 2 + reverse：先用树的非递归模板，再利用双栈将 根-右-左 的结果反过来

```python
def postorder_iter2(root: TreeNode):
    if root is None: return
    stack, res = [], []
    node = root
    while node or stack:
        while node:  # 先找最右的 node，路途依次入栈
            res.append(node)  # 根
            stack.append(node)
            node = node.right  # 右
        node = stack.pop()
        node = node.left  # 左
    for node in reversed(res):
        node.visit()
```

#### 层序遍历

```python
def levelorder_iter(root: TreeNode):
    if root is None: return
    queue = [root]
    while queue:
        node = queue.pop(0)
        node.visit()
        if node.left is not None: queue.append(node.left)
        if node.right is not None: queue.append(node.right)
```


