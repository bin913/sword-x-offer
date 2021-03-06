# 二叉搜索树的第k个结点

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8） 中，按结点数值大小顺序第三小结点的值为4。

## 分析

二叉查找树（Binary Search Tree），（又：二叉搜索树，二叉排序树）它或者是一棵空树，或者是具有下列性质的二叉树：

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉排序树。

因此，按照中序遍历（左-根-右）二叉查找树，就可以从小到大的方式遍历节点。若以`右-根-左`方式遍历，则可以从大到小的方式遍历节点。

## 递归：中序遍历

### 中序遍历1

```cpp
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
    unsigned int count = 0;
public:
    TreeNode* KthNode(TreeNode* pRoot, int k) {
        if(pRoot && k > 0) {
            TreeNode* ret = KthNode(pRoot->left, k);
            if(ret) return ret;
            if(++count == k) return pRoot;
            ret = KthNode(pRoot->right, k);
            if(ret) return ret;
        }
        return nullptr;
    }
};
```

### 中序遍历2

```cpp
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
typedef TreeNode* pnode;
class Solution {
    unsigned int m;
    pnode ans;
public:
    void dfs(pnode p) {
        if(!p || m == 0) return;
        dfs(p->left);
        if(m == 1) ans = p; // m--在后，因而与1比较
        m--;
        if(m > 0) dfs(p->right); //if判断是否小于等于0，没必要算下去了，判断可有可无
    }
    TreeNode* KthNode(TreeNode* pRoot, int k) {
        ans = nullptr; m = k;
        dfs(pRoot);
        return ans;
    }
};
```

## 非递归：中序遍历

```cpp
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
public:
    TreeNode* KthNode(TreeNode* pRoot, int k) {
        if(!pRoot || k < 1) return nullptr;
        TreeNode* p = pRoot;
        stack<TreeNode*> s;
        unsigned int count = 0;
        while(p || s.size()) {
            while(p) {
                s.push(p);
                p = p->left;
            }
            if(s.size()) { // 注意这里是if，不是while
                count++;
                p = s.top();
                if(count == k)
                    return p;
                s.pop();
                p = p->right; // 取出当前节点的右孩子，用于下一轮while找左孩子
            }
        }
        return nullptr;
    }
};
```
