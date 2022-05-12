# LeetCode

## [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

> 题目描述

![image-20220430122352827](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204301223985.png)

![image-20220430122409414](https://cdn.jsdelivr.net/gh/kkkkwqqqq/typora/typoraImage/202204301224501.png)

> 思路：**二叉树的问题难点在于，如何把题目的要求细化成每个节点需要做的事情**，

==函数定义：输入两个节点，将它俩连接起来==

```java
class Solution {
    // 主函数
    public Node connect(Node root) {
        if (root == null) return null;
        // 遍历「三叉树」，连接相邻节点
        traverse(root.left, root.right);
        return root;
    }

    // 三叉树遍历框架
    void traverse(Node node1, Node node2) {
        if (node1 == null || node2 == null) {
            return;
        }
        /**** 前序位置 ****/
        // 将传入的两个节点穿起来
        node1.next = node2;
        
        // 连接相同父节点的两个子节点
        traverse(node1.left, node1.right);
        traverse(node2.left, node2.right);
        // 连接跨越父节点的两个子节点
        traverse(node1.right, node2.left);
    }
}
// 详细解析参见：
// https://labuladong.github.io/article/?qno=116

```

