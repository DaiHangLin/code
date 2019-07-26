```js
/*
 * @lc app=leetcode.cn id=7 lang=javascript
 *
 * [7] 整数反转
 *
 * https://leetcode-cn.com/problems/reverse-integer/description/
 *
 * algorithms
 * Easy (32.67%)
 * Likes:    1166
 * Dislikes: 0
 * Total Accepted:    151K
 * Total Submissions: 462.3K
 * Testcase Example:  '123'
 *
 * 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
 * 
 * 示例 1:
 * 
 * 输入: 123
 * 输出: 321
 * 
 * 
 * 示例 2:
 * 
 * 输入: -123
 * 输出: -321
 * 
 * 
 * 示例 3:
 * 
 * 输入: 120
 * 输出: 21
 * 
 * 
 * 注意:
 * 
 * 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回
 * 0。
 * 
 */
/**
 * @param {number} x
 * @return {number}
 */

var reverse2 = function(x) {
  let rev = 0
  let pop = 0
  while (x !== 0) {
    pop = x % 10;
    x = Math.floor(x / 10)
    rev = rev * 10 + pop;
  }

  if (rev <= -2147483648 || rev >= 2147483647) {
    return 0
  }
  return rev
};

var reverse = function(x) {
  // const result = twoSums([-1,-2,-3,-4,-9], -13)
  const t = x > 0 ? 1 : -1
  return t * reverse2(Math.abs(x))
}

```