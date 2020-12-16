# 12月中算法题汇总

12/161
## [1. 只出现一次的数字](https://leetcode-cn.com/leetbook/read/top-interview-questions/xm0u83/)

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
说明：
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

解法：
因为两次是偶数次数，而一个数进行偶数次异或会变成0，所以这题使用异或可以轻松解决

``` js
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    let ans = 0;
    nums.forEach(item => {
        ans ^= item;
    })
    return ans;
};
```

## [2. 多数元素](https://leetcode-cn.com/leetbook/read/top-interview-questions/xm77tm/)

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组出现次数大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。

解法一：hash表  
构造hash表保存存取次数，然后取出出现次数最多的元素

``` js
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
    // 哈希表
    let obj = {};
    for (const item of nums) {
        if (obj[item]) {
            obj[item] += 1;
        } else {
            obj[item] = 1;
        }
        if (obj[item] > nums.length / 2) {
            return item;
        }
    }
};
```

解法二：Boyer-Moore 投票算法  
由于本题中要寻找的元素是过半的大多数元素，所以我们可以类比到生活中的投票，显然，如果我们把众数记为 +1，把其他数记为 -1，将它们全部加起来，显然和大于 0，从结果本身我们可以看出众数比其他数多。

在这个算法中，我们设置两个变量count和candidate，一开始为count赋予初值0，然后遍历数组，在每次count等于0的时候，我们将当前遍历元素赋值给candidate，然后再进行比较，当当前元素等于candidate，那么就让count+1,否则就让count-1。

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
    // 投票算法
    let count=0,candidate=nums[0]
    for(let item of nums){
        if(count===0){
            candidate = item;
        }
        if(item===candidate){
            count+=1;
        }else{
            count-=1;
        }
    }
    return candidate;
};
```

## [3.搜索二维矩阵](https://leetcode-cn.com/leetbook/read/top-interview-questions/xmlwi1/)
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。
 
这样的矩阵，从右上角往左下角看，就是一个二叉搜索树，所以我们从右上角作为切入点进行二分搜索：
```js
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var searchMatrix = function (matrix, target) {
    let left = 0, right = matrix[0].length - 1;
    while (left < matrix.length && right > -1) {
        if (matrix[left][right] > target) {
            right--;
        } else if (matrix[left][right] < target) {
            left++;
        } else {
            return true;
        }
    }
    return false;
};
```

## [4.合并两个有序数组](https://leetcode-cn.com/leetbook/read/top-interview-questions/xmi2l7/)
给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。  
说明：  
初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。  
解法一（利用临时数组从前往后）
```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function (nums1, m, nums2, n) {
    // 比较排序
    let point1 = 0, point2 = 0;
    let temp = [], count = 0;
    while (point1 < m && point2 < n) {
        if (nums1[point1] <= nums2[point2]) {
            temp[count++] = nums1[point1++];
        } else {
            temp[count++] = nums2[point2++];
        }
    }
    while (point1 < m) {
        temp[count++] = nums1[point1++]
    }
    while (point2 < n) {
        temp[count++] = nums2[point2++]
    }
    for (let i = 0; i < count; i++) {
        nums1[i] = temp[i];
    }
};
```
解法二：从后往前插入  
```js
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function(nums1, m, nums2, n) {
    // 从后往前
    let point1=m-1,point2=n-1,count=m+n-1;
    while(point1>-1&&point2>-1){
        if(nums1[point1]>nums2[point2]){
            nums1[count--]=nums1[point1--];
        }else{
             nums1[count--]=nums2[point2--];
        }
    }
    while(point1>-1){
        nums1[count--]=nums1[point1--];
    }
    while(point2>-1){
        nums1[count--]=nums2[point2--];
    }
};
```

## [5.单词规律](https://leetcode-cn.com/problems/word-pattern/)
给定一种规律 pattern 和一个字符串 str ，判断 str 是否遵循相同的规律。

这里的 遵循 指完全匹配，例如， pattern 里的每个字母和字符串 str 中的每个非空单词之间存在着双向连接的对应规律。

示例1:

输入: pattern = "abba", str = "dog cat cat dog"  
输出: true  
 
解法：hash表进行记录  
```js
/**
 * @param {string} pattern
 * @param {string} s
 * @return {boolean}
 */
var wordPattern = function(pattern, s) {
    let obj={}
    let testArr = s.split(' ');
    // 判断长度
    if(pattern.length!==testArr.length){
        return false;
    }
    for(let i=0;i<testArr.length;i++){
        // 进行hash匹配
        if(obj[pattern[i]]){
            if(testArr[i]!==obj[pattern[i]]){
                return false;
            }
        }else{
            // 判断是否在对象中出现过
            for(const key in obj){
                if(obj[key]===testArr[i]){
                    return false;
                }
            }
            obj[pattern[i]]=testArr[i];
        }
    }
    return true;
};
```