#LeetCode题解-树篇
## 104. 二叉树的最大深度
给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。
### 解法
树的问题绝大多数要用递归求解：
* 设置基线条件：当遇到叶子节点时，返回1，当遇到空节点时，返回0
* 设置递归条件：用HighL表示左节点的高度，用HighR表示右节点的高度，这两个高度应该都是递归的结果，当前的高度就是左右节点的最大高度+1 
```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) { return 0; }
        if (isLeaf(root)) { return 1; }
        int highL = maxDepth(root->left);
        int highR = maxDepth(root->right);
        return max(highL, highR) + 1;
    }
    int isLeaf(TreeNode* root) {
        if (root != nullptr && root->left == nullptr && root->right == nullptr) {
            return true;
        }
        return false;
    }
};
```
## 110. 平衡二叉树
给定一个二叉树，判断它是否是高度平衡的二叉树。
本题中，一棵高度平衡二叉树定义为：
一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。
### 解法
