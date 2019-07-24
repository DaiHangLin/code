### 两数之和
```js
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

### 解答

```
 这道题考察的内容其实是查找
 可以把题目分解为，已知一个目标值target，一个已知值data, 
 判断另一个值 target 在数组中是否存在

 Map/Set查找的效率是O(1)的，所以应该利用这个特性
```

```js
var twoSum = function(nums, target) {
  let map = new Map()
  for (let i in nums) {
    const o = target - nums[i]
    const oi = map.get(o)
    if (map.has(o) && oi !== i) {
      return [oi, i]
    }
    map.set(nums[i], i)
  }
};
```