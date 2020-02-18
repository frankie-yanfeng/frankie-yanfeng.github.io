---
layout:     post   				    # 使用的布局（不需要改）
title:      Leetcode-102 Binary Tree Level Order Traversal 				# 标题
subtitle:   Leetcode-102 #副标题
date:       2020-02-18 				# 时间
author:     frankie 					# 作者
header-img: img/tag-bg-o.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Leetcode
    - Tree
    - BFS
    - DFS
---

## 分析
![分析](https://i.imgur.com/L8bo52y.png)
### BFS
[BFS](https://leetcode.com/problems/binary-tree-level-order-traversal/discuss/33753/C%2B%2B-BFS-and-DFS)
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root) {
            return {};
        }
        vector<vector<int> > levels;
        queue<TreeNode*> todo;
        todo.push(root);
        while (!todo.empty()) {
            levels.push_back({});
            for (int i = 0, n = todo.size(); i < n; i++) {
                TreeNode* node = todo.front();
                todo.pop();
                levels.back().push_back(node -> val);
                if (node -> left) {
                    todo.push(node -> left);
                }
                if (node -> right) {
                    todo.push(node -> right);
                }
            }
        }
        return levels;
    }
};
```

### DFS
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    void dfs(TreeNode* root, int depth, vector<vector<int> >& ans) {

        if (!root)
            return;

        if (ans.size() <= depth){
            ans.push_back({});
        }

        ans[depth].push_back(root->val);

        dfs(root->left,  depth+1, ans);
        dfs(root->right, depth+1, ans);

    }

    vector<vector<int>> levelOrder(TreeNode* root) {

        if (!root)
            return {};

        vector<vector<int> > ans;

        dfs(root, 0, ans);

        return ans;
    }
};
```
