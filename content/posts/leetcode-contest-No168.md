---
title: LeetCode Contest No.168
comment: true
toc: true
tags:
  - LeetCode
  - Algorithm
mathjax: true
abbrlink: 4dc6fe2a
---
  
  
  
## Contest 168

通过：4/4  排名：61 / 1552  1:08:39  1WA

### No.5291 [统计位数为偶数的数字](https://leetcode-cn.com/problems/find-numbers-with-even-number-of-digits/)

>给你一个整数数组 nums，请你返回其中位数为 偶数 的数字的个数。
>
>**示例 1：**
>
>>**输入：** nums = [12,345,2,6,7896]
>>**输出：** 2
>>**解释：**
>>12 是 2 位数字（位数为偶数）
>>345 是 3 位数字（位数为奇数）  
>>2 是 1 位数字（位数为奇数）
>>6 是 1 位数字 位数为奇数）
>>7896 是 4 位数字（位数为偶数）  
>>因此只有 12 和 7896 是位数为偶数的数字
>
>**示例 2：**
>
>>**输入：** nums = [555,901,482,1771]
>>**输出：** 1
>>**解释：**
>>只有 1771 是位数为偶数的数字。
>
>**提示：**
>
>>- 1 <= nums.length <= 500
>>- 1 <= nums[i] <= 10^5

嗯，水题。（但是大佬一行代码。。）
时间复杂度：$O(n)$

``` python
class Solution:
    def findNumbers(self, nums: List[int]) -> int:
        ans = 0
        for num in nums:
            if len(str(num))%2 == 0:
                ans+=1
        return ans
```

<!-- more -->

### No.5292 [划分数组为连续数字的集合](https://leetcode-cn.com/problems/divide-array-in-sets-of-k-consecutive-numbers/)

>给你一个整数数组 nums 和一个正整数 k，请你判断是否可以把这个数组划分成一>些由 k 个连续数字组成的集合。
>如果可以，请返回 True；否则，返回 False。
>
>**示例 1：**
>
>>**输入：** nums = [1,2,3,3,4,4,5,6], k = 4
>>**输出：** true
>>**解释：** 数组可以分成 [1,2,3,4] 和 [3,4,5,6]。
>
>**示例 2：**
>
>>**输入：** nums = [3,2,1,2,3,4,3,4,5,9,10,11], k = 3
>>**输出：** true
>>**解释：** 数组可以分成 [1,2,3] , [2,3,4] , [3,4,5] 和 [9,10,11]。
>
>**示例 3：**
>
>>**输入：** nums = [3,3,2,2,1,1], k = 3
>>**输出：** true
>
>**示例 4：**
>
>>**输入：** nums = [1,2,3,4], k = 3
>>**输出：** false
>>**解释：** 数组不能分成几个大小为 3 的子数组。
>
>**提示：**
>
>>- 1 <= nums.length <= 10^5
>>- 1 <= nums[i] <= 10^9
>>- 1 <= k <= nums.length

将所有数字进行排序，排序后选出最小的数字，组成一个分组（因为如果能分组成功的话，这个最小的数字在组内也必定是最小的数字），然后将这个分组的数字移除，重复过程直到不能进行分组或者全部数字分组完成。

时间复杂度： $O(nlgn+n)$

``` python
class Solution:
    def isPossibleDivide(self, nums: List[int], k: int) -> bool:
        ans = len(nums)
        if ans%k != 0:
            return False
        dd = {}
        nums.sort()
        for n in nums:
            dd[n] = dd.get(n,0)+1
        while len(dd)!=0:
            a = min(dd.keys())
            for i in range(a,a+k):
                if i in dd:
                    dd[i]-=1
                    if dd[i] == 0:
                        del dd[i]
                else:
                    return False
        return True
```
  
### No.5293 [子串的最大出现次数](https://leetcode-cn.com/problems/maximum-number-of-occurrences-of-a-substring/)

>给你一个字符串 s ，请你返回满足以下条件且出现次数最大的 任意 子串的出现次数：
>
>子串中不同字母的数目必须小于等于 maxLetters 。
>子串的长度必须大于等于 minSize 且小于等于 maxSize 。
>
>**示例 1：**
>
>>**输入：** s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
>>**输出：** 2
>>**解释：** 子串 "aab" 在原字符串中出现了 2 次。
>>它满足所有的要求：2 个不同的字母，长度为 3 （在 minSize 和 maxSize 范围内）。
>
>**示例 2：**
>
>>**输入：** s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
>>**输出：** 2
>>**解释：** 子串 "aaa" 在原字符串中出现了 2 次，且它们有重叠部分。
>
>**示例 3：**
>
>>**输入：** s = "aabcabcab", maxLetters = 2, minSize = 2, maxSize = 3
>>**输出：** 3
>
>**示例 4：**
>
>>**输入：** s = "abcde", maxLetters = 2, minSize = 3, maxSize = 3
>>**输出：** 0
>
>**提示：**
>
>>- 1 <= s.length <= 10^5
>>- 1 <= maxLetters <= 26
>>- 1 <= minSize <= maxSize <= min(26, s.length)
>>- s 只包含小写英文字母。

这道题最开始理解错题意了，题目要求是满足条件的字串中出现次数最多的字串的数量。我理解成了所有可能满足条件的字串的数量。这样分析的话`maxSize`其实是没有用的因为只要不是以`minSize`组成的子串，必然能选出`minSize`大小的字串出现数量大于等于当前字串的出现数量。所以该题目简化后变成，遍历字符串中所有`minSize`长度的字串，在满足条件的字串中查找出现数量最多的子串的出现数量。

时间复杂度：$O(n \times minSize)$

``` python
class Solution:
    def maxFreq(self, s: str, maxLetters: int, minSize: int, maxSize: int) -> int:
        a = []
        for i in range(len(s)-minSize+1):
            if len(set(s[i:i+minSize])) <= maxLetters:
                a.append(s[i:i+minSize])
        b = collections.Counter(a).most_common(1)
        return b[0][1] if b else 0
```
  
### No.5294 [你能从盒子里获得的最大糖果数](https://leetcode-cn.com/problems/maximum-candies-you-can-get-from-boxes/)

这个题目太吓人了，好长，输入量也很多。但是感觉只要注意细节其实比第三题简单的（日常第三题比第四题难）。我们要做的就是遍历已有的没开的`box`，依次打开并拿出里面的盒子、糖果、钥匙，然后根据盒子和钥匙再继续向下开，中间记录一下已经开过的盒子，避免重复开盒子计数即可。当某次遍历时不能拿到新的盒子的话，就算已经把能开的都开完了。

``` python
class Solution:
    def maxCandies(self, status: List[int], candies: List[int], keys: List[List[int]], containedBoxes: List[List[int]], initialBoxes: List[int]) -> int:
        mybox = initialBoxes
        mykeys = []
        openedbox = set([])
        ans = 0
        while mybox:
            llen = len(mybox)
            for box in mybox:
                if (status[box] == 1 or (status[box] == 0 and box in mykeys)) and box not in openedbox:
                    mykeys = mykeys+keys[box]
                    ans+=candies[box]
                    mybox+=containedBoxes[box]
                    openedbox.add(box)
            if llen == len(mybox):
                return ans
        return 0
```
