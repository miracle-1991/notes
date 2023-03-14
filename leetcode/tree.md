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
```
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        high(root);
        return m_is_balanced;
    }
    int high(TreeNode* root) {
        if (root == nullptr) { return 0; }
        int left = high(root->left);
        int right = high(root->right);
        if (left - right > 1 || right - left > 1) {
            m_is_balanced = false;
        }    
        return max(left, right) + 1;
    }
    
private:
    bool m_is_balanced = true;
};
```

## 543. 二叉树的直径
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。
### 解法
```
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        maxHigh(root);
        return m_max_len;
    }
    int maxHigh(TreeNode* root) {
        if (root == nullptr) { return 0; }
        int left = maxHigh(root->left);
        int right = maxHigh(root->right);
        m_max_len = left + right > m_max_len ? left + right : m_max_len;
        return max(left, right) + 1;
    }
private:
    int m_max_len = 0;
};
```

## 226. 翻转二叉树
给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。
### 解法
```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) { return root; }
        TreeNode* left = invertTree(root->left);
        TreeNode* right = invertTree(root->right);
        root->left = right;
        root->right = left;
        return root;
    }
};
```

## 617. 合并二叉树
给你两棵二叉树： root1 和 root2 。
想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。
返回合并后的二叉树。
注意: 合并过程必须从两个树的根节点开始。
### 解法
```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (root1 == nullptr) { return root2; }
        if (root2 == nullptr) { return root1; }
        TreeNode* left = mergeTrees(root1->left, root2->left);
        TreeNode* right = mergeTrees(root1->right, root2->right);
        TreeNode* head = root1;
        head->val = root1->val + root2->val;
        head->left = left;
        head->right = right;
        return head;
    }
};
```

## 112. 路径总和
给你二叉树的根节点 root 和一个表示目标和的整数 targetSum 。判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。如果存在，返回 true ；否则，返回 false 。
叶子节点 是指没有子节点的节点。
### 解法
```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        return hasPathSum(root, 0, targetSum);
    }

    bool hasPathSum(TreeNode* root, int sum, int target) {
        if (root == nullptr) { return false; }
        sum = sum + root->val;
        if (isleaf(root)) {
            return sum == target;
        }
        bool left = hasPathSum(root->left, sum, target);
        bool right = hasPathSum(root->right, sum, target);
        return left | right;
    }

    bool isleaf(TreeNode* root) {
        if (root != nullptr && root->left == nullptr && root->right == nullptr) {
            return true;
        }
        return false;
    }
};
``` 

## 437. 路径总和 III
给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。
路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
### 解法
双递归：
* 外层递归：
    * 设置基线条件： 当遇到空节点时，直接返回0
    * 设置外层递归条件：递归求解左侧/右侧的值，加上以当前节点为根节点的内部递归的值，返回
        * 内部递归：
            * 设置基线条件：当遇到空节点时，返回0
            * 设置递归条件： 判断当前的值是否等于target，Y则+1，继续内部递归左侧节点和右侧节点，递归时将target减去当前的值
```
class Solution {
public:
    int pathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) { return 0; }
        int left = pathSum(root->left, targetSum);
        int right = pathSum(root->right, targetSum);
        return left + right + pathSumWithRoot(root, targetSum);
    }
    int pathSumWithRoot(TreeNode* root, size_t targetSum) {
        if (root == nullptr) { return 0; }
        size_t left = pathSumWithRoot(root->left, targetSum - root->val);
        size_t right = pathSumWithRoot(root->right, targetSum - root->val);
        int cnt = 0;
        if (root->val == targetSum) { cnt += 1; }
        return cnt + left + right;
    }
private:
    int m_path_sum_cnt = 0;
};
```

单递归：
* 设置基线条件：当遇到空节点时，直接返回
* 设置递归条件：
    * 将当前节点加入栈(用数组表示)
    * 从数组的尾部向头部顺序计算和，相当于从当前节点向根节点回溯，每当和等于目标值，计数+1
    * 迭代左右节点
    * 将当前节点出栈(栈要描述的是从根节点到当前节点的路径，如果不出栈，会导致左边的叶子节点也在该路径中)
```
class Solution {
public:
    int pathSum(TreeNode* root, int targetSum) {
        vector<int> vl;
        pathSum(root, vl, targetSum);
        return m_path_sum_cnt;
    }

    void pathSum(TreeNode* root, vector<int>& vl, int targetSum) {
        if (root == nullptr) { return; }
        vl.push_back(root->val);
        calcCnt(vl, targetSum);
        if (root->left != nullptr) { 
            pathSum(root->left, vl, targetSum);
        }
        if (root->right != nullptr) {
            pathSum(root->right, vl, targetSum);
        }
        vl.pop_back();
        return;
    }

    void calcCnt(vector<int>& vl, int targetSum) {
        size_t sum = 0;
        for (int i = vl.size() - 1; i >= 0; i--) {
            sum += vl[i];
            if (sum == targetSum) {
                 m_path_sum_cnt++; 
            }
        }
    }
private:
    int m_path_sum_cnt = 0;
};
```

## 572. 另一棵树的子树
给你两棵二叉树 root 和 subRoot 。检验 root 中是否包含和 subRoot 具有相同结构和节点值的子树。如果存在，返回 true ；否则，返回 false 。
二叉树 tree 的一棵子树包括 tree 的某个节点和这个节点的所有后代节点。tree 也可以看做它自身的一棵子树。
### 解法
两层递归：
* 外层递归
    * 设置基线条件：如果root为空，返回false
    * 设置递归条件：如果当前节点是子树，返回true，否则，递归做节点/右节点，取并集
        * 内层递归：
            * 设置基线条件，如果root为空并且subroot也空，返回true，否则如果二者任一节点为空而另一个不为空返回false
            * 设置递归条件：当前节点必须与subroot的节点相同，递归左右节点，左右节点也必须与subroot的左右节点完全相同，这种情况才返回true，否则返回false
```
class Solution {
public:
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        if (root == nullptr) { return false; }
        if (isSubtreeWithRoot(root, subRoot)) { return true; }
        return isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);
    }

    bool isSubtreeWithRoot(TreeNode* root, TreeNode* subRoot) {
        if (root == nullptr && subRoot == nullptr) { return true; }
        if (root == nullptr || subRoot == nullptr) { return false; }
        if (root->val != subRoot->val) { return false; }
        bool left = isSubtreeWithRoot(root->left, subRoot->left);
        bool right= isSubtreeWithRoot(root->right, subRoot->right);
        return left && right; 
    }
};
```

## 101. 对称二叉树
给你一个二叉树的根节点 root ， 检查它是否轴对称。
### 解法
单层递归
* 设置基线条件：如果左侧节点与右侧节点都为空，返回true，如果二者中任一节点为空，而另外一个不为空，返回false
* 设置递归条件：如果两个节点值不相同，返回false，否则，取左边节点的右侧节点/左侧节点，取右侧节点的左侧节点/右侧节点，递归判断二者是否相同
```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == nullptr) { return true; }
        return isSymmetric(root->left, root->right);
    }

    bool isSymmetric(TreeNode* left, TreeNode* right) {
        if (left == nullptr && right == nullptr) { return true; }
        if (left == nullptr || right == nullptr) { return false; }
        if (left->val != right->val) { return false; }
        bool f1 = isSymmetric(left->left, right->right);
        bool f2 = isSymmetric(left->right, right->left);
        return f1 && f2;
    }
};
```

## 111. 二叉树的最小深度
给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
说明：叶子节点是指没有子节点的节点。
### 解法
单层递归：
* 设置基线条件：如果当前节点是空节点，返回0
* 设置递归条件：left为左侧递归的最小高度，right为右侧递归的最小高度：
    * 如果left并且right都为0，说明是叶子节点，返回高度1
    * 如果left或者right其中一个是0，由于空节点不能参与运算，能参与运算的最多截止到叶子节点，返回有值的那个+1
    * 如果left和right都不是0，返回1+min(left, right)
```
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (root == nullptr) { return 0; }
        int left = minDepth(root->left);
        int right = minDepth(root->right);
        if (left == 0 && right == 0) { return 1; }
        if (left == 0 && right != 0) { return 1 + right; }
        if (left != 0 && right == 0) { return 1 + left; }
        return 1 + min(left, right);
    }
};
```
