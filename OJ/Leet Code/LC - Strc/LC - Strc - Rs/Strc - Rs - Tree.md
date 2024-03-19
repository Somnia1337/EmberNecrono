```text
题库: 94
```

#### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

@2024.02.21

```rust
pub fn inorder_traversal(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<i32> {
	let mut ans = vec![];
	Self::dfs(&root, &mut ans);
	ans
}

fn dfs(node: &Option<Rc<RefCell<TreeNode>>>, ans: &mut Vec<i32>) {
	if node.is_none() { return; }
	let nd = node.as_ref().unwrap().borrow();
	Self::dfs(&nd.left, ans);
	ans.push(nd.val);
	Self::dfs(&nd.right, ans);
}
```