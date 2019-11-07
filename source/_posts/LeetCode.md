---
title: LeetCode
date: 2019-11-06 09:50:19
tags: LeetCode
---
## LeetCode

最近在学习python3，便想到了用python3刷LeetCode，既可以学习到python3的一些语法细节，也可以锻炼算法能力。

刷题未完全按照LeetCode排列顺序去刷，但是排布会按照题号大小去排列。算法小白，大佬看到勿喷。

<!-- more -->
<!-- toc -->

### 1.两数之和

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9，所以返回 [0, 1]

执行用时 :64 ms, 在所有 python3 提交中击败了93.62%的用户
        内存消耗 :15.5 MB, 在所有 python3 提交中击败了5.05%的用户

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        num_dict = {}
        for index, value in enumerate(nums):
            num_dict[value] = index

        for index, value in enumerate(nums):
            v = num_dict.get(target-value)
            if v is not None and v != index:
                return[index, v]
            
            
s = Solution()
nums = [3, 2, 4]
target = 6
print(s.twoSum(nums, target))
```

### 2.两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
		输出：7 -> 0 -> 8
		原因：342 + 465 = 807

执行用时 :72 ms, 在所有 python3 提交中击败了98.47%的用户
	    内存消耗 :13.8 MB, 在所有 python3 提交中击败了5.06%的用户

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 is None:
            return l2
        if l2 is None:
            return l1
        base = ListNode(0)
        tmp = base
        curry = 0  # 两数相加，大于10时的十位，即进位，需在下一位加上该值
        while l1 and l2:
            sum = l1.val + l2.val + curry
            index = sum % 10
            tmp.next = ListNode(index)
            curry = sum // 10
            l1 = l1.next
            l2 = l2.next
            tmp = tmp.next

        while l1:
            sum = l1.val + curry
            index = sum % 10
            tmp.next = ListNode(index)
            curry = sum // 10
            l1 = l1.next
            tmp = tmp.next

        while l2:
            sum = l2.val + curry
            index = sum % 10
            tmp.next = ListNode(index)
            curry = sum // 10
            l2 = l2.next
            tmp = tmp.next
        if(sum >= 10):
            tmp.next = ListNode(sum//10)
        base = base.next
        return base


s = Solution()
# l1 = 342
l1 = ListNode(2)
l1_next = l1.next = ListNode(4)
l1_next.next = ListNode(3)

# l2 = 465
l2 = ListNode(5)
l2_next = l2.next = ListNode(6)
l2_next.next = ListNode(4)
# l1 + l2 = 807
ret = s.addTwoNumbers(l1, l2)
while ret:
    print("ret:", ret.val)
    ret = ret.next
```

### 3.无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**示例 1**:

输入: "abcabcbb"

输出: 3

解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

**示例 2**:

输入: "bbbb"

输出: 1

解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

**示例 3**:

输入: "pwwkew"

输出: 3

解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。

请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

```python
# 暴力解法，两层for循环
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if len(s) == 1:
            return 1
        max_len = 0
        for i in range(0, len(s)):
            tmp_list = []
            tmp_list.append(s[i])
            for j in range(i+1, len(s)):
                if s[j] not in tmp_list:
                    tmp_list.append(s[j])
                else:
                    break
            if len(tmp_list) > max_len:
                max_len = len(tmp_list)

        return max_len
```

**思路优化**：

```python
class Solution:
    def removeKdigits(self, num: str, k: int) -> str:
        if len(num) == k:
            return '0'
        stack = [num[0]]
        for i in range(1, len(num)):
            while stack and stack[-1] > num[i] and k > 0:
                stack.pop()
                k -= 1
            stack.append(num[i])
        # k > 0说明剩余数字为全部相同或者递增如：11111或12345
        # 将后k个删除即可
        if k > 0:
            stack = stack[:-k]
        while True:
            if stack[0] == '0' and len(stack) > 1:
                stack = stack[1:]
            else:
                break
        return ''.join(stack)


num = "1234560"
k = 3
ret = Solution().removeKdigits(num, 6)
print(ret)
```

### 5. 最长回文子串-动态规划

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

**示例 1**：

输入: "babad"

输出: "bab"

注意: "aba" 也是一个有效答案。

**示例 2**：

输入: "cbbd"

输出: "bb"

**暴力解法**

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) < 2:
            return s
        max_len = 1
        result = s[0]
        for i in range(len(s)-1):
            for j in range(i+1, len(s)):
                is_back_to_text = self.isBackTotext(s, i, j)
                if is_back_to_text and j - i + 1 > max_len:
                    max_len = j - i + 1
                    result = s[i:j+1]

        return result

    def isBackTotext(self, s, left, right):
        while(left < right):
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
```

**动态规划**:

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        result = ''
        for i in range(len(s)):
            str1 = self.getLongestPalindrome(s, i, i)
            if len(str1) >len(result):
                result = str1
            str2 = self.getLongestPalindrome(s, i, i+1)
            if len(str2) >len(result):
                result = str2

        return result

    def getLongestPalindrome(self, s, left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1

        return s[left+1:right]


s = "babad"
ret = Solution().longestPalindrome(s)
print(ret)
```

### 7. 整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1**:

输入: 123

输出: 321

 **示例 2**:

输入: -123

输出: -321

**示例 3**:

输入: 120

输出: 21

注意:

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231, 231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

例如：

在python中 ：-53除以10=-6 …7 所以python中 -53%10=7

在c语言中，-53除以10=-5 … -3 所以c语言中 -53%10=-3

（python3中， /是精确除法，//是向下取整除法，%是求模，四舍五入取整round, 向零取整int, 向下和向上取整函数math.floor, math.ceil）

**解法一**：

```python
class Solution:
    def reverse(self, x: int) -> int:
        tmp = x
        if tmp < 0:
            tmp = -tmp
        str_tmp = str(tmp)
        str_tmp = str_tmp[::-1]
        int_tmp = int(str_tmp)
        if x < 0:
            int_tmp = -int_tmp
        if int_tmp <= -2**31 or int_tmp >= 2**31-1:
            return 0
        return int_tmp
```

**解法二**：

```python
class Solution:
    def reverse(self, x: int) -> int:
        if x < 0:
            num = -x
        else:
            num = x
        num_list = []
        while num:
            tmp = num % 10
            num_list.append(tmp)
            num = num // 10

        num_list.reverse()

        while num_list:
            tmp = num_list.pop()
            num = num * 10 + tmp
        if x < 0:
            num = -num
        if num <= -2**31 or num >= 2**31-1:
            return 0
        return num


ret = Solution().reverse(-123)
print(ret)
```

### 8. 字符串转换整数 (atoi)

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

**说明**：

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2**31, 2**31 − 1]。如果数值超过这个范围，请返回 INT_MAX (2**31 − 1) 或 INT_MIN (−2**31) 。

**示例 1**:

输入: "42"

输出: 42

**示例 2**:

输入: "  -42"

输出: -42

解释: 第一个非空白字符为 '-', 它是一个负号。

   我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。

**示例 3**:

输入: "4193 with words"

输出: 4193

解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

**示例 4**:

输入: "words and 987"

输出: 0

解释: 第一个非空字符是 'w', 但它不是数字或正、负号。

   因此无法执行有效的转换。

**示例 5**:

输入: "-91283472332"

输出: -2147483648

解释: 数字 "-91283472332" 超过 32 位有符号整数范围。

   因此返回 INT_MIN (−2**31) 。

```python
class Solution:
    def myAtoi(self, strs: str) -> int:
        if strs == "":
            return 0
        size = len(strs)
        num_list = []
        index = 0
        nums = set(str(i) for i in range(10))
        for i in range(size):
            if strs[i] == ' ':
                continue
            elif strs[i] == '-' or strs[i] == '+' or strs[i] in nums:
                index = i
                break
            else:
                return 0
        while True:
            num_list.append(strs[index])
            index += 1
            if index >= size or strs[index] not in nums:
                break
        ret_str = "".join(num_list)
        if len(ret_str) == 1 and ret_str[0] not in nums:
            return 0
        ret_num = int(ret_str)
        if ret_num <= -2**31 or ret_num > 2**31-1:
            if ret_num > 0:
                return 2147483647
            return -2147483648
        return ret_num


s = "2147483648"
ret = Solution().myAtoi(s)
print(ret)
```

### 9. 回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1**:

输入: 121

输出: true

**示例 2**:

输入: -121

输出: false

解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

**示例 3**:

输入: 10

输出: false

解释: 从右向左读, 为 01 。因此它不是一个回文数。

进阶:

你能不将整数转为字符串来解决这个问题吗？

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False
        num = x
        new_num = 0
        while num:
            tmp_num = num % 10
            new_num = new_num * 10 + tmp_num
            num = num // 10
        return new_num == x


num = 121
ret = Solution().isPalindrome(num)
print(ret)
```

### 13. 罗马数字转整数

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

| 字符 | 数值 |
| :--: | :--: |
|  I   |  1   |
|  V   |  5   |
|  X   |  10  |
|  L   |  50  |
|  C   | 100  |
|  D   | 500  |
|  M   | 1000 |

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做 XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。

同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。

X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 

C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

**示例 1**:

输入: "III"

输出: 3

**示例 2**:

输入: "IV"

输出: 4

**示例 3**:

输入: "IX"

输出: 9

**示例 4**:

输入: "LVIII"

输出: 58

解释: L = 50, V= 5, III = 3.

**示例 5**:

输入: "MCMXCIV"

输出: 1994

解释: M = 1000, CM = 900, XC = 90, IV = 4.

```python
# 暴力解法
class Solution:
    def romanToInt(self, s: str) -> int:
        num_dict = dict(I=1, V=5, X=10, L=50, C=100, D=500, M=1000, IV=4, IX=9, XL=40, XC=90, CD=400, CM=900)
        num_list = []
        i = 0
        while i < len(s):
            if i == (len(s) - 1):
                num_list.append(s[i])
                i += 1
                continue
            if s[i] == 'I' and (s[i+1] == 'V' or s[i+1] == 'X'):
                tmp = s[i] + s[i+1]
                num_list.append(tmp)
                i += 1
            elif s[i] == 'X' and (s[i+1] == 'L' or s[i+1] == 'C'):
                tmp = s[i] + s[i+1]
                num_list.append(tmp)
                i += 1
            elif s[i] == 'C' and (s[i+1] == 'D' or s[i+1] == 'M'):
                tmp = s[i] + s[i+1]
                num_list.append(tmp)
                i += 1
            else:
                num_list.append(s[i])
            i += 1

        num = 0
        for i in num_list:
            num = num + num_dict[i]
        return num
```

**优化-但速度并未明显提升**：

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        num_dict = dict(I=1, V=5, X=10, L=50, C=100, D=500, M=1000, IV=4, IX=9, XL=40, XC=90, CD=400, CM=900)
        num_set = set(('I', 'V', 'X', 'L', 'C', 'D', 'M', 'IV', 'IX', 'XL', 'XC', 'CD', 'CM'))
        index = 0
        num = 0
        while index < len(s):
            if index+1 < len(s) and s[index]+s[index+1] in num_set:
                num = num + num_dict[s[index] + s[index+1]]
                index += 2
            else:
                num = num + num_dict[s[index]]
                index += 1
        return num


s = "MCMXCIV"
ret = Solution().romanToInt(s)
print(ret)
```

### 14. 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

**示例 1**:

输入: ["flower","flow","flight"]

输出: "fl"

**示例 2**:

输入: ["dog","racecar","car"]

输出: ""

解释: 输入不存在公共前缀。

**说明**:

所有输入只包含小写字母 a-z 。

```python
class Solution:
    def longestCommonPrefix(self, strs: [str]) -> str:
        size = len(strs)
        if size == 0:
            return ""
        if size == 1:
            return strs[0]

        # 先找出前两个的公共前缀
        # 若存在，则用该前缀与后续str比较
        # 若不存在，则直接返回""
        index = 0
        if strs[0] == "" or strs[1] == "" or strs[0][0] != strs[1][0]:
            return ""
        while index < len(strs[0]) and index < len(strs[1]):
            if strs[0][index] == strs[1][index]:
                index += 1
            else:
                break
        public_str = strs[0][:index]
        for i in range(2, size):
            index = 0
            # 判断首位是否相同，不同直接返回""
            if strs[i] == "" or public_str[index] != strs[i][index]:
                return ""
            while index < len(public_str) and index < len(strs[i]):
                if public_str[index] == strs[i][index]:
                    index += 1
                else:
                    break
            public_str = public_str[0:index]
        return public_str
```

**大神解法**：

```python
class Solution:
    def longestCommonPrefix(self, strs: [str]) -> str:
        s = ""
        for i in zip(*strs):
            if len(set(i)) == 1:
                s += i[0]
            else:
                break
        return s


strs = ["flower", "flow", "flight"]
ret = Solution().longestCommonPrefix(strs)
print(ret)
```

### 20.有效的括号-栈

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。

左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

**示例 1**:

输入: "()"

输出: true

**示例 2**:

输入: "()[]{}"

输出: true

**示例 3**:

输入: "(]"

输出: false

**示例 4**:

输入: "([)]"

输出: false

**示例 5**:

输入: "{[]}"

输出: true

执行用时 :40 ms, 在所有 python3 提交中击败了91.98%的用户

内存消耗 :13.7 MB, 在所有 python3 提交中击败了5.51%的用户

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = []
        for i in s:
            if(len(stack) <= 0):
                stack.append(i)
                continue
            last = stack.pop()
            # 配对成功
            if (last == '(' and i == ')' or
                last == '[' and i == ']' or
                last == '{' and i == '}'):
                    print(last, " and ", i, "success!")
            # 配对上一个符号和当前符号一次入栈
            else:
                stack.append(last)
                stack.append(i)

        if(len(stack) > 0):
            return False
        return True

str_test = "([)]"
s = Solution()
print(s.isValid(str_test))
```

### 21. 合并两个有序链表

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例**：

输入：1->2->4, 1->3->4

输出：1->1->2->3->4->4

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 is None:
            return l2
        elif l2 is None:
            return l1

        head = ListNode(0)
        cur = head
        while l1 and l2:
            if l1.val < l2.val:
                cur.next = l1
                l1 = l1.next
            else:
                cur.next = l2
                l2 = l2.next
            cur = cur.next
        if l1:
            cur.next = l1
        elif l2:
            cur.next = l2
        return head.next


l1 = ListNode(1)
l1.next = ListNode(2)
l1_next = l1.next
l1_next.next = ListNode(4)
l2 = ListNode(1)
l2.next = ListNode(3)
l2_next = l2.next
l2_next.next = ListNode(4)
ret = Solution().mergeTwoLists(l1, l2)
while ret:
    print(ret.val)
    ret = ret.next
```

### 53. 最大子序和-动态规划

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例**:

输入: [-2,1,-3,4,-1,2,1,-5,4],

输出: 6

解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

```python
class Solution:
    def maxSubArray(self, nums: [int]) -> int:
        if max(nums) < 0:
            return max(nums)
        local_max = 0
        global_max = 0
        for num in nums:
            local_max = max(0, local_max+num)
            global_max = max(local_max, global_max)
        return global_max
```

### 55.跳跃游戏-贪婪算法

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

**示例 1**:

输入: [2,3,1,1,4]

输出: true

解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。

**示例 2**:

输入: [3,2,1,0,4]

输出: false

解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。

思路：从后往前走，看start所在位置+start所在位置可走步数能否大于end所在位置

   若大于，则说明start可以到达end位置，则end位置可往前移

   最终若end到达0点，则说明可能够到达最后一个位置。

```python
# 从后往前
class Solution:
    def canJump(self, nums: [int]) -> bool:
        end = len(nums) - 1
        start = len(nums) - 2
        while start >= 0:
            if nums[start] + start >= end:
                end = start
            start -= 1
        return end <= 0


s = Solution()
nums = [2, 3, 1, 1, 4]
ret = s.canJump(nums)
print(ret)
```

### 62. 不同路径-动态规划

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

说明：m 和 n 的值均不超过 100。

**示例 1**:

输入: m = 3, n = 2

输出: 3

解释:

从左上角开始，总共有 3 条路径可以到达右下角。

1. 向右 -> 向右 -> 向下

2. 向右 -> 向下 -> 向右

3. 向下 -> 向右 -> 向右

```python
class Solution:
    def uniquePaths(self, m, n):
        dp = [[1]*n] + [[1]+[0] * (n-1) for _ in range(m-1)]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
                return dp[m-1][n-1]


ret = Solution().uniquePaths(3, 3)
print(ret)
```

### 63.不同路径II

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。

说明：m 和 n 的值均不超过 100。

**示例 1**:

输入:

[ [0,0,0],

 [0,1,0],

 [0,0,0]]

输出: 2

解释:

3x3 网格的正中间有一个障碍物。

从左上角到右下角一共有 2 条不同的路径：

1. 向右 -> 向右 -> 向下 -> 向下

2. 向下 -> 向下 -> 向右 -> 向右

**思路**: 将障碍位置可以到达的方法设为0即可(该方法速度过慢)

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: [[int]]) -> int:
        if obstacleGrid[0][0] == 1:
            return 0
        # m 行 ,n 列
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        for i in range(m):
            for j in range(n):
                if i == 0 and j == 0:
                    obstacleGrid[i][j] = 1
                    continue
                if obstacleGrid[i][j] == 1:
                    obstacleGrid[i][j] = 0
                    continue
                if i == 0:
                    obstacleGrid[i][j] = obstacleGrid[i][j-1]
                elif j == 0:
                    obstacleGrid[i][j] = obstacleGrid[i-1][j]
                else:
                    obstacleGrid[i][j] = obstacleGrid[i-1][j] + obstacleGrid[i][j-1]
        return obstacleGrid[m-1][n-1]
```

**优化思路**:将0,0位置的赋值，以及第一行和第一列的赋值提出来,避免双层循环内大量if判断降低运行速度.

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: [[int]]) -> int:
        if obstacleGrid[0][0] == 1:
            return 0
        # m 行 ,n 列
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        obstacleGrid[0][0] = 1
        # 将第一行数据赋值
        for j in range(1, n):
            obstacleGrid[0][j] = 1 if obstacleGrid[0][j] == 0 and obstacleGrid[0][j-1] == 1 else 0

        # 将第一列数据赋值
        for i in range(1, m):
            obstacleGrid[i][0] = 1 if obstacleGrid[i][0] == 0 and obstacleGrid[i-1][0] == 1 else 0

        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 1:
                    obstacleGrid[i][j] = 0
                    continue
                if i == 0:
                    obstacleGrid[i][j] = obstacleGrid[i][j-1]
                elif j == 0:
                    obstacleGrid[i][j] = obstacleGrid[i-1][j]
                else:
                    obstacleGrid[i][j] = obstacleGrid[i-1][j] + obstacleGrid[i][j-1]
        return obstacleGrid[m-1][n-1]


s = [[0, 0, 0],
     [0, 1, 0],
     [0, 0, 0]]
ret = Solution().uniquePathsWithObstacles(s)
print(ret)
```

### 64. 最小路径和

给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

**示例**:

输入:

[ [1,3,1],

 [1,5,1],

 [4,2,1]]

输出: 7

解释: 因为路径 1→3→1→1→1 的总和最小。

```python
class Solution:
    def minPathSum(self, grid: [[int]]) -> int:
        m = len(grid)     # 行
        n = len(grid[0])  # 列
        # 先把第一列算出来
        for i in range(1, m):
            grid[i][0] = grid[i][0] + grid[i-1][0]

        # 再把第一行算出来
        for j in range(1, n):
            grid[0][j] = grid[0][j] + grid[0][j-1]

        for i in range(1, m):
            for j in range(1, n):
                # 比较该位置上方或左方数字，将较小值加给该位置
                # if grid[i-1][j] < grid[i][j-1]:
                #     num = grid[i-1][j]
                # else:
                #     num = grid[i][j-1]
                num = grid[i-1][j] if grid[i-1][j] < grid[i][j-1] else grid[i][j-1]
                grid[i][j] += num

        return grid[m-1][n-1]


s = [[1, 3, 1],
     [1, 5, 1],
     [4, 2, 1]]
ret = Solution().minPathSum(s)
print(ret)
```

### 70. 爬楼梯

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

**示例 1**：

输入： 2

输出： 2

解释： 有两种方法可以爬到楼顶。

1. 1 阶 + 1 阶

2. 2 阶

**示例 2**：

输入： 3

输出： 3

解释： 有三种方法可以爬到楼顶。

1. 1 阶 + 1 阶 + 1 阶

2. 1 阶 + 2 阶

3. 2 阶 + 1 阶

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        climbNums = [1 for _ in range(n)]
        climbNums[0] = 1
        climbNums[1] = 2
        for i in range(2, len(climbNums)):
            climbNums[i] = climbNums[i-2] + climbNums[i-1]
        return climbNums[-1]


ret = Solution().climbStairs(6)
print(ret)
```

### 71.简化路径-栈

以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：Linux / Unix中的绝对路径 vs 相对路径

请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

**示例 1**：

输入："/home/"

输出："/home"

解释：注意，最后一个目录名后面没有斜杠。

**示例 2**：

输入："/../"

输出："/"

解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。

**示例 3**：

输入："/home//foo/"

输出："/home/foo"

解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。

**示例 4**：

输入："/a/./b/../../c/"

输出："/c"

**示例 5**：

输入："/a/../../b/../c//.//"

输出："/c"

**示例 6**：

输入："/a//b////c/d//././/.."

输出："/a/b/c"

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        stack = []
        path_list = path.split('/')
        for i in path_list:
            if len(stack) > 0 and i == "..":
                stack.pop()
            elif i != '.' and i != '' and i != '..':
                stack.append(i)
        return '/' + '/'.join(stack)


s = Solution()
print(s.simplifyPath("/../"))
```

### 94.二叉树的中序遍历

给定一个二叉树，返回它的中序 遍历。

示例:

输入: [1,null,2,3]

```python
  1
   \
    2
   /
  3
```

输出: [1,3,2]

执行用时 :44 ms, 在所有 python3 提交中击败了75.75%的用户

内存消耗 :13.7 MB, 在所有 python3 提交中击败了5.32%的用户

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class Solution:
    def inorderTraversal(self, root: TreeNode):
        if not root:
            return []
        ret, stack = [], []
        while True:
            while root:
                stack.append(root)
                root = root.left
            if not stack:
                return ret
            node = stack.pop()
            if node.val is not None:
                ret.append(node.val)
            root = node.right
        return ret


t = TreeNode(1)
t.left = None
t.right = TreeNode(2)
t2 = t.right
t2.left = TreeNode(3)
t2.right = TreeNode(None)
t3 = t2.left
t3.left = None
t3.right = None
s = Solution()
ret = s.inorderTraversal(t)
print(ret)
```

### 122.买卖股票的最佳时机 II-贪婪算法

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1**:

输入: [7,1,5,3,6,4]

输出: 7

解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。

   随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。

**示例 2**:

输入: [1,2,3,4,5]

输出: 4

解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。

   注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。

   因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

**示例 3**:

输入: [7,6,4,3,1]

输出: 0

解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

思路：所有上涨交易日都买卖，所有下降交易日都不买卖

   “等价于每天都买卖”，把可能跨越多天的买卖都化解成相邻两天的买卖

```python
class Solution:
    def maxProfit(self, prices: [int]) -> int:
        sum = 0
        for i in range(1, len(prices)):
            tmp = prices[i] - prices[i-1]
            if tmp > 0:
                sum += tmp
        return sum


s = Solution()
price = [1, 2, 3, 4, 5]
ret = s.maxProfit(price)
print(ret)
```

### 134.加油站-贪婪算法

在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

说明: 

如果题目有解，该答案即为唯一答案。

输入数组均为非空数组，且长度相同。

输入数组中的元素均为非负数。

**示例 1**:

输入:

gas = [1,2,3,4,5]

cost = [3,4,5,1,2]

输出: 3

解释:

从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油

开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油

开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油

开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油

开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油

开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。

因此，3 可为起始索引。

**示例 2**:

输入:

gas = [2,3,4]

cost = [3,4,3]

输出: -1

解释:

你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。

我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油

开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油

开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油

你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。

因此，无论怎样，你都不可能绕环路行驶一周。

这道题还有些不清楚

```python
class Solution(object):
    def canCompleteCircuit(self, gas, cost):
        """
        :type gas: List[int]
        :type cost: List[int]
        :rtype: int
        """
        location = 0
        total_sum = 0
        # 如果每个站点加的油总量和小于消耗的总油量，则肯定环绕不了一周
        if sum(gas) < sum(cost):
            return -1
        for index in range(len(gas)):
            total_sum += gas[index] - cost[index]
            if total_sum < 0:
                location = index+1
                total_sum = 0
        return location


ret = Solution().canCompleteCircuit([5, 8, 2, 8], [6, 5, 6, 6])
print(ret)
```

### 215.数组中的第K个最大元素-堆

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**示例 1**:

输入: [3,2,1,5,6,4] 和 k = 2

输出: 5

**示例 2**:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4

输出: 4

说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

```python
import heapq


class Solution:
    def findKthLargest(self, nums: [int], k: int) -> int:
        heap = []
        for num in nums[:k]:
            heapq.heappush(heap, num)
        for num in nums[k:]:
            if num > heap[0]:
                heapq.heappop(heap)
                heapq.heappush(heap, num)
        return heap[0]


nums = [3, 2, 1, 5, 6, 4]
s = Solution()
ret = s.findKthLargest(nums, 2)
print(ret)
```

### 264.丑数II-堆

编写一个程序，找出第 n 个丑数。

丑数就是只包含质因数 2, 3, 5 的正整数。

**示例**:

输入: n = 10

输出: 12

解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。

**说明**: 

1 是丑数。

n 不超过1690。

因为丑数是2, 3, 5的倍数，我们不断把它们的倍数压入栈中，再按顺序弹出！

时间复杂度：nlogn

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        import heapq
        heap = []
        heapq.heappush(heap, 1)
        for _ in range(n):
            ret = heapq.heappop(heap)
            while heap and ret == heap[0]:
                ret = heapq.heappop(heap)
            a, b, c = ret*2, ret*3, ret*5
            for i in [a, b, c]:
                heapq.heappush(heap, i)
        return ret


s = Solution()
ret = s.nthUglyNumber(9)
print(ret)
```

### 313.超级丑数-堆

编写一段程序来查找第 n 个超级丑数。

超级丑数是指其所有质因数都是长度为 k 的质数列表 primes 中的正整数。

**示例:**

输入: n = 12, primes = [2,7,13,19]

输出: 32

解释: 给定长度为 4 的质数列表 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。

**说明**:

1 是任何给定 primes 的超级丑数。

 给定 primes 中的数字以升序排列。

0 < k ≤ 100, 0 < n ≤ 106, 0 < primes[i] < 1000 。

第 n 个超级丑数确保在 32 位有符整数范围内。

```python
class Solution:
    def nthSuperUglyNumber(self, n: int, primes: [int]) -> int:
        import heapq
        heap = []
        heapq.heappush(heap, 1)
        for _ in range(n):
            ret = heapq.heappop(heap)
            while heap and ret == heap[0]:
                ret = heapq.heappop(heap)
            for i in primes:
                heapq.heappush(heap, ret*i)
        return ret


s = Solution()
primes = [2, 7, 13, 19]
ret = s.nthSuperUglyNumber(12, primes)
print(ret)
```

### 347.前 K 个高频元素-堆

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

**示例 1**:

输入: nums = [1,1,1,2,2,3], k = 2

输出: [1,2]

**示例 2**:

输入: nums = [1], k = 1

输出: [1]

**说明**：

你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。

你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。

```python
# 自己的方法
class Solution:
    def topKFrequent(self, nums, k):
        num_dict = {}
        for i in nums:
            if i not in num_dict:
                num_dict[i] = 1
                continue
            num_dict[i] += 1
        ret = sorted(num_dict.items(), key=lambda x: x[1], reverse=True)
        ret = ret[:k]
        ret = [x[0] for x in ret]
        return ret
        
s = Solution()
nums = [1, 1, 1, 1, 2, 2, 3]
ret = s.topKFrequent(nums, 2)
print(ret)
```

```python
# 用堆
class Solution:
    def topKFrequent(self, nums, k):
        import collections
        import heapq
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        count = collections.Counter(nums)
        return heapq.nlargest(k, count.keys(), key=count.get)


s = Solution()
nums = [1, 1, 1, 1, 2, 2, 3]
ret = s.topKFrequent(nums, 2)
print(ret)
```

### 376.摆动序列-贪婪算法

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。

例如, [1,7,4,9,2,5] 是一个摆动序列，因为差值 (6,-3,5,-7,3) 是正负交替出现的。

相反, [1,4,7,2,5] 和 [1,7,4,5,5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

给定一个整数序列，返回作为摆动序列的最长子序列的长度。 通过从原始序列中删除一些（也可以不删除）元素来获得子序列，剩下的元素保持其原始顺序。

**示例 1**:

输入: [1,7,4,9,2,5]

输出: 6

解释: 整个序列均为摆动序列。

**示例 2**:

输入: [1,17,5,10,13,15,10,5,16,8]

输出: 7

解释: 这个序列包含几个长度为 7 摆动序列，其中一个可为[1,17,10,13,10,16,8]。

**示例 3**:

输入: [1,2,3,4,5,6,7,8,9]

输出: 2

进阶:

你能否用 O(n) 时间复杂度完成此题?

```python
class Solution:
    def wiggleMaxLength(self, nums: [int]) -> int:
        if len(nums) < 2:
            return len(nums)
        max_len_list = [nums[0]]
        UP = False
        DOWN = False
        for i in range(0, len(nums) - 1):
            if UP is False and nums[i+1] - nums[i] > 0:
                UP = True
                DOWN = False
                max_len_list.append(nums[i+1])
            elif UP is True and nums[i+1] - nums[i] > 0:
                # 持续上升，替换为最大值
                max_len_list[-1] = nums[i+1]
            elif DOWN is False and nums[i+1] - nums[i] < 0:
                DOWN = True
                UP = False
                max_len_list.append(nums[i+1])
            elif DOWN is True and nums[i+1] - nums[i] < 0:
                max_len_list[-1] = nums[i+1]
        return len(max_len_list)


nums = [1,7,4,9,2,5]
ret = Solution().wiggleMaxLength(nums)
print(ret)
```

### 392.判断子序列-贪婪算法

给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ~= 500,000），而 s 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

**示例 1**:

s = "abc", t = "ahbgdc"

返回 true.

**示例 2**:

s = "axc", t = "ahbgdc"

返回 false.

后续挑战 :

如果有大量输入的 S，称作S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if s == "":
            return True
        for i in t:
            if s[0] == i:
                s = s[1:]
                if len(s) <= 0:
                    return True
        return False


s = Solution()
ret = s.isSubsequence("", "ahbgdc")
print(ret)
```

### 402.移掉K位数字-贪婪算法

给定一个以字符串表示的非负整数 num，移除这个数中的 k 位数字，使得剩下的数字最小。

注意:

num 的长度小于 10002 且 ≥ k。

num 不会包含任何前导零。

**示例 1** :

输入: num = "1432219", k = 3

输出: "1219"

解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。

**示例 2** :

输入: num = "10200", k = 1

输出: "200"

解释: 移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。

**示例 3** :

输入: num = "10", k = 2

输出: "0"

解释: 从原数字移除所有的数字，剩余为空就是0。

利用桟维持一个递增的序列，也就是说将字符串中字符依次入栈，

如果当前字符比栈顶元素小，并且还可以继续删除元素，那么就将栈顶元素移掉，且继续向下一个栈顶元素比较，尽量维持序列递增，也可以算是一个贪心思想。

```python
class Solution:
    def removeKdigits(self, num: str, k: int) -> str:
        if len(num) == k:
            return '0'
        stack = [num[0]]
        for i in range(1, len(num)):
            while stack and stack[-1] > num[i] and k > 0:
                stack.pop()
                k -= 1
            stack.append(num[i])
        # k > 0说明剩余数字为全部相同或者递增如：11111或12345
        # 将后k个删除即可
        if k > 0:
            stack = stack[:-k]
        while True:
            if stack[0] == '0' and len(stack) > 1:
                stack = stack[1:]
            else:
                break
        return ''.join(stack)


num = "1234560"
k = 3
ret = Solution().removeKdigits(num, 6)
print(ret)
```

### 455.分发饼干-贪婪算法

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。对每个孩子 i ，都有一个胃口值 gi ，

这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j ，都有一个尺寸 sj 。如果 sj >= gi ，我们可以将这个饼干 j 分配给孩子 i ，

这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

注意：

你可以假设胃口值为正。

一个小朋友最多只能拥有一块饼干。

**示例 1**:

输入: [1,2,3], [1,1]

输出: 1

解释:

你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。

虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。

所以你应该输出1。

**示例 2**:

输入: [1,2], [1,2,3]

输出: 2

解释:

你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。

你拥有的饼干数量和尺寸都足以让所有孩子满足。

所以你应该输出2.

```python
class Solution:
    def findContentChildren(self, g: [int], s: [int]) -> int:
        child_list = sorted(g)  # 小朋友
        cookie_list = sorted(s)  # 小饼干
        num = 0
        child_index = 0
        cookie_index = 0
        while child_index < len(child_list) and cookie_index < len(cookie_list):
            if cookie_list[cookie_index] >= child_list[child_index]:
                num += 1
                child_index += 1
            cookie_index += 1
        return num


s = Solution()
ret = s.findContentChildren([10, 9, 8, 7], [5, 6, 7, 8])
print(ret)
```

### 703.数据流中的第K大元素-堆

设计一个找到数据流中第K大元素的类（class）。注意是排序后的第K大元素，不是第K个不同的元素。

你的 KthLargest 类需要一个同时接收整数 k 和整数数组nums 的构造器，它包含数据流中的初始元素。每次调用 KthLargest.add，返回当前数据流中第K大的元素。

**示例**:

```c++
int k = 3;
int[] arr = [4,5,8,2];
KthLargest kthLargest = new KthLargest(3, arr);
kthLargest.add(3);  // returns 4
kthLargest.add(5);  // returns 5
kthLargest.add(10); // returns 5
kthLargest.add(9);  // returns 8
kthLargest.add(4);  // returns 8
```

执行用时 :120 ms, 在所有 python3 提交中击败了95.49%的用户

内存消耗 :17.4 MB, 在所有 python3 提交中击败了5.63%的用户

```python
class KthLargest:

    def __init__(self, k: int, nums: [int]):
        import heapq
        self.k = k
        self.nums = nums[:k]
        heapq.heapify(self.nums)
        for i in nums[k:]:
            if i > self.nums[0]:
                heapq.heappop(self.nums)
                heapq.heappush(self.nums, i)

    def add(self, val: int) -> int:
        import heapq
        if len(self.nums) < self.k:
            heapq.heappush(self.nums, val)
        elif val > self.nums[0]:
            heapq.heappop(self.nums)
            heapq.heappush(self.nums, val)
        return self.nums[0]


k = 3
arr = [4, 5, 8, 2]
s = KthLargest(k, arr)
ret = s.add(3)
print(ret)  # 4
ret = s.add(5)
print(ret)  # 5
ret = s.add(10)
print(ret)  # 5
```

### 1021.删除最外层的括号-栈

有效括号字符串为空 ("")、"(" + A + ")" 或 A + B，其中 A 和 B 都是有效的括号字符串，+ 代表字符串的连接。例如，""，"()"，"(())()" 和 "(()(()))" 都是有效的括号字符串。

如果有效字符串 S 非空，且不存在将其拆分为 S = A+B 的方法，我们称其为原语（primitive），其中 A 和 B 都是非空有效括号字符串。

给出一个非空有效字符串 S，考虑将其进行原语化分解，使得：S = P_1 + P_2 + ... + P_k，其中 P_i 是有效括号字符串原语。

对 S 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 S 。

**示例 1**：

输入："(()())(())"

输出："()()()"

解释：

输入字符串为 "(()())(())"，原语化分解得到 "(()())" + "(())"，

删除每个部分中的最外层括号后得到 "()()" + "()" = "()()()"。

**示例 2**：

输入："(()())(())(()(()))"

输出："()()()()(())"

解释：

输入字符串为 "(()())(())(()(()))"，原语化分解得到 "(()())" + "(())" + "(()(()))"，

删除每隔部分中的最外层括号后得到 "()()" + "()" + "()(())" = "()()()()(())"。

**示例 3**：

输入："()()"

输出：""

解释：

输入字符串为 "()()"，原语化分解得到 "()" + "()"，

删除每个部分中的最外层括号后得到 "" + "" = ""。

**提示**：

S.length <= 10000

S[i] 为 "(" 或 ")"

S 是一个有效括号字符串

```python
class Solution:
    def removeOuterParentheses(self, S: str) -> str:
        stack = []
        ret_str = ''
        for i in S:
            if i == '(':
                if(len(stack) >= 1):
                    ret_str += i
                stack.append(i)
            else:  # i == ')'
                if(len(stack) > 1):
                    ret_str += i
                stack.pop()
        return ret_str


s = Solution()
s_test = "(()())(())(()(()))"
print(s.removeOuterParentheses(s_test))
```

### 1047.删除字符串中的所有相邻重复项-栈

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

**示例**：

输入："abbaca"

输出："ca"

解释：

例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

**提示**：

1 <= S.length <= 20000

S 仅由小写英文字母组成。

```python
class Solution:
    def removeDuplicates(self, S: str) -> str:
        stack = []
        for i in S:
            if stack and stack[-1] == i:
                stack.pop()
            else:
                stack.append(i)

        ret_str = ''.join(stack)
        return ret_str

s = Solution()
print(s.removeDuplicates("abbaca"))
```

