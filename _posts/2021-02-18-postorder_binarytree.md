---
layout: post
title: 后序遍历二叉树（迭代）自己理解后写出的版本
comments: true
toc: true
tags: [postorder, binarytree, postordertraverse]
---

# 起因
在看邓俊辉在学堂在线上的数据结构课的时候，发现前序/中序的二叉树迭代遍历讲的都非常好，偏偏不讲后序。看了书上的讲解后也觉得一头雾水，尤其是需要去找左侧最深可见叶子节点那里，我基本需要背诵代码的逻辑，非常痛苦。
所以开始在网上找有没有更好理解的逻辑去写这个后序遍历。

# 发现
我发现，大多数讲解后序遍历的博客多提到两个关键点，`第一个是要关注`：对于一个节点node，我们什么时候才对其进行访问？答案是在左右子树都访问完后，对node进行访问；或是左右子树都空时，这就等同于左右子树都访问完了。
`第二个要关注`的点是，后序遍历的访问次序是怎么样的？答案是，左｜右｜当前根节点。

基于以上的两个理解，我们不难发现，每次遇到一个节点，我们需要首先进行一个判断，当前这个节点是否满足访问条件？如果满足，对其进行访问，并且出栈（表示访问完成了）。如果不满足，那么我们先不要访问当前这个节点，（不要出栈），而是将当前节点的右/左子树根节点依次入栈，并且进入下一次循环。

# 实现
```cpp
vector<int> postorderTraversal(TreeNode* root) {
        if (!root) return vector<int> {};
        stack<TreeNode *> s;
        vector<int> res;
        
        s.push(root);
        TreeNode *prev = nullptr, *node;
        while (!s.empty()) {
            node = s.top();
            if (node->left == nullptr && node->right == nullptr || node->right && prev == node->right || node->left && node->left == prev && node->right == nullptr) {
                res.push_back(node->val);
                prev = node;
                s.pop();
            } else {
                if (node->right != nullptr) {
                    s.push(node->right);
                }
                if (node->left != nullptr)
                    s.push(node->left);
            }
        }
        return res;
    }
```