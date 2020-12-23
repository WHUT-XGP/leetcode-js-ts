## [12/17 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transacti)

给定一个整数数组 prices，其中第 i 个元素代表了第 i 天的股票价格 ；非负整数 fee 代表了交易股票的手续费用。  
你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。  
返回获得利润的最大值。  
注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

解法一（动态规划）  
因为本题规定，我们无法在卖出股票时继续购买股票，所以对于任何一天，都必定存在两种状态（手上持有股票或者手上没有股票)  
我们使用dp[i][0]表示第i+1天手上没有股票时的收益，dp[i][1]表示第i+1天手上有股票时的收益。  
根据这种设定，那么我们可以根据第一天的值设定为：  
dp[0][0]=0, 
dp[0][1]=-prices[0]  
对于手上没有股票的情况，可能是这一天的上一天就没有股票，也可能是在这一天将昨天的股票卖出。而对于这两种情况，前者显然就是dp[i-1][0]，而后者则是dp[i-1][1]+prices[i]-fee, 我们对于两者取最大值就可以获得对应结果。  
而对于手上持有股票的情况，则可能是这一天的上一天就持有股票，也可能是昨天没有股票，今天选择了买入。同样的对于两者而言，前者就是保持昨日收益情况dp[i-1][1], 而后者选择了买入，就是dp[i-1][0]-prices[i]，我们同样对于这两个结果取最大值，即可获取对应的结果  
保存状态继续迭代，遍历数组后我们可以得到两个结果，即dp[prices.length-1][0]和dp[prices.length-1][1]，显然手上还有股票的话，必定是要小于手上没有股票的情况的，所以我们直接返回dp[prices.length-1][0]即可。

``` js
/**
 * @param {number[]} prices
 * @param {number} fee
 * @return {number}
 */
var maxProfit = function(prices, fee) {
    const dp = new Array(prices.length).fill(new Array(2).fill(0));
    dp[0][0] = 0;
    dp[0][1] = -prices[0];
    for (let i = 1; i < prices.length; i++) {
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee);
        dp[i][1] = Math.max(dp[i - 1][0] - prices[i], dp[i - 1][1]);
    }
    return dp[prices.length - 1][0]
};
```

## [12/22 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

给定一个二叉树，返回其节点值的锯齿形层序遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。  

思路：只需要按正常层序遍历的bfs思路，在bfs基础上加个flag作为标记，每一层加入数组时进行判断，从而倒置数组，再进行插入即可  

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
 * @return {number[][]}
 */
var zigzagLevelOrder = function(root) {
    if (!root) {
        return [];
    }
    let queue = [root];
    let res = [];
    let flag = false;
    5
    while (queue.length) {
        let length = queue.length;
        let temp = [];
        for (let i = 0; i < length; i++) {
            let node = queue.shift();
            temp.push(node.val);
            if (node.left) {
                queue.push(node.left)
            }
            if (node.right) {
                queue.push(node.right)
            }
        }
        if (flag) {
            temp = temp.reverse();
        }
        flag = !flag
        res.push(temp);
    }
    return res;
};
```

## [12/23 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

遍历两遍：

``` js
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
    // hash表两边遍历
    let obj = {}
    for (let item of s) {
        obj[item] ? obj[item]++ : obj[item] = 1
    }
    for (let i in s) {
        if (obj[s[i]] === 1) {
            return i;
        }
    }
    return -1;
};
```
