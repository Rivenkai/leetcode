#### 二叉树

##### 验证二叉搜索树

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

递归：

```c++
bool helper(TreeNode* root, long min, long max) {
	if (root == nullptr)
		return true;

	if (root->val <= min || root->val >= max)
		return false;

	return helper(root->left, min, root->val) && helper(root->right, root->val, max);
}
bool isValidBST(TreeNode* root) {
    return helper(root, LONG_MIN, LONG_MAX);
}
```

中序遍历： 迭代

```c++
bool isValidBST(TreeNode* root) {
    stack<TreeNode*> stack;
    long long inorder = (long long)INT_MIN - 1;
    
    while (!stack.empty() || root){
        if(root) {
            stack.push(root);
            root = root->left;
        }
        root = stack.top();
        stack.pop();
        
        if (root->val < inorder)
            return false;
        
        inorder = root->val;
        root = root->right;
    }
    return true;
}
```



##### 对称二叉树

给定一个二叉树，检查它是否是镜像对称的。

递归：

```c++
bool isSymmetric(TreeNode* root) {
    if (root == NULL)
        return true;
        
    return isMirror(root, root);
}

bool isMirror(TreeNode* p, TreeNode* q) {
    if (!p && !q)
        return true;
        
    if (!p || !q)
        return false;
        
    if (p->val == q->val)
        return isMirror(p->left, q->right) && isMirror(p->right, q->left);
        
    return false;
}
```

迭代+队列：

```c++
bool isSymmetric(TreeNode* root) {
    return check(root, root);
}

bool check(TreeNode* u, TreeNode* v) {
    queue<TreeNode *> q;
    q.push(u);
    q.push(v);
    
    while (!q.empty()) {
        u = q.front();
        q.pop();
        
        v = q.front();
        q.pop();
        
        if (!u && !v)
            continue;
        
        if (!u || !v)
            return false;
        
       	q.push(u->left);
        q.push(v->right);
        
        q.push(u->right);
        q.push(v->left);
    }
    return true;
}
```



##### 二叉树的层次遍历

给一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）

递归：

```c++
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> ans;
    preOrder(root, 0, ans);
    
    return ans;
}

void preOrder(TreeNode* root, int depth, vector<vector<int>> &ans) {
    if (!root)
        return ;
        
    if(depth >= ans.size())
        ans.push_back(vector<int> {});
        
    ans[depth].push_back(root->val);
    preOrder(root->left, depth+1, ans);
    preOrder(root->right, depth+1, ans);
}
```



BFS +队列：

```c++
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if (!root)
        return ret;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int curSize = q.size();
        res.push_back(vector<int> ());
        for (int i = 1; i<=curSize; ++i) {
            auto node = q.front();
            q.pop();
            res.back().push_back(node->val);
            
            if (node->left)
                q.push(node->left);
            if (node->right)
                q.push(node->right);
        }
    }
    
    return res;
}
```

