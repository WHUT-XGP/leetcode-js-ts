# 1月算法题

## 剑指offer

### [1. 剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)
一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

题解：通过二分即可

``` js
// 二分
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function(nums) {
    // 二分找法
    let left = 0,
        right = nums.length - 1;
    while (left <= right) {
        mid = Number.parseInt(left + (right - left) / 2);
        // 判断mid是否小于对应值
        if (nums[mid] != mid) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return left

};
```

### [2. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

给定一棵二叉搜索树，请找出其中第k大的节点。

利用二叉搜索树中序遍历返回升序排列地特性

``` js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} k
 * @return {number}
 */
var kthLargest = function(root, k) {
    // 中序遍历返回第k
    let ans = [];
    const helper = (root) => {
        if (root) {
            helper(root.left);
            ans.push(root.val)
            helper(root.right)
        }
    }
    helper(root);
    return ans[ans.length - k]
};
```
