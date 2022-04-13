# Merge Two Sorted Lists

- Problem: https://leetcode.com/problems/merge-two-binary-trees/
- Level: Easy

### Explanation

With binary tree, we can use tree traversal methods to walk through all nodes of two trees accordingly and merge them together, also adding nullability check to avoid using `nil` node.

### Code

Here I use in-order tree traversal to traverse through two trees.

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func mergeTrees(root1 *TreeNode, root2 *TreeNode) *TreeNode {
    return mergeRec(root1, root2)
}

func mergeRec(r1 *TreeNode, r2 *TreeNode) *TreeNode {
    r := TreeNode{}
    
    if r1 != nil && r2 != nil {
        r.Left = mergeRec(r1.Left, r2.Left)
        r.Val = r1.Val + r2.Val
        r.Right = mergeRec(r1.Right, r2.Right)
    } else if r1 == nil && r2 != nil {
        r.Left = mergeRec(nil, r2.Left)
        r.Val = r2.Val
        r.Right = mergeRec(nil, r2.Right)
    } else if r1 != nil && r2 == nil {
        r.Left = mergeRec(r1.Left, nil)
        r.Val = r1.Val
        r.Right = mergeRec(r1.Right, nil)
    } else {
        return nil
    }
    
    return &r
}
```

### Complexity

- Time: O(max(n,m)) with n and m is number of nodes in two input trees accordingly.
- Space: O(max(n,m)) due to stack that is used for recursive calls.
