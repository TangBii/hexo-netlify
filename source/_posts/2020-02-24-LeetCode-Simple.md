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

### 题目描述

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

### 代码实现

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

### 解题思路

![01](01.png)

使用散列表 `temp`:

- `i` 作为散列表的值
- `nums[i] `作为散列表的索引
- 令`dif = target - nums[i]`
- 判断 `temp[dif ]` 是否存在:
  - 存在, 结果即为 `[temp[dif , i]`
  - 不存在，令 `temp[nums[i]] = i`

### 注意事项

- 结果的顺序。如果存在结果，第一项必然是 `temp[dif ]`，第二项是 `i`
- 判定的条件。不能只 `if(temp[dif])` 因为 `temp[dif]` 可能取值为 0 ， 必须使用 `temp[dif] !== undefined` 判断

## 2.  整数反转

### 题目描述

>给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
>
>示例 1:
>
>输入: 123
>输出: 321
> 示例 2:
>
>输入: -123
>输出: -321
>
> 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231, 231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。 

### 代码实现

```js
var reverse = function(x) {
    var flag = x < 0 ? '-' : '',
        x = flag ? 0 - x : x	// 也可以使用 Math.abs()
        str = String(x).split("").reverse().join("")
        min = String(2147483648),
        max = String(2147483647)
    if ((str.length < 10) || (flag && str <= min) || (!flag && str <= max)) {
    return Number(flag + str)
    }
    return 0
};
```

### 解题思路

- 利用变量 `flag` 存储符号，然后取绝对值
- 将 `x` 转换为字符型，分隔为数组，数组反转，合并为字符串实现反转
- 由于最大值和最小值都是 10 位数字所以可以使用如下方式判断结果是否符合要求:
  - 小于 10 位，符合要求
  - 大于十位，不符合要求
  - 等于 10 位，分情况与 `min` 或 `max` 比较

### 注意事项

- `String.prototype.split()` 返回的数组：
  - 不传参数组元素只有一个为原始字符串
  - 传入参数为空字符串，数组元素为字符串的每个字符
- `Array.prototype.join()`  返回的字符串:
  - 不传参数时元素以`,` 拼接
  - 传入空字符串时，元素直接拼接

## 3. 回文数

### 题目描述

> 判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。 不将整数转为字符串来解决这个问题吗
>
> **示例 1:**
>
> ```js
> 输入: 121
> 输出: true
> ```

### 代码实现

```js
    var isPalindrome = function(x) {
        if(x < 0 || (x % 10 === 0 && x !== 0)){
            return false
        }
        var revertedNumber = 0
        while(revertedNumber < x) {
            revertedNumber = revertedNumber * 10 + x % 10
            x = Math.floor(x / 10)
        }
        return revertedNumber === x || Math.floor(revertedNumber / 10) === x

    };
```

### 解题思路

- x 为负数返回 `false` , x 个位数为 0 本身不为 0 返回 `false`
- 利用循环将 x 后半部分反转与前半部分比较:
  - 循环终止条件为 `revertedNumber >= x`
  - 当 x 有奇数个数时，revertedNumeber 会比循环后的 `x `多一位，最后比较时去掉这一位即可

### 注意事项

- 使用取余操作可以获取数字的最后一位 `%`
- 使用向下取整的除操作可以抹除数字最后一位 `/`

## 4. 罗马数字转整数

### 题目描述

> 	罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
>
> 字符          数值
> I             1
> V             5
> X             10
> L             50
> C             100
> D             500
> M             1000
> 例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。
>
> 通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：
>
> I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
> X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
> C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
> 给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。
>
> 示例 1:
>
> 输入: "III"
> 输出: 3

### 代码实现

```js
var romanToInt = function(s) {
    var rules = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000,
        Z: 0
    },
        result = 0
        arr = s.split('')
        for(let i = 0, length = arr.length; i < length;) {
            const item = arr[i],
                  nextItem = arr[i + 1] || 'Z',
                  itemValue = rules[item],
                  nextItemValue = rules[nextItem]
            if(itemValue < nextItemValue) {
                result += nextItemValue - itemValue
                i = i + 2
                continue
            }
            result += itemValue
            i = i + 1
        }    
    return result
};
```

### 解题思路

- 一次判断两个字符:
  - 如果前面的值比后面的值小，两个字符的总值为后面的值减去前面的值，循环变量加 2
  - 否则，累加首个字符，循环变量加 1

### 注意事项

- `nextItem = arr[i + 1] || 'Z'` 当 arr[i + 1] 为 undefined 时赋值为 0 否则任意数与其比较都为 false

## 5. 最长公共前缀

### 题目描述

> 编写一个函数来查找字符串数组中的最长公共前缀。
>
> 如果不存在公共前缀，返回空字符串 ""。
>
> 示例 1:
>
> 输入: ["flower","flow","flight"]
> 输出: "fl"

### 代码实现

```js
var longestCommonPrefix = function(strs) {
    if(sr === null || strs.length === 0) return ""
    let prefix = strs[0]
    for(let i = 1, length = strs.length; i < length; i++) {
        while(!(strs[i].indexOf(prefix) === 0)){
            prefix = prefix.slice(0, -1)
            if(prefix === "") {
                return ""
            }
        }
    }
    return prefix
}
```

### 解题思路

- 先假设数组中第一个字符串即`strs[0]`为最长公共前缀 `prefix` 
- 遍历字符串数组:
  - 如果 `prefix` 不是相应元素的公共前缀，删除 `prefix` 最后一个字符后重新判断
  - 如果 `prefix` 为空，返回 “”

### 注意事项

- `strs[i].indexOf(prefix) === 0` 可以使用这个语句判断 prefix 是不是 strs[i] 的前缀
- `prefix.slice(0, -1)` 返回去除最后一个字符的 prefix
  - 注意区分 `slice()` 、`subString()`、`substr()` 对负数的处理方式



### 代码实现2

```js
var longestCommonPrefix = function(strs) {
    strs = strs.sort((item1, item2) => item1.length - item2.length)
    if(strs === null || strs.length === 0) return ""
    for(var i = 0, length = strs[0].length; i < length; i++) {
        var c = strs[0].charAt(i)
        for(var j = 1; j < strs.length; j++) {
            if(i === strs[j].length || strs[j].charAt(i) !== c) {
                return strs[0].slice(0, i)
            }
        }
    }
    return strs[0]
};
```

### 解题思路

- 与方法1不同，该方法先试探数组内第一个字符串的第一个元素即 `strs[0].chatAt(0)` 是否为公共前缀
- 以 `i` 为循环变量遍历第一个字符串元素 `c`:
  - 以 `j` 为循环变量遍历从第二个字符串开始的所有字符串：
    - 如果 `i` 等于字符串长度或者字符串 `j` 位置上元素不等于 `c` 。第一个字符串 [0,i) 的子串即为最长公共前缀
    - 如果字符串 `j` 位置上元素等于 `c` ，`i++` 继续遍历

### 注意事项

- `Array.prototype.slice(start, end)` 获取的元素区间为 [start, end)

### 代码实现3

```js
var longestCommonPrefix = function (strs) {

  function getCommonStr(strL, strR) {
    for (let i = 0, length = strL.length; i < length; i++) {
      if (i === strR.length || strL[i] !== strR[i]) {
        return strL.slice(0, i)
      }
    }
    return strL
  }

  function getLCP(strs) {
    if (strs === null || strs.length === 0) {
        return 
    }  
    if (strs.length === 1) {
      return strs[0]
    }
    if (strs.length === 2) {
      let strL = strs[0],
        strR = strs[1]
      return getCommonStr(strL, strR)
    }
    let mid = Math.floor(strs.length / 2),
      leftStr = getLCP(strs.slice(0, mid)),
      rightStr = getLCP(strs.slice(mid, strs.length))

    return getCommonStr(leftStr, rightStr)
  }

  return getLCP(strs)
};
```

### 解题思路

- getLCP(strs) = getLCP(getLCP(strsLeft), getLCP(strsRight)) 递归

## 6. 有效的括号

### 题目描述

> 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
>
> 有效字符串需满足：
>
> 左括号必须用相同类型的右括号闭合。
> 左括号必须以正确的顺序闭合。
> 注意空字符串可被认为是有效字符串。
>
> 示例 1:
>
> 输入: "()"
> 输出: true

### 代码实现

```js
var isValid = function(s) {
    if(s === null || s.length === 0) {
        return true
    }
    if(s.length % 2 !== 0 || ['}', ')', ']'].indexOf(s) !== -1) {
        return false
    }
    let arr = [],
        rule = {
            '}': '{',
            ']': '[',
            ')': '('
        }
    for(let i = 0, length = s.length; i < length; i++) {
        const element = s[i]
        if(arr.length === 0) {
            arr.push(element)
            continue
        }
        if(rule[element] === arr[arr.length - 1]) {
            arr.pop()
        }else {
            arr.push(element)
        }
    }
    return arr.length === 0 
};
```

### 解题思路

- 使用栈辅助，以 i 为循环变量遍历字符串:
  - 如果栈为空，`str[i]` 进栈
  - 如果栈不为空，栈顶元素与 `str[i]` 如果相同，出栈，如果不同，`str[i]` 进栈
- 遍历结束后，如果栈为空，则匹配，否则不匹配 

### 注意事项

- 注意遍历未结束栈为空时下一个元素直接入栈
- 长度为奇数和以反括号开头的元素必定不匹配，可事先排除

## 7. 合并两个有序链表※

### 题目描述

> ​	将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。  
>
> **示例：**
>
> ```js
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> ```

### 代码实现1

```js
var mergeTwoLists = function(l1, l2) {
    if(l1 === null){
        return l2
    }
    if(l2 === null){
        return l1
    }
    if(l1.val < l2.val){
        l1.next = mergeTwoLists(l1.next, l2)
        return l1
    }else{
        l2.next = mergeTwoLists(l1, l2.next)
        return l2
    }
};
```

### 解题思路

- 使用递归:
  - `list1[0] + merge(list1[1: ], list2) 当 list1[0] < list2[0]`
  - 结束条件为: 有一个链表为空
  - 每层都取出一个最小值作为链表头，将剩下的值继续递归，最后返回排序好的链表头

### 代码实现2

```js
var mergeTwoLists = function(l1, l2) {
    if(l1 === null) {
        return l2
    }
    else if(l2 === null) {
        return l1
    }

    let head,
        prevNode
    if(l1.val < l2.val) {
        head = l1
        prevNode = l1
        l1 = l1.next
    }else{
        head = l2
        prevNode = l2
        l2 = l2.next
    }    

    while(l1 !== null && l2 !==null){
        if(l1.val < l2.val){
            let temp1 = prevNode.next,
                temp2 = l1.next
            prevNode.next = l1
            l1.next = temp1
            prevNode = l1
            l1 = temp2
        }else{
            let temp1 = prevNode.next,
            temp2 = l2.next
            prevNode.next = l2
            l2.next = temp1
            prevNode = l2
            l2 = temp2
        }
    }    

    if(l1 === null){
        prevNode.next = l2
    }else{
        prevNode.next = l1
    }

    return head
};
```

### 解题思路

- 当有一个链表为空时，直接返回另一个链表
- 定义辅助变量 `head` 和 `prevNode`
- 比较两个链表的第一个元素:
  - 将较小的值作为要返回的链表头并赋值给辅助变量 `head`
  - 初始化 `prevNode` 为 `head`
- 当两个链表都不为空时迭代:
  - 控制 `l1` 与 `l2` 让其始终指向各自链表的第一个未迭代元素
  - 比较两个链表当前元素, 将较小的元素链接到 `prevNode` 之后，并修改 `prevNode` 的值为这个元素，为下一次迭代做准备
- 迭代完成后，将不为空的链表接在 `prevNode` 之后即可

### 注意事项

- 合理定义辅助变量:
  - `head` 是为了方便返回最终链表
  - `prevNode` 是插入元素时使用，单链表不便于在一个元素前插入元素
- 及时绑定和及时解绑
  - 时刻注意维护 `prevNode` 的指向，初始化为将返回链表的表头，并在下一次迭代前务必更新 `prevNdode` 的值
  - 时刻注意维护 `l1` 和 `l2` 的指向，将其始终指向链表将要迭代的值



## 8. 删除排序数组中的重复项

### 题目描述

> 给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
>
> 示例 1:
>
> 给定数组 nums = [1,1,2], 
>
> 函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
>
> 你不需要考虑数组中超出新长度后面的元素。
>

### 代码实现

```js
var removeDuplicates = function(nums) {
  if(nums === null) return 0
  let slow = 0, 
      fast = 1
  while(fast < nums.length) {
      if(nums[slow] === nums[fast]) {
          fast++
      }else{
          nums[slow + 1] = nums[fast]
          slow = slow + 1
      }
  }    
  return slow + 1
};
```

### 解题思路

慢指针指定元素，快指针搜索重复元素。

- 使用一个快指针`fast` 和 一个慢指针`slow` , 并初始化 `slow = 0` ，`fast = 1`
- 当 `fast < nums.length`  时循环：
  - 如果快指针指向的值与慢指针指向的值相同，快指针向后移动
  - 如果快指针与慢指针指向的值不同，把快指针指向的值赋值到慢指针的下一个位置并把慢指针向后移动一位

### 注意事项

- 返回的是数组长度，是索引加一，即 `slow + 1`

## 9. 删除数组指定元素

### 题目描述

> 给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
>
> 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
>
> 示例 1:
>
> 给定 nums = [3,2,2,3], val = 3,
>
> 函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
>
> 你不需要考虑数组中超出新长度后面的元素。
>

### 代码实现1

```js
var removeElement = function(nums, val) {
    let slow = nums.indexOf(val),
        fast = slow + 1
    if(slow === -1) {
        return nums.length
    }    
    while(fast < nums.length) {
        if(nums[fast] !== val) {
            nums[slow] = nums[fast]
            slow++
        }
        fast++
    }
    return slow 
};
```



### 解题思路

慢指针决定要替换的位置，快指针决定要替换的值

- 先找到数组中第一个 val 的位置，如果位置不存在返回原数组的长度，如果存在令慢指针该位置，快指针指向该位置的下一个位置
- 遍历数组，直到快指针指向一个不等于 `val` 的值，将其赋值给慢指针所在位置。慢指针指向下一个位置，快指针指向下一个位置，继续遍历

### 注意事项

- `slow` 指针指的是将要被替换的位置，结果数组长度应该为 `slow - 1 + 1` 即 `slow`

### 代码实现2

```js
var removeElement = function(nums, val) {
    let i = 0,
        n = nums.length
   while(i < n) {
       if(nums[i] === val) {
           nums[i] = nums[n - 1]
           n--
       }else{
           i++
       }
   }
   return n 
};
```

### 解题思路

- 遍历数组，如果碰到与 `val` 相同的元素，将数组最后一个元素赋值给 val， 数组长度减 1

### 注意事项 

- 当把数组最后一个值赋值给前面的元素时，`i` 不能加一，因为这个数组最后一个值也可能是要被删除的元素

## 10. 实现 IndexOf()

### 题目描述

> 实现 IndexOf() 函数。
>
> 给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。
>
> 示例 1:
>
> 输入: haystack = "hello", needle = "ll"
> 输出: 2

### 代码实现

```js
var strStr = function(haystack, needle) {
    if(needle === '') {
        return 0
    }
    if(haystack.length < needle.length) {
        return -1
    }
    let i = 0,
        n = haystack.length - needle.length 
    while( i <= n) {
        if(haystack[i] === needle[0]) {
            if(haystack.substring(i, i + needle.length) === needle) {
                return i
            }
        }
        i++
    }
    return -1
};
```

### 解题思路

- 先找出 `haystack` 中与 `needle` 首个字符匹配的位置
- 从该位置开始截取字串，比较字串与 `needle`

### 注意事项

- 循环的结束条件为 `haystack`的当前遍历位置加 `needle` 长度小于等于 `haystack` 长度，即 `i <= haystack.length - needle.length`

## 11. 搜索插入位置

### 题目描述

> ​	给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
> 你可以假设数组中无重复元素。
>
> **示例 1:**
>
> ```js
> 输入: [1,3,5,6], 5
> 输出: 2
> ```

### 代码实现

```js
var searchInsert = function(nums, target) {
   let left = 0,
       right = nums.length - 1
    while(left <= right) {
        let mid = Math.floor((left + right) / 2)
        if(nums[mid] === target){
            return mid
        }
        if(nums[mid] < target) {
            left = mid + 1
        }else{
            right = mid - 1
        }
    }
    return left
};
```



### 解题思路

- 二分查找

### 注意事项

- 如果没有找到相等的值， left 即为该插入的位置，因为 left 是第一个大于 target 的位置 (如果不大于就会继续二分查找)

- 二分查找的模板

  ```js
  function divideSearch(nums, target) {
    let left = 0,
      right = nums.length - 1 // right 是 num.length - 1
  
    while (left <= right) { // left === right 时也进入循环
      let mid = Math.floor((left + right) / 2)
  
      if (nums[mid] === target) {
        return mid
      }
  
      if (nums[mid] < target) {
        left = mid + 1 // 二分是  mid + 1
      } else {
        right = mid - 1 // 二分是 mid -1
      }
    }
    return -1
  }
  ```

## 12. 外观数列

### 题目描述

> ​	1, 11, 21, 1211, 111221, 312211, 13112221, 1113213211, ……
>
> 它以数字1开始，序列的第n+1项是对第n项的描述。比如第2项是2个1，所以下一项（第三项）就是21。又比如第5项是111221，描述就是3个1，2个2，1个1， 所以下一项就是312211。

### 代码实现

```js
var countAndSay = function(n) {
    if(n === 1) {
        return '1'
    }
    let str = countAndSay(n - 1),
        slow = 0,
        fast = 1,
        result = ''
    while(fast < str.length) {
        if(str[fast] === str[slow]) {
            fast++
        }else{
            result += (fast - slow) + str[slow]
            slow = fast
            fast++
        }
    }    
    result += (fast - slow) + str[slow]

    return result
    
};
```

### 解题思路

- 递归 + 双指针
- slow 表示字符，fast 寻找与 slow 相等的字符， fast -slow 为重复字符次数

### 注意事项

- 出 while 循环时，因为最后一个字符描述没有放入到 result， 所以还需要再累加一次

### 代码实现2

```js
var countAndSay = function(n) {
    if(n === 1) {
        return '1'
    }
    let str = countAndSay(n - 1)
    const chars = str.match(/(\d)\1*/g)
    return chars.reduce((accumulator, curr) => {
        return accumulator + curr.length + curr[0]
    }, "")
    
};
```



### 解题思路

- 使用 `String.prototype.match()` 分割数组
- 使用  `Array.prototype.reduce()` 累加获得结果

### 注意事项

- 正则表达式 中`()` 表示分组 ，`\n` 获取分组 ，`* ` 重复0次或多次，`?` 重复0次或1次，`+` 重复0次或1次

## 13.  最大子序和

### 题目描述

>  给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。 
>
> **示例:**
>
> ```js
> 输入: [-2,1,-3,4,-1,2,1,-5,4],
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
> ```

### 代码实现1

```js
var maxSubArray = function(nums) {
    let result = nums[0],
        sum = 0
    for(let i = 0, length = nums.length; i < length; i++) {
        const element = nums[i]
        if(sum > 0) {
            sum += element
        } else {
            sum = element
        }
        result = Math.max(result, sum)
    }
    return result 

};
```

动态规划法, 使得每一步结果都为最优

- result 存储最大子序和, sum 为辅助变量
- 如果 sum > 0 说明有增益可以继续往后加
- 如果 sum <= 0 说明无增益，放弃该子序

### 注意事项

- 在每一次循环后都比较 result 与 sum，及时存储最大子序，而不是整个数组遍历后 。因为最大子序和 (sum取最大值时) 与能作为下一个子序和的元素 (sum > 0 ) 判定不一样

### 代码实现2

```js
var maxSubArray = function(nums) {

function getMaxCross(nums, left, right) {
    if(left === right) {
        return nums[left]
    }

    let p = Math.floor((left + right) / 2)
      let maxLeftCrossSub = -Infinity,
          maxRightCrossSub = -Infinity,
        sum = 0 
    for (let i = p; i >= left; i--) {
        sum += nums[i]
        maxLeftCrossSub = Math.max(maxLeftCrossSub, sum)
    }

    sum = 0
    for (let i = p + 1; i <= right; i++) {
        sum+= nums[i]
        maxRightCrossSub = Math.max(maxRightCrossSub, sum)
    }

    return maxLefCrossSub + maxRightCrossSub

}  

function getMaxSub(nums, left, right) {
    if(left === right) {
        return nums[left]
    }
    let p = Math.floor((left + right) / 2)
    let maxLeftSub = getMaxSub(nums, left, p)
    let maxRightSub = getMaxSub(nums, p + 1, right)
    let maxCrossSub = getMaxCross(nums, left, right)
    return Math.max(maxLeftSub, maxRightSub, maxCrossSub)
}

return getMaxSub(nums, 0, nums.length - 1)

};
```



### 解题思路

- 递归求解 `maxLeftSub` 、 `maxRightSub` 、`maxCrossSub`，再取其中最大值
- `maxLeftSub` 指从数组开头到中部的最大子序列
  - 分解数组，只剩下一个元素的时候即为初始最大右子序列
- `maxRightSub` 指从数组中部到末尾的最大子序列
  - 分解数组，只剩下最后一个元素的时候即为初始最大右子序列
- `maxCrossSub` 指从数组中部到两边的最大子序列
  - 从中间向左边的最大子序列 `maxLeftCrossSub` 与从中间向右边的最大子序列 `maxRightCrossSub` 的和

## 14. 最后一个单词长度

### 题目描述

> 给定一个仅包含大小写字母和空格 ' ' 的字符串 s，返回其最后一个单词的长度。
>
> 如果字符串从左向右滚动显示，那么最后一个单词就是最后出现的单词。
>
> 如果不存在最后一个单词，请返回 0 。
>
> 说明：一个单词是指仅由字母组成、不包含任何空格的 最大子字符串。
>
> 示例:
>
> 输入: "Hello World"
> 输出: 5

### 代码实现

```js
var lengthOfLastWord = function(s) {
    s = s.trim()
    if (s.length === 0) {
        return 0
    }
    const position = s.lastIndexOf(' ')
    if(position === -1) {
        return s.length
    } else{
        return s.length - position - 1
    }
};
```

### 解题思路

- 去除字符串首位的空格
- 如果字符串为空串， 返回 0
- 找到最后一个空格的位置 position，s.length - position - 1 即为所求

### 注意事项

- 注意几种特殊情况
  - 空串
  - 末尾有空格

## 15. 加一

### 题目描述

> 给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
>
> 最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
>
> 你可以假设除了整数 0 之外，这个整数不会以零开头。
>
> 示例 1:
>
> 输入: [1,2,3]
> 输出: [1,2,4]
> 解释: 输入数组表示数字 123。

### 代码实现

```js
var plusOne = function(digits) {
    let i = digits.length - 1
    if (digits[i] !== 9) {
        digits[i] += 1
    } else {
        while(digits[i] === 9 && i >= 0) {
            digits[i] = 0
            i--
        }
        if(i >= 0 ) {
            digits[i] += 1
        }else{
            digits.unshift(1)
        }
    }
    return digits
};
```

### 解题思路

- 最后一位不为 9 ， 直接加 1 ，返回
- 最后一位为 9， 赋值为0，向前移动直到找到第一个不为 9  的位置
- 如果这个位置不小于 0 ，加一即可，否则向数组头部添加一个 1

### 注意事项

重点考虑两种情况

- nnn99…
- 9999…

## 16. 二进制求和

### 题目描述

> 给定两个二进制字符串，返回他们的和（用二进制表示）。
>
> 输入为非空字符串且只包含数字 1 和 0。
>
> 示例 1:
>
> 输入: a = "11", b = "1"
> 输出: "100"

### 代码实现

```js
var addBinary = function (a, b) {
  let relativeNum1 = (a.length > b.length? a: b).split("").map(item => +item),
      relativeNum2 = (a.length > b.length? b: a).split("").map(item => +item),
      i = relativeNum1.length - 1,
      k = relativeNum2.length - 1
      for(let n = 0; n < i - k; n++) {
          relativeNum2.splice(0, 0, 0)
      }
  while (i >=0) {
    const bit1 = relativeNum1[i],
          bit2 = relativeNum2[i]
    switch (bit1) {
      case 0:
        relativeNum1[i] = bit2
        i--
        break
      case 1:
        if(bit2 === 1) {
            relativeNum1[i] = 0
            if(i - 1 >= 0) {
              relativeNum1[i - 1] = relativeNum1[i - 1] + 1
            } else {
                relativeNum1.unshift(1)
            }
        }
        i--
        break
      case 2:
        relativeNum1[i] = bit2 === 0? 0: 1
        if(i - 1 >= 0) {
        relativeNum1[i - 1] = Number(relativeNum1[i - 1]) + 1
        } else {
          relativeNum1.unshift(1)
        }
        i--
    }
  }

  return relativeNum1.join("")
};
```

### 解题思路

- 统一两个数的位数，短位数字在前面补 0
- 从低位到高位相加

### 注意事项 

- JavaScript 可以使用 `parseInt(num, n).toString(m)` 实现对 num 从进制 `n` 到 进制 `m` 的转换，但是对数有长度要求

## 17. x 的平方根

### 题目描述

> 实现 int sqrt(int x) 函数。
>
> 计算并返回 x 的平方根，其中 x 是非负整数。
>
> 由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。
>
> 示例 1:
>
> ```js
> 输入: 8
> 输出: 2
> 说明: 8 的平方根是 2.82842..., 
>      由于返回类型是整数，小数部分将被舍去
> ```

### 代码实现

```js
var mySqrt = function(x) {
    if(x < 2) {
        return x
    }
    let left = 0, right = x / 2
    while(left <= right) {
        let mid = Math.floor((left + right) / 2)
        if(mid * mid > x) {
            right = mid - 1
        }else{
            left = mid + 1 
        }
    }
    return left - 1
};
```

### 解题思路

- 二分查找法

### 注意事项

- 返回值是 left - 1，因为 left 是第一个平方大于 x 的数字



## 18. 爬楼梯

### 题目描述

> 
> 假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。
>
> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
>
> **注意：**给定 *n* 是一个正整数。
>
> **示例 1：**
>
> ```js
> 输入： 2
> 输出： 2
> 解释： 有两种方法可以爬到楼顶。
> 1.  1 阶 + 1 阶
> 2.  2 阶
> ```

### 代码实现

```js
var climbStairs = function(n) {
    const arr = [1, 2]
    for (var i = 2; i < n; i++) {
        arr[i] = arr[i - 1] + arr[i - 2]
    }
    return arr[n - 1]
};
```

### 解题思路

- 斐波那契数列，不用递归是因为递归超时
- 爬楼梯问题是斐波那契数列是因为倒着看，比如要爬到第 10 个台阶，倒数第二步必然在第 8 个台阶或者第 9 个台阶上， 所以共有 climbStairs(8) + climbStairs(9) 种办法

## 19. 删除排序链表的重复元素

### 题目描述

> 给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
>
> **示例 1:**
>
> ```js
> 输入: 1->1->2
> 输出: 1->2
> ```

### 代码实现

```js
var deleteDuplicates = function(head) {
    if (head === null) {
        return null
    } 
    let slow = head,
        fast = slow.next
    while(slow.next !== null) {
      if(fast.val === slow.val) {
          slow.next = fast.next
      }else{
          slow = slow.next
      }
      fast = fast.next
    }
    return head
};
```

### 解题思路

- 双指针

## 20. 合并两个有序数组

### 题目描述

> 给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。
>
> 说明:
>
> 初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
> 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
> 示例:
>
> 输入:
> nums1 = [1,2,3,0,0,0], m = 3
> nums2 = [2,5,6],       n = 3
>
> 输出: [1,2,2,3,5,6]
>

### 代码实现

```js
var merge = function(nums1, m, nums2, n) {
    let i = m - 1,
        k = n - 1
        index = m + n - 1
    while(i >= 0 && k >= 0) {
        nums1[index--] = nums1[i] > nums2[k]? nums1[i--]: nums2[k--]
    }    
    while(k >= 0) {
        nums1[index--] = nums2[k--]
    }
};
```



### 解题思路

- 从后往前遍历两个数组元素，把较大的放到合并后的数组最后面

### 注意事项

- 从后往前遍历比从前往前遍历节省空间
- 遍历完后如果 nums2 有剩余记得合并到数组

## 21. 相同的树

### 题目描述

> 给定两个二叉树，编写一个函数来检验它们是否相同。
>
> 如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。
>
> 示例 1:
>
> 输入:       1         1
>                / \       / \
>              2   3     2   3
>
> 输出  [1,2,3],   [1,2,3]

### 代码实现

```js
var isSameTree = function(p, q) {
   if(p === null && q === null) {
       return true
   }
   if(p === null || q === null) {
       return false
   }
   if(p.val !== q.val) {
       return false
   }
   return isSameTree(p.left, q.left) && isSameTree(p.right, q.right)
};
```

### 解题思路

树的前序遍历

### 注意事项

- 如果 p 和 q 都为 null ，返回 true
-  再如果 p 或 q 有一个为 null  返回false

## 22. 对称二叉树

### 题目描述

> 给定一个二叉树，检查它是否是镜像对称的。
>
> 例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
>
> ```js
>     1
>    / \
>   2   2
>  / \ / \
> 3  4 4  3
> ```

### 代码实现1

```js
var isSymmetric = function(root) {
    return isSame(root, root)
    
    function isSame(leftTree, rightTree) {
        if(leftTree === null && rightTree === null) {
            return true
        }
        if(leftTree == null || rightTree == null) {
            return false
        }
        if(leftTree.val !== rightTree.val) {
            return false
        }
        return isSame(leftTree.left, rightTree.right) && 
               isSame(leftTree.right, rightTree.left)
    }
};
```

### 解题思路

两棵树为对称二叉树的条件是:

- 根节点相同
- 每个树的左子树与另一个树的右子树相同
- 每个树的右子树与另一个树的左子树相同

### 注意事项

注意跳出循环的条件:

- 左子树与右子树同为 null ，返回 true；只有一个为 null 返回 false
- 任意左子树与右子树不为镜像，返回 false

### 代码实现2

```js
var isSymmetric = function(root) {
    let nodes = []
    nodes.push(root, root)
    while(nodes.length > 0) {
        let leftTree = nodes.shift()
        let rightTree = nodes.shift()
        if(leftTree === null && rightTree === null) {
            continue
        }
        if(leftTree === null || rightTree === null) {
           return false
        } 
        if(leftTree.val !== rightTree.val) {
            return false
        }
        nodes.push(leftTree.left, rightTree.right,
                   leftTree.right, rightTree.left)
    }
    return true
};
```

### 解题思路

- 定义一个队列 nodes， 初始化对列元素为两个根节点，当队列不为空时，循环
  - 每次取出队首两个元素 `leftTree` 和 `rightTree`
  - 比较这两个元素，如果只有一个为 null 或者两个值不同返回 false
  - 如果两个元素都为 null，进行下一次循环；否则在队首依次添加 4 个元素，分别是: `leftTree.left` 、`rightTree.right`、`leftTree.right`、 `rightTree.left`
- 如果直到循环结束都没返回值则返回 true

### 注意事项

- 每次在队首弹出两个元素， 但是在队尾添加了四个元素，所以循环条件为队列不为空
- 当两个元素都为 null 时  `continue` 循环而不是直接返回，因为这两个元素可能是上一次新添加的前两个元素，后两个元素仍待比较
- 如果直到循环结束都没返回值，说明整个树都是镜像的，返回 true

## 23. 二叉树的最大深度

### 题目描述

> 给定一个二叉树，找出其最大深度。
>
> 二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
>
> **说明:** 叶子节点是指没有子节点的节点。
>
>  **示例：**
> 给定二叉树 `[3,9,20,null,null,15,7]`， 
>
> ```js
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
>  返回它的最大深度 3 。 

### 代码实现1

```js
var maxDepth = function(root) {
    if (root === null) {
        return 0
    }
    let leftMax = maxDepth(root.left),
        rightMax = maxDepth(root.right)
    return Math.max(leftMax, rightMax) + 1       
};
```

### 解题思路

二叉树的最大深度是取左树最大深度与右树最大深度的最大值

### 注意事项

可以不使用变量在返回结果中加 1 来累加

### 代码实现2

```js
var maxDepth = function (root) {
  if (root === null) {
    return 0
  }
  let depth = 0,
    nodes = [root]
  while (nodes.length > 0) {
    let len = nodes.length
    while (len > 0) {
      const node = nodes.shift()
      if (node.left !== null) {
        nodes.push(node.left)
      }
      if (node.right !== null) {
        nodes.push(node.right)
      }
      len--
    }
    depth++
  }
  return depth
};
```

### 解题思路

- 使用一个队列 nodes，初始化元素为 root, 当 nodes 不为空时循环
  - 每层循环再循环 nodes.length 次，即对队列中每一个元素执行一下操作
    - 取出队首元素，如果该元素的左右子树存在，则将其左右子树放入队列

---



## n. 模板

### 题目描述

> ​	1

### 代码实现

```js

```



### 解题思路



### 注意事项



****