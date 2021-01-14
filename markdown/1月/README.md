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

### [3. 剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

1. bfs题解： 直接每次下一层就深度加1

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
 * @return {number}
 */
var maxDepth = function(root) {
    if (!root) return 0;
    //bfs
    let queue = [root]
    let length = 0;
    while (queue.length) {
        let size = queue.length;
        length++;
        for (let i = 0; i < size; i++) {
            let node = queue.shift();
            if (node.left) {
                queue.push(node.left)
            }
            if (node.right) {
                queue.push(node.right)
            }
        }
    }
    return length;
};
```

2. dfs解法：  

直接用0记录最底层，然后返回最大地长度+1

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
 * @return {number}
 */
var maxDepth = function(root) {
    if (!root) return 0;

    const dfs = (root) => {
        if (!root) return 0;
        let left = dfs(root.left) + 1;
        let right = dfs(root.right) + 1;
        return Math.max(left, right)
    }
    return dfs(root);

};
```

### [4. 剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

1. 首先求左右子树的长度
2. 然后计算对应高度差
3. 检测高度差是否小于1
4. 递归计算左子树和右子树

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
 * @return {boolean}
 */
const dfs = (root) => {
    if (!root) return 0;
    let left = dfs(root.left) + 1;
    let right = dfs(root.right) + 1;
    return Math.max(left, right)
}
var isBalanced = function(root) {
    if (!root) return true;
    // 平衡二叉树
    let length = Math.abs(dfs(root.left) - dfs(root.right));
    return length <= 1 && isBalanced(root.left) && isBalanced(root.right);
};
```

### [5. 1018. 可被 5 整除的二进制前缀](https://leetcode-cn.com/problems/binary-prefix-divisible-by-5/)

给定由若干 0 和 1 组成的数组 A。我们定义 N_i：从 A[0] 到 A[i] 的第 i 个子数组被解释为一个二进制数（从最高有效位到最低有效位）。

返回布尔值列表 answer，只有当 N_i 可以被 5 整除时，答案 answer[i] 为 true，否则为 false。

思路：  
可被5整除的数字只跟该数字的最后一位数字（为0或5）有关系，于是不需要具体的算出二进制前缀对应的十进制整数是多少，只需每次保留最后一位数字（保留用该数字对10取余的十进制整数的结果）就好，而下一个二进制前缀对应的十进制整数 = 上一次的结果左移一位（乘以2）的结果 + 这次的A[i]（0或者1，正好对应十进制的0或者1）的结果。
``` js
/**
 * @param {number[]} A
 * @return {boolean[]}
 */

var prefixesDivBy5 = function(A) {
    // 检测
    let ans = [];
    let number = 0;
    for (let i = 0; i < A.length; i++) {
        number = number << 1;
        number += A[i];
        number %= 10;
        ans.push(number % 5 === 0)
    }
    return ans;
};
```
