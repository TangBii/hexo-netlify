---
title: LeetCode-Simple
header-img: back.png
header-color: 'rgb(255, 255, 255)'
tag-color: '34,42,53'
catalog: true
toc-number: false
date: 2020-02-24 07:59:51
tags: ['javaScript', 'LeetCode']
---

## 1. 两数之和

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
>
> 示例：
>
> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]

```js
var twoSum = function(nums, target) {
    const temp = []
    for(let i =0, length = nums.length; i < length; i++) {
        let dif = target - nums[i]
        if(temp[dif] !== undefined) {
            return [temp[dif], i]
        }
        temp[nums[i]] = i
    }
    return []
};
```

![01](01.png)

使用散列表 `temp`:

- `i` 作为散列表的值
- `nums[i] `作为散列表的索引
- 令`dif = target - nums[i]`
- 判断 `temp[dif ]` 是否存在:
  - 存在, 结果即为 `[temp[dif , i]`
  - 不存在，令 `temp[nums[i]] = i`

需要注意的是:

- 结果的顺序。如果存在结果，第一项必然是 `temp[dif ]`，第二项是 `i`
- 判定的条件。不能只 `if(temp[dif])` 因为 `temp[dif]` 可能取值为 0 ， 必须使用 `temp[dif] !== undefined` 判断