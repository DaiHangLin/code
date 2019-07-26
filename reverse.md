``` js
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
function calc(result, x, origLength) {
  let length = `${Math.abs(x)}`.length
  const base = Math.pow(10, length - 1)
  const base2 = Math.pow(10, origLength - length)
  if (length === 1) {
    return result + x *  base2
  }
  const data = x > 0 ? Math.floor(x / base) : Math.round(x / base)
  result = result + data * base2
  return calc(result, x - data * base, origLength)
}

var reverse = function(x) {
  const t = x > 0 ? 1 : -1
  const result = t * calc(0, Math.abs(x), `${Math.abs(x)}`.length)

  if (result < -2147483648 || result > 2147483647) {
    return 0
  }
  return result

};
```
