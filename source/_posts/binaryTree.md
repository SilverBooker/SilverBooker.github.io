---
title: 二叉树复习（WIP）
date: 2020-12-05 17:12:02
tags: ['算法']
---

<!-- toc -->

<!--more-->
## 前序遍历
根左右
可以通过递归与循环实现。
### 递归
递归体内是问题的最简处理形式，节点按照根左右的按顺序入 list，递归退出条件是节点为空。同时需要在递归体外定义一个作用域更高的变量 result 来记录入 list 的顺序，最后 list 的状态就是前序遍历的结果。中序遍历，后序遍历与这个方法都在框架上是相同的。
时间复杂度：O(n), 其中 n 是二叉树的节点数，每个节点被访问一次。
空间复杂度：平均 O(logN), 最坏情况为 O(n) (树完全为链状)
#### Java

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        preorder(root, result);
        return result;
    }

    public void preorder(TreeNode root, List<Integer> result){
        if (root == null){
            return;
        }

        result.add(root.val);
        preorder(root.left, result);
        preorder(root.right, result);
    }
}
```
#### Python3
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    result = []

    def preorderTraversal(self, root: TreeNode) -> List[int]:
        def preorder(root: TreeNode):
            if root is None:
                return
            result.append(root.val)
            preorder(root.left)
            preorder(root.right)
        result = list()
        preorder(root)
        return result
```

### 迭代

递归的时候隐式的维护了一个栈，迭代的时候可以显示的模拟出来。

#### Java
```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        if (root == null){
            return result;
        }
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode node = root;
        while (!stack.isEmpty() || node != null){
            while (node != null){
                result.add(node.val);
                stack.push(node);
                node = node.left;
            }
            node = stack.pop();
            node = node.right;
        }

        return result;
        
    }
}
```

#### Python3
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = list()
        if not root:
            return result

        stack = list()
        node = root

        while stack or node:
            while node:
                result.append(node.val)
                stack.append(node)
                node = node.left
            node = stack.pop()
            node = node.right

        return result
```

### Morris 遍历

思路与算法

>有一种巧妙的方法可以在线性时间内，只占用常数空间来实现前序遍历。这种方法由 J. H. Morris 在 1979 年的论文「Traversing Binary Trees Simply and Cheaply」中首次提出，因此被称为 Morris 遍历。

>Morris 遍历的核心思想是利用树的大量空闲指针，实现空间开销的极限缩减。其前序遍历规则总结如下：

1. 新建临时节点，令该节点为 root；

2. 如果当前节点的左子节点为空，将当前节点加入答案，并遍历当前节点的右子节点；

3. 如果当前节点的左子节点不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点：

4. 如果前驱节点的右子节点为空，将前驱节点的右子节点设置为当前节点。然后将当前节点加入答案，并将前驱节点的右子节点更新为当前节点。当前节点更新为当前节点的左子节点。

5. 如果前驱节点的右子节点为当前节点，将它的右子节点重新设为空。当前节点更新为当前节点的右子节点。

6. 重复步骤 2 和步骤 3，直到遍历结束。

例如：
[![r9Cd4H.png](https://s3.ax1x.com/2020/12/08/r9Cd4H.png)](https://imgchr.com/i/r9Cd4H)

* 将根节点1设置为cur。
* 因为cur（节点1）不为空，且cur（节点1）的左孩子节点2不为空，所以我们找到以节点2为根节点的左子树中最右端的节点5。
* 节点5右孩子为空，此时我们输出cur（节点1）的值，然后将节点5右孩子指向为cur，即节点1。更新* cur节点为cur左孩子，即节点2。
因为cur（节点2）左孩子不为空，找到其左子树最右端节点4
* 节点4右孩子为空，先输出cur（节点2）的值，再将节点4右孩子指向cur（节点2）,并更新cur（节点2）为其左孩子节点4。
这个时候cur（节点4）的左孩子为空，所以访问其右孩子，发现右孩子指向了节点2，所以我们将cur更新为节点2。
* 这个时候我们发现cur又指向了节点2，所以左孩子节点4不为空，我们再次找到左子树中最右端节点4，但是这个时候节点4的右孩子指向了cur，所以我们将其删除，即节点4右孩子指向为空，恢复原来的树结构。并且由于已经访问了左孩子和根节点，所以这个时候我们访问其右孩子节点5。

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        if (root == null){
            return result;
        }
        TreeNode p1 = root;
        while(p1!=null){
            TreeNode p2 = p1.left;
            if (p2!=null){
                while (p2.right!=null && p2.right!=p1){
                    p2 = p2.right;
                }
                if (p2.right==null){
                    result.add(p1.val);
                    p2.right = p1;
                    p1 = p1.left;
                    continue;
                }
                else{
                    p2.right = null;
                }
            }
            else{
                result.add(p1.val);
            }
            p1 = p1.right;
        }
        return result;
    }
}
```

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = list()
        if not root:
            return result
        p1 = root
        while p1:
            p2 = p1.left
            # 如果当前节点的有左孩子就进入该流程
            if p2:
                # 找到当前节点左子树的最右节点，将最右节点的右孩子指向当前节点
                while p2.right and p2.right!=p1:
                    p2 = p2.right
                if not p2.right:
                    result.append(p1.val)
                    p2.right = p1
                    p1 = p1.left
                    continue
                # 如果当前节点左子树的最右节点是当前节点本身，要消环
                else:
                    p2.right=None
            # 如果当前节点没有左孩子，将当前节点加入答案，然后将当前节点移动到右孩子
            else:
                result.append(p1.val)
            p1 = p1.right
        return result
```

## 中序遍历
