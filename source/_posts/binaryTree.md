---
title: 二叉树复习（WIP）
date: 2020-12-05 17:12:02
tags: ['算法']
---

<!-- toc -->

## 前序遍历
根左右
可以通过递归与循环实现。
### 递归
递归体内是问题的最简处理形式，节点按照根左右的按顺序入 list，递归退出条件是节点为空。同时需要在递归体外定义一个作用域更高的变量 result 来记录入 list 的顺序，最后 list 的状态就是前序遍历的结果。中序遍历，后序遍历与这个方法都在框上是相同的。
<!--more-->
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
