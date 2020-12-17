# 字符串专题：
## [1. 验证回文串](https://leetcode-cn.com/leetbook/read/top-interview-questions/xah8k6/)
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。
说明：本题中，我们将空字符串定义为有效的回文串。

双指针入门题
```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function (s) {
    // 双指针移动判断
    function isNeedCheck(char) {
        char = String(char)
        return  (char.toUpperCase() >= 'A' && char.toUpperCase() <= 'Z') || (char >= '0' && char <= '9')
    }
    let left = 0, right = s.length - 1;
    while (left <= right) {
        while (!isNeedCheck(s[left])||s[left]===' ') {
            left++;
        }
        while(!isNeedCheck(s[right])||s[right]===' '){
            right--;
        }
        if(left<right){
            if(s[left].toUpperCase()!==s[right].toUpperCase()){
                return false;
            }
        }
        left++;
        right--;
    }
    return true;
};
```