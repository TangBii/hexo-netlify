## 1. 数组

---

### [11. 盛水最多的容器](https://leetcode-cn.com/problems/container-with-most-water/ )

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

  ![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg) 



图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

**思路**

> 1. 容纳水的体积由 x 和 y 共同确定
> 2. 先选择最边缘的两根柱子,此时 x 最大
> 3. 把 y 较小的柱子向内移动, 直到左右边界相遇

```js
var maxArea = function(height) {
    let l = 0,
        r = height.length - 1,
        max = 0
    while (l < r) {
        // 判断出小的 y 以后,直接移动柱子,可简化代码,但是注意算 x 时要加回来
        const minHeight = height[l] < height[r]? height[l++]: height[r--]
        max = Math.max(max, minHeight * (r - l + 1))
    }
    return max
};
```

### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明:

必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。

**思路**

> 1. 使用 zeroIndex 记录下一个存放非 0 元素的位置
> 2. 遍历数组, 如果当前元素不为 0 则存放到 zeroIndex 的位置,
> 3. 如果存放非 0 元素的位置与当前遍历元素的位置不同, 当前元素赋值为 0
> 4. zeroIndex++

```js
var moveZeroes = function(nums) {
    let zeroIndex = 0
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            nums[zeroIndex] = nums[i]
            nums[i] = zeroIndex === i? nums[i]: 0
            zeroIndex++
        }
    }
};
```

### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

**思路**

> $f(n) = f(n - 1) + f(n - 2)$ 即斐波那契数列
>
> 1. 直接使用递归有大量的重复计算,导致效率低下,需要记忆化
> 2. 本题不需记录完整数列, 只需记录$f(n - 1)$和$f(n - 2)$即可

#### 递归版本

```js
var climbStairs = function(n, cache = []) {
    if (n < 3) {
        return n 
    }
    // 使用 cache 记忆化, 递归调用时需要传入 cache
    const n_1 = cache[n - 1]? cache[n - 1]: climbStairs(n - 1, cache),
          n_2 = cache[n - 2]? cache[n - 2]: climbStairs(n - 2, cache)

    return cache[n] = n_1 + n_2
};
```

#### 非递归版本

```js
var climbStairs = function(n) {
    if (n < 3) {
        return n
    }
    // 只需记录 prev1 和 prev 2 即可
    let result = 0,
        prev1 = 2,
        prev2 = 1
    for (let i = 3; i <= n; i++) {
        result = prev2 + prev1
        prev2 = prev1
        prev1 = result
    }
    return result 
};
```

#### 非递归至简版本

```js
var climbStairs = function(n) {
    let a = 1,
        b = 1
    while (n--) {
        // b = a + b, a = b
        a = (b += a) - a
    }
    return a
};
```



### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

 **示例：** 

给定数组 nums = [-1, 0, 1, 2, -1, -4]，满足要求的三元组集合为: [   [-1, 0, 1]  ,   [-1, -1, 2]   ]

**思路**

> 1. 先把数组排序,  **需自己定义比较器, js 自带的比较器是比较字符值**
> 2. $a + b + c = 0 $ 可以转换为$a + b = -c$ , 这样可把三数之和转换为两数之和
> 3. 使用双指针, 和大于目标值左指针右移, 和小于目标值左指针右移
> 4. 需要在循环过程中进行一些判断去重

#### 暴力版

```js
// 注意暴力遍历的方式    
for (let i = 0; i < nums.length - 2; i++) {
        for (let j = i + 1; j < nums.length - 1; j++) {
            for (let k = j + 1; k < nums.length; k++) {
                if (nums[i] + nums[j] + nums[k] === 0) {
                    result.push([nums[i], nums[j], nums[k]])
                }
            }
        }
    }
```

#### 双指针版

```js
var threeSum = function(nums) {
    if (!nums || nums.length < 3) {
        return []
    }
    // 先排序
    nums = nums.slice().sort((a, b) => a - b)
    const result = []
    for (let i = 0; i < nums.length - 2; i++) {
        // 如果 nums[i] > 0 则肯定不存在 a + b = -c
        if (nums[i] > 0) {
            break
        }
        // 去重
        if (i > 0 && nums[i] === nums[i - 1]) {
            continue
        }
        // 双指针，需自己控制增减，所以使用 while 循环比 for 好
        let l = i + 1,
            r = nums.length - 1
        while (l < r) {
            const target = 0 - nums[i],
                  sum = nums[l] + nums[r]
            if (sum < target) {
                l++
            } else if (sum > target) {
                r--
            } else {
                result.push([nums[i], nums[l], nums[r]])
                //  去重
                while (nums[l + 1] === nums[l]) l++
                while (nums[r - 1] === nums[r]) r--
                // 还需要再增减一次
                l++, r--
            }
        }
    }
    return result
};
```

### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

给定一个**排序**数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

**思路**

> 1. 使用 len 表示下一个非重复项应存放的位置, 初始化为 1, 因为默认索引为 0 的位置已存入
> 2. 从索引为 1 的位置开始遍历, 比较当前元素与前一个元素, 如果不同则放入 nums[len]

```js
var removeDuplicates = function(nums) {
    // 默认 0 已放入, 所以初始化为 1
    let len = 1
    // 从 1 开始遍历
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] !== nums[i -1]) nums[len++] = nums[i] 
    }
    return len
};
```

### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

说明:

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。

**思路**

> 从后往前合并

```js
var merge = function(nums1, m, nums2, n) {
    // 给的都是长度, 初始化最后一个索引,便于操作
    let i = m - 1,
        j = n - 1,
        len = m + n - 1
    while (i >= 0 && j >= 0) {
        nums1[len--] = nums1[i] > nums2[j]? nums1[i--]: nums2[j--]
    }
    while (j >= 0) {
        nums1[len--] = nums2[j--]
    }
};
```

### [66. 加一](https://leetcode-cn.com/problems/plus-one/)

给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1:

输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。

**思路**

> 1. 从后往前加
> 2. 如果当前数字不为 9 , 当前数字加一, 返回结果即可; 否则当前数字赋值为 0 , 继续向前判断
> 3. 如果直到第一个数字都没出现非 9 的数字, 则在数组头部添加 1, 返回结果

```js
var plusOne = function(digits) {
    for (let i = digits.length - 1; i >= 0; i--) {
        if (digits[i] !== 9) {
            digits[i] += 1
            return digits
        }
        digits[i] = 0
    }
    digits.unshift(1)
    return digits
};
```

### [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

示例 1:

输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]

**注意先处理 k, k %=  nums.length**

#### 暴力法

> 1. 存储数组最后一个元素
> 2. 数组所有元素后移一位
> 3. 把存储的元素放到数组头

```js
var rotate = function(nums, k) {
    k = k % nums.length
    for (let i = 0; i < k; i++) {
        const last = nums[nums.length - 1]
        for (let j = nums.length - 1; j > 0; j--) {
            nums[j] = nums[j - 1]
        }
        nums[0] = last
    }
};
```

#### 分段旋转法

> 1. 整个数组反转
> 2. 反转前 k 个元素
> 3. 反转之后的元素

```js
var rotate = function(nums, k) {
    k %= nums.length
    function reverse(nums, left, right) {
        while (left < right) {
            const temp = nums[right]
            nums[right] = nums[left]
            nums[left] = temp
            left++, right--
        }
    }
    reverse(nums, 0, nums.length - 1)
    reverse(nums, 0, k - 1)
    reverse(nums, k, nums.length - 1)
};
```

#### 循环旋转法

> 1. 从第一个元素开始旋转
> 2. 存储被替换的元素
> 3. 进行下一次旋转

```js
var rotate = function(nums, k) {
    k %= nums.length
    if (!k || !nums || nums.length < 2) {
        return 
    }
    // 计数
    let count = 0
    for (let i = 0; i < nums.length; i++) {
        // prev 前驱, target 后继, flag 是否循环到前驱元素 
        let prev = nums[i]
            target = (i + k) % nums.length,
            flag = true
        while (flag) {
            // 如果循环到了前驱元素, 再交换一次, 跳出循环
            if (target === i) {
                flag = false
            }
            // 交换前驱和后继
            const temp = nums[target]
            nums[target] = prev
            prev = temp
            target = (target + k) % nums.length
            count++
            if (count === nums.length) {
                return 
            }
        }
    }
};
```



## 2. 链表

---

### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

反转一个单链表。

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

#### 递归

```js
var reverseList = function(head) {
    if (!head || !head.next) {
        return head
    }
    const newHead = reverseList(head.next)
    // head 的下一个节点的 next 指向 head
    head.next.next = head
    // head 的下一个节点为空
    head.next = null
    return newHead
};
```

#### 迭代

```js
var reverseList = function(head) {
    if (!head || !head.next) {
        return head
    }
    let prev = null
    while (head) {
        const temp = head.next
        head.next = prev
        prev = head
        head = temp
    }
    return prev
};  
```

### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例:

给定 1->2->3->4, 你应该返回 2->1->4->3.

**思路**

> 1. 每次交换涉及四个节点: 要交换的两个节点， 两个节点的前驱节点， 两个节点的后继节点
> 2. 前驱节点的 next 指向第二个节点
> 3. 第一个节点的 next 指向后继节点
> 4. 第二个节点的 next 指向第一个节点

#### 迭代

```js
var swapPairs = function(head) {
    if (!head || !head.next) {
        return head
    }
    // 设置一个虚拟节点， 很便于操作
    dummyNode = new ListNode(-1)
    dummyNode.next = head
    let before = dummyNode
    // 判断需要交换的两个节点是否存在
    while (before.next && before.next.next) {
        let first = before.next,
            second = before.next.next
            after = second.next
        before.next = second	// 前驱节点的 next 指向第二个节点
        first.next = after		// 第一个节点的 next 指向后继节点
        second.next = first		// 第二个节点的 next 指向第一个节点
        before = first			// 更新前驱节点
    }
    return dummyNode.next		// 返回虚拟节点的 next
};
```

#### 递归

```js
var swapPairs = function(head) {
    if (!head || !head.next) {
        return head
    }
    const second = head.next
    head.next = swapPairs(second.next)	// 第一个节点的 next 指向反序好的头节点
    second.next = head	// 第二个节点的 next 指向头节点
    return second
};
```

### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

示例：

给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

说明：

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

**思路**

> 1. 每次变换涉及 k + 2 个节点： 要反转的 k 个节点 , k 个节点的前驱节点， k 个节点的后继节点
> 2. 通过循环找到一组节点的最后一个节点 last
> 3. 反转 (start, last)**记得断开 last 与之后节点的连接， 防止被一起反转了**
> 4. 前驱节点的 next 指向 last， first 的 next 指向后继节点

#### 迭代

```js
var reverseKGroup = function(head, k) {
    if (!head || !head.next) {
        return head
    }
    // 虚拟头节点， 辅助使用
    const dummyNode = new ListNode(-1)
    dummyNode.next = head
    let before = dummyNode
    while (before) {
        // 获得每组的最后一个节点
        let last = before
        for (let i = 0; i < k; i++) {
            last = last.next
            if (!last) {
                return dummyNode.next
            }
        }
        // 获得后继节点
        const after = last.next
        // 务必记得断开连接！
        last.next = null
        const first = before.next
        before.next = reverse(before.next)
        first.next = after
        before = first
    }
    return dummyNode.next

    function reverse(head) {
        if (!head || !head.next) {
            return head
        }
        const newHead = reverse(head.next)
        head.next.next = head
        head.next = null
        return newHead
    }
};
```

#### 递归

```js
var reverseKGroup = function(head, k) {
    const dummyNode = new ListNode(-1)
    dummyNode.next = head
    return help(dummyNode)

    function help (before) {
        // 如果节点个数少于 k 返回
        let last = before
        for (let i = 0; i < k; i++) {
            last = last.next
            if (!last) {
                // 注意返回值是 before.next
                return before.next
            }
        }
        const first = before.next,
              after = last.next

        // 断开连接
        last.next = null
        reverse(first)
        before.next = last
        // 恢复连接
        first.next = after
        first.next = help(first)
        // 注意返回值是 before.next
        return before.next
    }

    function reverse(head) {
        if (!head || !head.next) {
            return head
        }
        const newHead = reverse(head.next)
        head.next.next = head
        head.next = null
        return newHead
    }
};
```

### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

**思路**

> 1. 快指针每次走两步，慢指针每次走一步
> 2. 快慢指针如果能相遇，说明链表中有环
>
> ---
>
> 原理: 
>
> ​	以慢指针为参考系，慢指针静止， 快指针每次跑一步，所以如果有环，它们必定会相遇
>
> 可以分为 :
>
> ​		A情况: 快指针走过一圈后落后慢指针一步, 则下一次迭代快慢指针相遇
>
> ​		其他情况。其他情况经过若干次迭代会变为 A 情况		

```js
var hasCycle = function(head) {
    if(!head || !head.next) {
        return false
    }
    let slow = head,
        fast = head
    while (fast && fast.next) {
        slow = slow.next
        fast = fast.next.next
        if (slow === fast) {
            return true
        }
    }
    return false
};
```

### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

示例 1：

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

**思路**

> 1. 先判断是否有环，快指针一次走两步，慢指针一次走一步
> 2. 如果快慢指针相遇，令 一个指针指向头节点，一个指针指向快慢指针相遇节点
> 3. 两个指针都一步一步地走，相遇节点即为入环节点
>
> ---
>
> 原理:
>
> ​	假设入环前有 `a` 长度， 快慢指针相遇处距离入环节点 `b` 长度，相遇处再走 `c` 长度可再次到达入环节点
> $$
> \because a + b + c + b = 2 * (a + b)  \quad\text{快指针的路程是慢指针的二倍}\\
> \therefore a = c
> $$
> ​	因为 a = c 所以两个指针都一步一步地走，相遇节点即为入环节点

```js
var detectCycle = function(head) {
    if (!head || !head.next) {
        return null
    }
    let slow = head.next,
        fast = head.next.next
    // 找到相遇节点
    while (slow !== fast) {
        if (!fast || !fast.next) {
            return null
        }
        slow = slow.next
        fast = fast.next.next
    }
    // 两个指针都一步一步地走，相遇节点即为入环节点
    slow = head
    while (slow !== fast ) {
        slow = slow.next
        fast = fast.next
    }
    return slow
};
```

### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

**思路**

> 1. 引入虚拟节点
> 2. 把两个链表较小的节点依次挂载在虚拟节点之后

#### 迭代

```js
var mergeTwoLists = function(l1, l2) {
    if (!l1) return l2
    if (!l2) return l1
    const dummyNode = new ListNode(-1)
    let prev = dummyNode
    while (l1 && l2) {
       if (l1.val < l2.val) {
           prev.next = l1
           l1 = l1.next
       } else {
           prev.next = l2
           l2 = l2.next
       }
        prev = prev.next
    }
    // 使用 || 可以优化 if 判断
    prev.next = l1 || l2
    return dummyNode.next
};
```

#### 递归

```js
var mergeTwoLists = function(l1, l2) {
    if (!l1) return l2
    if (!l2) return l1
    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2)
        return l1
    } else {
        l2.next = mergeTwoLists(l1, l2.next)
        return l2
    }
};
```



## 3. 栈

### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true

**思路**

> 1. 维护一个栈， 遍历字符串
> 2. 如果是左括号，入栈对应的右括号
> 3. 如果是右括号，如果和栈顶元素不匹配，返回 false
> 4. 遍历结束如果栈长度为 0  返回 true。(如果字符串类似于 “{{{”能完成遍历但栈长度不为 0)

```js
var isValid = function(s) {
    if (!s) return true
    if (s.length % 2 !== 0) return false
    const stack = []
    for (const c of s) {
        if (c === '(') {
            stack.push(')')
        } else if (c === '{') {
            stack.push('}')
        } else if (c === '[') {
            stack.push(']')
        } else if (stack.pop() !== c){
            return false
        }
    }
    return  stack.length === 0
};
```

### [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

**思路**

> 1. 同时维护一个 min 栈
> 2. 当有新元素入栈时，同时把 Math.min(新元素，min栈顶元素) 压入 min栈
> 3. 出栈时 min 栈也出栈

代码略

### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram.png) ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram_area.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 `10` 个单位。

**思路**

> 左边第一个低于该柱子的索引记为 left​, 右边第一个低于该柱子的索引记为right,
>
> 每个柱子所能围城的最大矩形面积为: $[(right - 1) - (left + 1) - 1] * 当前柱子高度$

#### 暴力法

> 1. 两层循环，代表两根柱子
> 2. 找出两根柱子之间柱子的最小高度即可求得当前矩形面积，和记录的最大值比较，更新最大值。

```js
var largestRectangleArea = function(heights) {
   if (heights.length < 1) return 0
   if (heights.length < 2) return heights[0]
   let max = 0
   // 因为柱子可以自成宽度，所以循环为 [0, length - 1]
   for (let i = 0; i < heights.length; i++) {
       // 每次循环时起始最低柱子高度为左柱高度
       let minHeight = heights[i]
       // 因为柱子可以自成宽度，所以循环为 [0, length - 1]
       for (let j = i; j < heights.length; j++) {
           // 比较右柱高度和最低高度
           minHeight = Math.min(minHeight, heights[j])
           const nowArea = minHeight * (j - i + 1)
           max = Math.max(max, nowArea)
       }
   }
   return max
};
```

#### 单调递增栈法

> 1. 维护一个栈， 对每个栈顶元素,当要出栈时,次栈顶元素是左边第一个小于栈顶元素的元素, 当前遍历元素是右边第一个小于栈顶元素的元素.
>
> 2. 初始化栈顶元素为 -1, 便于以统一的方式计算宽度
>
> 3. 当要入栈的柱子高于栈顶元素时直接入栈
>
> 4. 否则栈顶元素出栈（左右都是第一个低于栈顶元素的元素），直到栈顶元素小于要入栈的柱子高度
>
> 5. 每当栈顶元素出栈时计算**i - 1**与**stack[top-1] 元素**围成的面积：
>
>    最小i高度为栈顶元素代表的高度即 **$heights(stack[top])$**
>
>    宽度为 $[i-1, stack[top-1] + 1]$ 即**$(i - 1) - (stack[top -1] + 1) + 1 = i- stack[top - 1] - 1$**
>
>    所以面积为 **$heights(stack[top]) \times (i - stack[top - 1] - 1)$**
>
> 6. 当元素遍历完成后，栈中所有元素出栈并计算面积，此时宽度为  **$stack.length - stack[top-1]-1$**

```js
var largestRectangleArea = function(heights) {
   if (heights.length < 1) return 0
   if (heights.length < 2) return heights[0]
   let max = 0
   const stack = [-1]
   for (let i = 0; i < heights.length; i++) {
       // 防止-1出栈需加上 stac.peek() !== 
        while (stack.peek() !== -1 && heights[i] < heights[stack.peek()]) {
            // stack.pop() 之后 stack[top - 1] 即为 stack.peek()
            const currArea = heights[stack.pop()] * (i- stack.peek() - 1)
            max = Math.max(max, currArea)
        }
        stack.push(i)
   }
   // -1 是栈初始值不纳入计算
   while (stack.peek() !== -1) {
       const currArea = 
             heights[stack.pop()] * (heights.length - stack.peek() - 1)
       max = Math.max(max, currArea)
   }
   return max
};

Array.prototype.peek = function () {
    return this[this.length - 1]
}
```

#### 分治法

> 1. 计算 [left, right] 的最小高度 minHeight，求出当前面积 currArea: $minHeight \times (right - left + 1)$
> 2. 递归计算左区域最大面积 leftArea
> 3. 递归计算右区域最大面积 rightArea
> 4. 取 currArea、rightArea、rightArea 最大值

```js
var largestRectangleArea = function (heights) {
  if (heights.length < 1) return 0
  if (heights.length < 2) return heights[0]
  return getArea(heights, 0, heights.length - 1)

  function getArea(heights, left, right) {
    if (left > right) return 0
    let minIndex = left
    for (let i = left; i <= right; i++) {
      if (heights[i] < heights[minIndex]) minIndex = i
    }
    const currArea = heights[minIndex] * (right + 1 - left),
          leftArea = getArea(heights, left, minIndex - 1),
          rightArea = getArea(heights, minIndex + 1, right)
    return Math.max(currArea, leftArea, rightArea)
  }
};
```

### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png) 

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。

示例:

输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6

**思路**

> 每个位置能接的雨水是$Math.min(左边最大高度, 右边最大高度) - 自身高度$

#### 暴力法

> 1. 计算每个元素左边的最大高度 leftHeight, 右边的最大高度 rightHeight, 并取较小值 minHeight
> 2. minHeight - 元素自身高度 如果大于  0 即为当前位置储水量

```js
var trap = function(height) {
  if (!height || height.length < 2) return 0
  let result = 0
  for (let i = 1; i < height.length; i++) {
    let leftHeight = 0,
        rightHeight = 0
    // 找出左边高度最大值
    for (let j = i - 1; j > -1; j--)
      leftHeight = Math.max(leftHeight, height[j])
    // 找出右边高度最大值
    for (let j = i + 1; j < height.length; j++)
      rightHeight = Math.max(rightHeight, height[j])
    // 当前位置储水量
    console.log(leftHeight, rightHeight)
    const curr = Math.min(leftHeight, rightHeight) - height[i]
    result += curr > 0? curr: 0
  }
  return result
};
```

#### 单调递减栈法

**思路**

> 1. 定义辅助栈 stack, 对于每个栈顶元素要出栈时, 次栈顶元素是左边元素最大高度, 当前遍历元素是右边第一个大于栈顶的元素
>
> 2. 遍历数组
>
> 3. 当数组元素小于栈顶对应元素, 入栈
>
> 4. 否则出栈并计算**栈顶元素**, **栈次顶元素**, **当前遍历元素** 的盛水量 $distance * minHeight$
>
>    **distance** 指的是水平方向有多少元素应加上该储水量, 因为计算每个元素时本应以右边的最大元素为参照, 但算法中以第一个大于它的右边元素为参照, 所以遇到更大的元素时需要重新加上缺失部分储水量
>    **miHeight** 等于左边最大高度即 栈次顶元素 与右边第一个大于栈顶元素的元素即 当前遍历元素的较小值

```js
var trap = function(height) {
  if (!height || height.length < 2) return 0
  let result = 0
  const stack = []
  for (let i = 0; i < height.length; i++) {
    while (stack.length > 0 && height[i] > height[stack.peek()]) {
      const selfHeight = height[stack.pop()]
      // 如果弹出栈顶元素后栈为空, 结束循环
      if (stack.length === 0) break
      const leftHeight = height[stack.peek()],
            rightHeight = height[i],

            // 注意 distance 的算法
            distance = i - stack.peek() - 1
      result += 
          distance * (Math.min(rightHeight , leftHeight) - selfHeight)
    }
    stack.push(i)
  }
  return result
};

Array.prototype.peek = function () {
    return this[this.length - 1]
}
```

#### 双指针法

> 1. 对于从左边开始遍历的元素来说 leftMax 是可信的, rightMax 是不可信的 (有可能不是最大), 但是如果 rightMax < leftMax, 则储水高度只与 leftMax 有关, 而不用关心 rightMax 是否可信了. 
> 2. 此时如果 height[left] < leftMax, 则result += leftMax - height[left], 否则 leftMax = height[left]
> 3. left++
> 4. 右边同理

```js
var trap = function(height) {
  if (!height || height.length < 2) return 0
  let leftMax = height[0],	// 左边最大值
      rightMax = height[height.length - 1],  // 右边最大值
      left = 1,	// 左边索引
      right = height.length - 2,  // 右边索引
      result = 0
  while (left <= right) {
    if (leftMax < rightMax) {
      height[left] < leftMax? 
      result += leftMax - height[left]: leftMax = height[left]
      left++  
    } else {
      height[right] < rightMax? 
      result += rightMax - height[right]: rightMax = height[right]
      right-- 
    }
  }
  return result 
};
```



## 4. 队列

### [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

进阶：

你能在线性时间复杂度内解决此题吗？

```
示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

滑动窗口的位置                最大值

 [1  3  -1] -3  5  3  6  7      3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

#### 暴力法

> 1. 共会滑动 nums.length - k - 1次
> 2. 获取每次滑动窗口的最大值并添加到结果中

```js
var maxSlidingWindow = function(nums, k) {
  const result = []
  for (let i = 0; i < nums.length - k + 1; i++) {
    let max = -Infinity
    for (let j = i; j < i + k; j++) {
      max = Math.max(max, nums[j])
    }
    result.push(max)
  }
  return result 
};
```

#### 双端队列法

> 1. 维护一个队列, 队列的第一个值存放当前窗口的最大值
> 2. 队列的生成方式: 如果要入队的值大于队尾的值,则队尾的值出队
> 3. 每次存储结果时, 如果队首元素不属于这个窗口,则将其出队

```js
var maxSlidingWindow = function(nums, k) {
    const queue = [],
          result = []
    for (let i = 0; i < nums.length; i++) {
        // 如果队列不为空，且要入队的元素大于队尾元素, 队尾元素出队
        while (queue.length > 0 && nums[i] > nums[queue[queue.length - 1]]) 		{
            queue.pop()
        }
        queue.push(i)

        // j 是把 i 为作为滑动窗口最后一个值时滑动窗口第一个值的索引
        const j = i - k + 1
        // j >= 0 说明滑动窗口已构建完毕
        if (j >= 0) {
            // 当队首元素不属于当前滑动窗口时出队
            if (queue[0] < j) queue.shift()
            // 把队首元素添加到结果数组中
            result.push(nums[queue[0]])
        }
    }
    return result
};
```

### [622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)

**思路**

> 1. 构造函数初始化四个值: 
>
>    **① queue ② head ③ tail ④capacity = k + 1 (为了便于判断队列满,总会浪费一个位置)**
>
> 2. 下一个索引位置的计算方式  **next(n) = (n + 1) % capacity**
>
> 3. 前一个索引位置的计算方式 **prev(n) = (n - 1 + k) % capacity**
>
> 4.  **this.head = this.tail ** 时, 队列为空
>
> 5. **next(tail) = this.head** 时, 队列为满

```js
// 初始化
var MyCircularQueue = function(capacity) {
    this.queue = []
    this.head = 0  // head 表示当前队列头
    this.tail = 0   // tail 表示下一个元素要存放的位置
    this.capacity = capacity + 1

    // 两个辅助函数
    this.next = (n) => (n + 1) % this.capacity
    this.prev = (n) => (n - 1 + this.capacity) % this.capacity
};

// 判断队列是否为空
MyCircularQueue.prototype.isEmpty = function() {
    return this.head === this.tail
};

// 判断队列是否满
MyCircularQueue.prototype.isFull = function() {
    return this.next(this.tail) === this.head
};

// 入队
MyCircularQueue.prototype.enQueue = function(value) {
    if (this.isFull()) return false
    this.queue[this.tail] = value
    this.tail = this.next(this.tail)
    return true
};

// 出队
MyCircularQueue.prototype.deQueue = function() {
    if (this.isEmpty()) return false
    const result = this.queue[this.head]
    this.head = this.next(this.head)
    return true
};

// 获取队首元素
MyCircularQueue.prototype.Front = function() {
    if (this.isEmpty()) return -1
    return this.queue[this.head]
};

// 获取队尾元素
MyCircularQueue.prototype.Rear = function() {
    if (this.isEmpty()) return -1
    return this.queue[this.prev(this.tail)]
};
```



## 5. 哈希表

### [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

**示例**

```
输入: s = "anagram", t = "nagaram"
输出: true
示例 2:

输入: s = "rat", t = "car"
输出: false
```

#### 哈希表法

> 1. 遍历前一个字符串, 并统计每个字符出现的次数记录在 hash 中
> 2. 遍历第二个字符串, 如果 hash 中不存在当前字符返回false, 否则 hash 中该字符对应的值减1, 如果减为 0, 删除 hash 中该字符的相应记录

```js
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false
    const hash = new Map()
    for (const c of s) {
        // 计算每个字符的个数
        hash.has(c)? hash.set(c, hash.get(c) + 1): hash.set(c, 1)
    }
    for (const c of t) {
        // 如果 hash 中不存在 c 返回 false 
        if (!hash.has(c)) return false
        
        // 如果 hash 中存在c, count--， 如果 count = 0, 删除 hash 中的此记录
        const count = hash.get(c) - 1
        count === 0? hash.delete(c): hash.set(c, count)
    }
    return hash.size === 0
};
```

#### 排序法

> 把两个字符串先拆分成数组 (字符串没有 sort 方法), 排序后再组合成字符串然后比较, 如果相同则是字母异位词

```js
var isAnagram = function(s, t) {
  return s.split('').sort().join() === t.split('').sort().join()
};
```

### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```js
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**思路**

> 如果遍历比较每个字符和其他字符是否属于同一组, 会使得时间复杂度过大而且字符数组的重复项难以处理. 
>
> 遍历一遍字符数组并同时使用哈希表记录会使得时间复杂度变为 O(n)
>
> 1. 使用 map 记录, 每个记录的键为 **关键字符**, 值为 **异位词数组**
> 2. 遍历字符数组, 
> 3. 对当前字符排序得到关键字符(需先转为数组才能使用 sort 方法, 排序后再使用 join 转回字符串)
>    如果 map 中有以当前关键字符为键的记录, 将当前字符添加到记录中
>    否则新建一条**包含当前字符的数组**的记录
> 4. 把 map 的值转换为数组,返回 ( `map.values()` 返回的是迭代器不是数组)

```js
var groupAnagrams = function(strs) {
    const map = new Map()
    for (const str of strs) {
        //通过对字符串排序获得关键字，关键字相同的字符属于异位词
        const key = str.split('').sort().join('')
        // 如果 map[key] 存在，把当前字符加入，否则新建一个包含当前字符的数组
        // 注意不能使用 map.get(key).push(str)赋值,因为取的是 push 的返回值
        map.has(key)? 
            map.set(key, [...map.get(key), str]): map.set(key, [str])
    }
    return Array.from(map.values())
};
```

### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

**示例:**

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

**思路**

> 1. 维护一个 map, 键为当前元素的值, 值为当前元素的索引
> 2. 遍历数组, remain = 目标值 - 当前元素的值
> 3. 如果 map 中存在以 remain 为键的记录, 则返回该记录和当前索引组成的数组
>    否则在 map 中添加一条以当前元素的值为键的记录
> 4. 如果遍历结束还没返回值, 则说明无符合要求的值, 返回空数组

```js
var twoSum = function(nums, target) {
    const map = new Map()
    for (let i = 0; i < nums.length; i++) {
        const remain = target - nums[i]
        if (map.has(remain)) return [map.get(remain), i]
        map.set(nums[i], i)
    }
    return []
};
```



## 6. 树

### [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

给定一个二叉树，返回它的 *前序* 遍历。

 **示例:**

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```

#### 递归

> 1. 先访问根
> 2. 递归访问左子树
> 3. 递归访问右子树

```js
var preorderTraversal = function(root) {
    const result = []
    return helper(root, result)
    function helper (root, result) {
        if (!root) return []
        // 注意结果数组中保存的是值不是节点
        result.push(root.val)
        helper(root.left, result)
        helper(root.right, result)
        return result
    }
};
```

#### 迭代1

> 1. 维护一个节点栈 stack
> 2. 如果**当前节点不为空** 或 **栈不为空** 时进入循环
> 3. 每次循环都找到**当前节点的最左边节点** ==先序遍历在这一步把当前节点加入结果数组==
> 4. 然后**访问栈顶元素的右节点** ==中序遍历在这一步把栈顶节点加入结果数组==

```js
var preorderTraversal = function(root) {
    const result = [],
          stack = []
    let current = root
    // 如果当前元素不为空 或 栈不为空 进入循环
    while (current || stack.length > 0) {
        // 找到当前节点最左的节点
        while (current) {
            // 把当前节点加入结果数组，注意加入的是节点的值
            result.push(current.val)
            // 当前节点入栈
            stack.push(current)
            current = current.left
        }
        // 访问节点的右节点
        current = stack.pop().right
    }
    return result
};
```

#### 迭代2

> 1. 维护一个栈，起初栈中放入根节点，当栈不为空时遍历
>
> 2. 每次循环:
>
>    弹出栈顶元素并把栈顶元素放入结果数组
>
>    如果栈顶元素的右节点不为空，压入栈中；如果栈顶元素左节点不为空，压入栈中。注意是**先右后左**

```js
var preorderTraversal = function(root) {
    if (!root) return []
    const result = [],
          stack = [root]
    while (stack.length > 0) {
        const current = stack.pop()
        result.push(current.val)
        if (current.right) stack.push(current.right)
        if (current.left) stack.push(current.left)
    }
    return result
};
```

### [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/narytreeexample.png" alt="img" style="zoom:50%;" />

 

返回其前序遍历: [1,3,5,6,2,4]。

#### 递归

```js
var preorder = function(root) {
    const result = []
    return helper(root, result)
    function helper(root, result) {
        if (!root) return []
        result.push(root.val)
        for (const child of root.children) helper(child, result)
        return result
    }
};
```

#### 迭代

**思路**

> 1. 维护一个栈，起初栈中放入根节点，当栈不为空时遍历
> 2. 每次循环弹出栈顶元素，放入结果数组，并把**栈顶元素的子元素<font color="red">反序</font>压入栈中**，保证栈顶元素就是要遍历的元素

```js
var preorder = function(root) {
    if (!root) return []
    const result = [],
          stack = [root]
    while (stack.length > 0) {
        const current = stack.pop()
        // 把栈顶元素放入结果数组
        result.push(current.val)
        // 把栈顶元素的子元素反序推入栈中
        stack.push(...current.children.reverse())
    }
    return result
};
```

### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

#### 递归

```js
var inorderTraversal = function(root) {
    const result = []
    return helper(root, result)
    function helper(root, result) {
        if (!root) return []
        helper(root.left, result)
        result.push(root.val)
        helper(root.right, result)
        return result
    }
};
```

#### 迭代

```js
var inorderTraversal = function(root) {
    const result = [],
          stack = []
    let current = root
    while (current || stack.length > 0) {
        while (current) {
            stack.push(current)
            current = current.left
        }
        // 在第二次访问节点时把节点加入结果数组
        current = stack.pop()
        result.push(current.val)
        current = current.right
    }
    return result
};
```

### [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

#### 递归

```js
var postorderTraversal = function(root) {
    const result = []
    return helper(root, result)
    function helper(root, result) {
        if (!root) return []
        helper(root.left, result)
        helper(root.right, result)
        result.push(root.val)
        return result
    }
};
```

#### 迭代

> 1. 与前序遍历和中序遍历略有不同，后序遍历需要一个额外的变量 prev
>
> 2. 入栈规则与前序中序相同，都是要先找到最左边的节点
>
> 3. 出栈规则不同:
>
>    只有 **栈顶元素的右节点为空** 或 **栈顶元素的右节点等于 prev** 时才出栈. 出栈同时需要进行以下操作:
>
>    ==把栈顶元素放入结果数组中==  ==更新 prev 为出栈节点== ==把 current 赋值为 null 判断下一个栈顶元素==
>
> 4. 如果元素不出栈，则访问元素的右节点

```js
var postorderTraversal = function(root) {
    const result = [],
          stack = []
    let current = root,
        prev = null
    while (current || stack.length > 0) {
        while (current) {
            stack.push(current)
            current = current.left
        }
        // 不能直接弹栈
        current = stack[stack.length - 1]
        // 如果没有右节点，或右节点已经访问过才可以弹栈
        if (!current.right || current.right === prev) {
            stack.pop()
            // 把 current 放入结果数组
            result.push(current.val)
            // 更新 prev
            prev = current
            // 判断下一个栈顶元素
            current = null
        } else {
            current = current.right
        }
    }
    return result
};
```

#### 迭代2

> 0. 反序遍历 **根右左** 得到的序列
>
> 1. 维护一个栈，初始化为根节点，当栈不为空时，循环
>
> 2. 每次循环:
>
>    栈顶元素出栈，并在结果数组中记录栈顶元素的值
>
>    如果存在左子树，左子树入栈；如果存在右子树，右子树入栈。**先左后右**
>
> 3. 将得到的序列反转

```js
var postorderTraversal = function(root) {
    if (!root) return []
    const result = [],
          stack = [root]
    while (stack.length > 0) {
        const current = stack.pop()
        result.push(current.val)
        current.left && stack.push(current.left)
        current.right && stack.push(current.right)
    }
    return result.reverse()
};
```



### [590. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

给定一个 N 叉树，返回其节点值的*后序遍历*。

例如，给定一个 3叉树 :

 

<img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/narytreeexample.png" alt="img" style="zoom:50%;" />

 

返回其后序遍历: [5,6,3,2,4,1]

#### 递归

> 类似于二叉树， 递归遍历子树后再访问根即可

```js
var postorder = function(root) {
    const result = []
    return helper(root, result)
    function helper(root, reuslt) {
        if (!root) return []
        const {children} = root
        // 递归遍历子树
        for (const child of children) {
            helper(child, result)
        }
        // 访问根
        result.push(root.val)
        return result
    }
};
```

#### 迭代

> 0. 先根-右子-左子遍历，再把得到的结果反转
> 1. 维护一个栈，起初放入根节点，当栈不为空时，循环
> 2. 每次循环弹出一个元素，放入结果数组，同时把它的子元素都放入栈中

```js
var postorder = function(root) {
    if (!root) return []
    const result = [],
          stack = [root]
    while (stack.length > 0) {
        const current = stack.pop()
        result.push(current.val)
        stack.push(...current.children)
    }
    return result.reverse()
};
```

### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

#### 保存前一层节点数组

```js
var levelOrder = function(root) {
    if (!root) return []
    const result = []
    // previous 表示前一层节点
    let previous = [root]
    while (previous.length > 0) {
        const values = [],
              nodes = []
        for (const curr of previous) {
            values.push(curr.val)
            // 如果存在则添加
            curr.left && nodes.push(curr.left)
            curr.right && nodes.push(curr.right)
        }
        previous = nodes
        result.push(values)
    }
    return result
};
```

#### 保存前一层节点个数

```js
var levelOrder = function(root) {
    if (!root) return []
    const result = [],
          queue = [root]
    while (queue.length > 0) {
        const values = []
        // 记录 size
        for (let i = 0, size = queue.length; i < size; i++) {
            const current = queue.shift()
            values.push(current.val)
            current.left && queue.push(current.left)
            current.right && queue.push(current.right)
        }
        result.push(values)
    }
    return result
};
```



### [429. N叉树的层序遍历](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

给定一个 N 叉树，返回其节点值的层序遍历。 (即从左到右，逐层遍历)。

例如，给定一个 3叉树 :

 <img src="https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/narytreeexample.png" alt="img" style="zoom:50%;" />

返回其层序遍历:

```
[
     [1],
     [3,2,4],
     [5,6]
]
```

#### 保存前一层节点数组

> 1. 需要维护一个存储上一层节点的数组 previous
> 2. 当 previous 不为空时，循环
> 3. 每次循环填充 values 数组 和 nodes 数组
> 4. 每次循环结束，把 values 添加到结果中，把 nodes 赋值给 previous

```js
var levelOrder = function(root) {
    if (!root) return []
    const result = []
    // previous 存储上一层节点
    let previous = [root]
    while (previous.length > 0) {
        const values = [],
              nodes = []
        for (const node of previous) {
            values.push(node.val)
            nodes.push(...node.children)
        }
        result.push(values)
        previous = nodes
    }
    return result
};
```

#### 保存前一层节点个数

> 1. 维护一个队列, 初始值为根节点, 当队列不为空时, 循环
> 2. 每次循环有一个子循环:  循环队列长度次,同时填充 values
> 3. 每次循环结束把 values 添加到结果中

```
var levelOrder = function(root) {
    if (!root) return []
    const queue = [root],
          result = []
    while (queue.length > 0) {
        const values = []
        for (let i = 0, size = queue.length; i < size; i++) {
            current = queue.shift()
            values.push(current.val)
            queue.push(...current.children)
        }
        result.push(values)
    }
    return result
};
```



## 7. 递归

### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

**示例：**

```js
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

**思路**

> 1. 左括号什么时候都可以添加， 只要数量不超标
> 2. 右括号只有在左括号数量大于右括号时才可以添加

```js
var generateParenthesis = function(n) {
    const result = []
    helper(n, result, "", 0, 0)
    return result
};

// n 表示括号对数，left 表示左括号个数，right 表示右括号个数
function  helper(n, result,str, left, right) {
    if (left + right === 2 * n) {
        return result.push(str)
    }
    if (left < n) {
        helper(n, result, str + '(',  left + 1, right)
    }
    if (right < left) {
        helper(n, result, str + ')', left, right + 1)
    }
}
```



### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

翻转一棵二叉树。

**示例：**

输入：

```
    4                     
   /   \
  2     7  
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**思路**

> 递归反转左右节点

```js
var invertTree = function(root) {
    if (!root) return null
    // 只暂存一下即可， 不需要先替换再反转
    const temp = root.left
    root.left = invertTree(root.right)
    root.right= invertTree(temp)
    return root
};
```



### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

- 节点的左子树只包含小于当前节点的数。
- 节点的右子树只包含大于当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**思路**

> 需要注意的是不仅需要比较左右节点和根节点，还需要比较当前节点和上下界，比如左树的右节点也不能大于根节点

#### 递归

> 1. 递归比较每个节点是否处于上下界之间

```js
var isValidBST = function(root) {
    return helper(root)
};
function helper(node, low = -Infinity, high = Infinity) {
    if (!node) return true
    const val = node.val
    if (val <= low || val >= high) return false
    // 上下界会逐层传递
    return helper(node.left, low, val) &&
           helper(node.right, val, high)
}
```

#### 迭代

> 二叉搜索树中序遍历的值为一个递增序列

```js
var isValidBST = function(root) {
    if (!root) return true
    const stack = []
    let prev = -Infinity,
        current = root
    while (current || stack.length > 0) {
        while (current) {
            stack.push(current)
            current = current.left
        }
        current  = stack.pop()
        // 如果不是递增序列，则返回false
        if (current.val <= prev) return false
        prev = current.val
        current = current.right
    }
    return true
};
```



### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

**示例：**
给定二叉树 [3,9,20,null,null,15,7]，

      	3
       / \
      9  20
        /  \
       15   7
返回它的最大深度 3 。

**思路**

> 递归求取左右子树的最大深度再加1

```js
var maxDepth = function(root) {
    if (!root) return 0
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
};
```



### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

与上一题类似，但是是求最小深度

**思路**

> 1. 如果没有左子树，则递归求右子树最小深度
> 2. 如果没有右子树，则递归求左子树最小深度
> 3. 如果左右子树都有，则递归求左右子树最小深度

```js
var minDepth = function(root) {
    if (!root) return 0
    if (!root.left) return 1 + minDepth(root.right)
    if (!root.right) return 1 + minDepth(root.left)
    return 1 + Math.min(minDepth(root.left), minDepth(root.right))
};
```



### [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

序列化是把树结构转换为字符串，反序列化是把字符串转换为树结构

**思路**

> 1. 利用前序遍历把树序列化，当节点为空时，赋值为 ‘#’，返回一个队列的字符串表示形式
> 2. 反序列化时，从队首取出一个元素:
>    如果是 ‘#’ 则返回 null ，否则以元素的值构建节点
>    该节点的左树为递归遍历队列的返回值
>    该节点的右树为递归遍历队列的返回值

```js
// 序列化
var serialize = function(root) {
    let result = []
    helper(root, result)
    return JSON.stringify(result)
};

function helper(root, result) {
    if (!root) return result.push('#')
    result.push(root.val)
    helper(root.left, result)
    helper(root.right, result)
}

// 反序列化
var deserialize = function(data) {
    data = JSON.parse(data)
    return helper2(data)
};

function helper2(data) {
    const val = data.shift()
    if (val === '#') return null
    const root = new TreeNode(val)
    root.left = helper2(data)
    root.right = helper2(data)
    return root
}

```



### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

 

**示例 1:**

```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3
```

**思路**

> 1. 如果 root 为空或者是 p， q 中的任何一个，返回 root
> 2. 在 root.left  和 root.right 中递归查找 p,q
> 3. 如果只有 left 不为空， 返回 left，如果只有 right 不为空，返回 right
> 4. 如果 left 和 right 都不为空， 返回 root

```js
var lowestCommonAncestor = function(root, p, q) {
    if (!root || root === p || root === q) return root
    const left = lowestCommonAncestor(root.left, p, q),
          right = lowestCommonAncestor(root.right, p, q)
    // 如果只有 left 或 right 有值，说明 p，q 是父子关系
    if (!left) return right
    if (!right) return left
    // 如果 left 和 right 都有值，root 是它们的公共祖先
    return root
};
```

### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

        3
       / \
      9  20
        /  \
       15   7
 **思路**

> 1. 取前序遍历的第一个元素作为根元素
> 2. 找到中序遍历中该元素的索引 index
> 3. 递归生成左子树 ==preorder[1, index + 1) & inorder[0, index)==
> 4. 递归生成右子树  ==preorder[index, length) & inorder[index + 1, length)==

```js
var buildTree = function(preorder, inorder) {
    if (!preorder.length) return null
    const index = inorder.indexOf(preorder[0]),
          left = buildTree(
              preorder.slice(1, index + 1),
              inorder.slice(0, index)
          ),
          right = buildTree(
              preorder.slice(index + 1),
              inorder.slice(index + 1)
          )
    return {val: preorder[0], left, right}
};
```

### [77. 组合](https://leetcode-cn.com/problems/combinations/)

给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

**示例:**

```
输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**思路**

> 1. 每组都是从 first 逐个加到 n
> 2. 当 curr 的长度为 k  时返回
> 3. 回溯时注意哪些是变化的，哪些是不变的

```js
var combine = function(n, k) {
    const result = []
    helper(n, k, result, 1)
    return result
};
function helper(n, k, result, first, curr = []) {
    if (curr.length === k) return result.push(curr)
    for (let i = first; i <= n; i++) {
        // first = i + 1, curr 是不变的，所以使用 concat(i)
        helper(n, k, result, i + 1, curr.concat(i))
    }
}
```



### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

**示例:**

```js
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

#### 交换法

> 1. 把选择的节点交换到第一个位置来
> 2. 每一层依次交换当前位置的节点与当前位置之后的节点

```js
var permute = function(nums) {
    const result = []
    helper(nums.length, nums, result, 0)
    return result
};
function helper(n, nums, result, first) {
    // 不能直接添加数组，因为会回溯，需要添加数组当前样式的拷贝
    if (first === n) return result.push(nums.concat())
    for (let i = first; i < n; i++) {
        swap(nums, first, i)
        // 不能 ++first, 而是 first + 1
        helper(n, nums, result, first + 1)
        swap(nums, first, i)
    }
}

function swap (nums, i, j) {
    const temp = nums[i]
    nums[i] = nums[j]
    nums[j] = temp
}
```

#### 访问数组法

> 1. 使用 used 数组记录哪些节点已经被选择了
> 2. 当 curr 的长度为 n 时就获得结果了

```js
var permute = function(nums) {
    const result = [],
          used = []
    helper(nums.length, nums, result, used)
    return result
};
function helper(n, nums, result, used, curr = []) {
    if (curr.length === n) return result.push([...curr])
    for (let i = 0; i < n; i++) {
        if (used[i]) continue
        curr.push(nums[i])
        used[i] = true
        helper(n, nums, result, used, curr)
        // 回溯
        curr.pop(nums[i])
        used[i] = false
    }
}
```

### [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

给定一个可包含重复数字的序列，返回所有不重复的全排列。	

**示例:**

```
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

> 1. 使用访问数组法全排列
> 2. 当 ` i > 0 && nums[i-1] === nums[i]&& !used[i - 1]` 时减枝

```js
var permuteUnique = function(nums) {
    // 数组排序
    nums.sort((a, b) => a - b)
    const result = [],
          used = []
    helper(nums.length, nums, result, used)
    return result
};
function helper(n, nums, result, used, curr = []) {
    if (curr.length === n) return result.push([...curr])
    for (let i = 0; i < n; i++) {
        if (used[i]) continue
        // 去重
        if (i > 0 && nums[i] === nums[i - 1] && !used[i - 1]) continue
        curr.push(nums[i])
        used[i] = true
        helper(n, nums, result, used, curr)
        //回溯
        curr.pop()
        used[i] = false
    }
}
```



## 动态规划

### [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

```
输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
```

1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右

**思路**

> 0. $f(i, j) = f(i - 1, j) + f(i, j - 1)$
> 1. 使用二维数组记录中间值
> 2. 第一行和第一列值都为 1
> 3. 结果为 `result[n - 1][m - 1]`

```js
var uniquePaths = function(m, n) {
  const result = []
  
  // 第一列全为 1
  for (let i = 0; i < n; i++) result[i] = [1]
  // 第一行全为1
  for (let j = 0; j < m; j++) result[0][j] = 1

  // f(i, j) = f(i - 1, j) + f(i, j - 1)
  for (let i = 1; i < n; i++) {
    for (let j = 1; j < m; j++) {
      result[i][j] = result[i - 1][j] + result[i][j - 1]
    }
  }
  
  return result[n - 1][m - 1]
};
```

### [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

 现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？ 

> 思路与上一题类似但是要注意:
>
> 1. 起始行，也就是第一行和第一列只要有障碍，后面的都为 0
> 2. 非起始行， 如果有障碍就为 0

```js
var uniquePathsWithObstacles = function(obstacleGrid) {
    const result = [],
          m = obstacleGrid[0].length,
          n = obstacleGrid.length
	// 初始化二维数组
    for (let i = 0; i < n; i++) result[i] = []
    result[0][0] = obstacleGrid[0][0] === 1? 0: 1
	
    // 前一个位置为 0， 或当前位置被阻塞初始化为 0 ， 否则 1
    for (let i = 1; i < n; i ++) {
      result[i][0] = 
          result[i - 1][0] === 0 || obstacleGrid[i][0] === 1? 0: 1
    }

    // 前一个位置为 0， 或当前位置被阻塞初始化为 0 ， 否则 1
    for (let i = 1; i < m; i ++) {
      result[0][i] = 
          result[0][i - 1] === 0 || obstacleGrid[0][i] === 1? 0: 1
    }
    
    // fn(i, j) = fn(i - 1, j) + fn(i, j -1)
    for (let i = 1; i < n; i++) {
      for (let j = 1; j < m; j++) {
        result[i][j] = 
          obstacleGrid[i][j] === 1? 0: result[i -1] [j] + result[i][j - 1]
      }
    }
    return  result[n - 1][m - 1]
};
```

### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

**示例:**

```
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。
```

**思路**

> 与前两题思路类似，但是注意:
>
> 1. 初始化第一列和第一行的值时值为: **前一个位置的值 + 当前值**
> 2. 对非第一列和第一行的值: **上一行和前一列的较小值 + 当前值**

```js
var minPathSum = function(grid) {
  const result = [],
        m = grid[0].length
        n = grid.length
  // 初始化二维数组
  for (let i = 0; i < n; i++) result[i] = []
  // 起点为初始值
  result[0][0] = grid[0][0]
  // 第一列赋初值
  for (let i = 1; i < n; i++) result[i][0] = grid[i][0] + result[i - 1][0]
  // 第一行赋初值
  for(let i = 1; i < m; i++) result[0][i] = grid[0][i] + result[0][i - 1]

  // f(i, j) = f(i - 1, j) + f(i, j - 1)
  for (let i = 1; i < n; i++) {
    for (let j = 1; j < m; j++) {
      result[i][j] = 
          Math.min(result[i - 1][j], result[i][j - 1]) + grid[i][j]
    }
  }
  return result[n - 1][m - 1]
};
```

### [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

**示例 1:**

```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```

**思路**

> $fn(n) = Math.min(fn(n - {c_1}) + 1, fn(n - {c_2}) + 1, ..., fn(n - {c_n}) + 1) \;\;{c_i}\in coins$ 
>
> 1. 创建一个 长度为 amount + 1 的一维数组 result，并用 Infinity 填充 (便于之后比较)
> 2. result[0] = 0 , 作为动态规划的起点
> 3. 对每个 amount，如果 amount  **>=** c 则取 当前值 (会不断迭代，这也是初始值为 Infinity 的原因) 和 fn(amount - c) 的较小值

```js
var coinChange = function(coins, amount) {
  const dp = new Array(amount + 1).fill(Infinity)
  dp[0] = 0
  for (let i = 1; i <= amount; i++) {
    for (const c of coins) {
      // 只有 i >= c 时才比较
      if (i >= c) {
        dp[i] = Math.min(dp[i], 1 + dp[i - c])
      }
    }
  }
  return dp[amount] === Infinity? -1: dp[amount]
};
```

### [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。  

**示例 1:**	

```js
输入: amount = 5, coins = [1, 2, 5]
输出: 4
解释: 有四种方式可以凑成总金额:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**思路**

> 本题和上一题有些类似但又不尽相同。如果使用 amount 做规划则在累加过程中会出现重复，所以本题目不适合用这种方法，而应该使用硬币面额数规划
>
> 1. 从任何硬币面额都没有，逐步规划到包含所有 coins 面额的硬币。
> 2. 当任何硬币都没有时，result[0] = 1
> 3. 每次比较时从 i = coin 开始比较，$fn(n) += fn(n - 1)$

```js
var change = function(amount, coins) {
  const result = new Array(amount + 1).fill(0)
  result[0] = 1
  for (const coin of coins) {
    for (let i = coin; i <= amount; i++) {
      result[i] += result[i - coin]
    }
  }
  return result[amount]
};
```

### [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

若这两个字符串没有公共子序列，则返回 0。

**示例 1:**

```js
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
```

**思路**

> 1. text1 做为 x 轴， text2 作为 y 轴，进行二维动态规划
> 2. 初始化第一个值: 如果 text1[0] = text2[0] 初始化为 1， 否则 0 
> 3. 初始化第一行 和 第一列: 如果当前字符与另一个串相同，初始化为1， 否则初始化为前一个位置的值
> 4. 迭代: 如果当前字符与另一个串字符相同，**$fn(i,j) = fn(i-1,j-1) + 1$**
>                    否则 **$f(i, j) = Math.max(f(i-1, j), f(i, j - 1))$**

```js
var longestCommonSubsequence = function(text1, text2) {
  const result = []
        m = text1.length,
        n = text2.length

  // 初始化数组
  for (let i = 0; i < n; i++) result[i] = []
  result[0][0] = text1[0] === text2[0]? 1: 0

  // 初始化第一行： 如果当前字符与 text2 相同为 1, 否则为前一个的值
  for (let i = 1; i < m; i++) {
    result[0][i] = text1[i] === text2[0]? 1: result[0][i - 1]
  }

  // 初始化第一列： 如果当前字符与 text1 相同为 1, 否则为前一个的值
  for (let i = 1; i < n; i++) {
    result[i][0] = text2[i] === text1[0]? 1: result[i - 1][0]
  }

  for (let i = 1; i < n; i++) {
    for (let j = 1; j < m; j++) {
      result[i][j] = text1[j] === text2[i]?
        result[i - 1][j - 1] + 1: 
      	Math.max(result[i - 1][j], result[i][j - 1])
    }
  }

  return result[n - 1][m - 1] 
};
```

### [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

**示例**

```
例如，给定三角形：
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
```

**思路**

> 1. 自底向上动态规划
> 2. $fn(i, j) = Math.min(fn(i + 1, j), fn(i + 1, j + 1))$
> 3. $fn(0, 0) $即为结果

```js
var minimumTotal = function(triangle) {
    const result = triangle
    // 注意是 i-- , 自底向上
    for (let i = result.length - 2; i >= 0; i--) {
      for (let j = 0; j < result[i].length; j++) {
        result[i][j] = 
          Math.min(result[i + 1][j], result[i + 1][j + 1]) + result[i][j]
      }
    }
    return result[0][0]
};
```

### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例:**

```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**思路**

> $fn(i) = Math.max(fn(i - 1), 0) + fn(i)$

```js
var maxSubArray = function(nums) {
  const result = [nums[0]]
  for (let i = 1; i < nums.length; i++) {
    // 如果之前累加对结果没有贡献则舍去
    result[i] = Math.max(result[i - 1], 0) + nums[i]
  }
  return Math.max(...result)
};
```

### [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

 给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字）。 

**示例 1:**

```
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**思路**

> 1. 维护一个当前乘积最小值 min ，和一个当前乘积最大值 max
> 2. 遍历数组，当前遍历元素为负值时，交换最大值和最小值
> 3. 取 **$Max\{max\times num, num\}和 Min\{min\times num, num\} $** 
> 4. 每次循环结束取 **$Max\{max, result\}$**

```js
var maxProduct = function(nums) {
    let result = -Infinity,
        max = 1,
        min = 1
    for (const num of nums) {
      if (num < 0) [min, max] = [max, min]
      // 如果之前乘积没有贡献则舍去
      max = Math.max(max * num, num) 
      min = Math.min(min * num, num)
      // 最大值可能出现在中间所以要每次循环都比较 max 和 result
      result = Math.max(max, result)
    }
    return result
};
```

### [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

**示例 1:**

```
输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**思路：**

> 1. 从前往后动态规划
> 2. **$f(n) = Math.max(f(n - 2) + nums[n], f(n - 1))\;\;f(n) 表示偷第n个的最大值$** 

#### 常规动态规划

```js
var rob = function(nums) {
  if (nums.length < 2) return nums[0] || 0
  const result = [nums[0]]
  result[1] = Math.max(nums[0], nums[1])
  // 可以使用 result[-1] = result[-2] 代替以上三行  
  for (let i = 2; i < nums.length; i++) {
    result[i] = Math.max(result[i - 1], result[i - 2] + nums[i])
  }
  return result[nums.length - 1]
};
```

#### 简洁版

```js
var rob = function(nums) {
  let prevMax = 0,
      result = 0
  for (const num of nums) {
    const temp = result 
    result = Math.max(prevMax + num, result)
    prevMax = temp
  }
  return result 
};
```

### [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

 这个地方所有的房屋都**围成一圈，**这意味着第一个房屋和最后一个房屋是紧挨着的。 

**示例 1:**

```
输入: [2,3,2]
输出: 3
解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**思路**

> 只需进行两次动态规划，第一次不包含第一个房屋，第二次不包含最后一个房屋，然后取最大值即可

```js
var rob = function(nums) {
  // 注意只有一个元素首尾相接的情况
  if (nums.length === 1) return nums[0] || 0
  return Math.max(
    helper(nums.slice(1)),
    helper(nums.slice(0, -1))
  )

  function helper(nums) {
  let prevMax = 0,
        result = 0
  for (const num of nums) {
    const temp = result
    result = Math.max(prevMax + num, result)
    prevMax = temp
  }
  return result
  }
};
```

### [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。

**示例**

    输入: [3,2,3,null,3,null,1]
    
         3
        / \
       2   3
        \   \ 
         3   1
    
    输出: 7 
    解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
**思路**

> 1. 对每棵子树返回一个数组 result
>
>    ​	result[0] 表示不选择根节点，左右子节点都可以 选/ 不选, 则: 
>
>       $\;\;\;\;\;result[0] = Math.max(left[0], left[1]) + Math.max(right[0] + right[1])$
>
>       result[1] 表示选择根节点， 那么左右节点都不能选, 则:
>
>    $\;\;\;\;\;\;\;\;result[1] = left[0] + right[0] + root.val$
>
> 2. 对整棵树进行后序遍历即可得到结果

```js
var rob = function(root) {
    const result = helper(root)
    return Math.max(result[0], result[1])
    function helper(root) {
      if (!root) return [0, 0]
      const left = helper(root.left),
            right = helper(root.right)
      // result[0] 表示不选，result[1]表示选
      const result = []
      result[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1])
      result[1] = left[0] + right[0] + root.val
      return result
    }
};
```

### 股票问题集



#### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票。

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```



