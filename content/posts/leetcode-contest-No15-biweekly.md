---
title: LeetCode Contest No.15 Biweekly
comment: true
toc: true
tags:
  - LeetCode - Algorithm
mathjax: true
abbrlink: 38a01d69
updated: 2019-12-15 12:36:30
---
  
  
  
## Contest No.15 Biweekly

通过：4/4  排名：43 / 797  0:46:11  1WA

### No.5126 [有序数组中出现次数超过25%的元素](https://leetcode-cn.com/problems/element-appearing-more-than-25-in-sorted-array/)

>给你一个非递减的 有序 整数数组，已知这个数组中恰好有一个整数，它的出现次数超过数组元素总数的 25%。
>请你找到并返回这个整数
>
>示例：
>
>>输入：arr = [1,2,2,6,6,6,6,7,10]
>>输出：6
>
>提示：
>
>>- 1 <= arr.length <= 10^4
>>- 0 <= arr[i] <= 10^5

我很认真地按照题目意思去做的。。。后来翻看别人的答案恍然大悟，其实就是求众数，可以用`collections.Counter()` 加上 `most_common`就可以了

时间复杂度：$O(n)$

``` python
class Solution:
    def findSpecialInteger(self, arr: List[int]) -> int:
        a = len(arr)//4
        ln = 9999999999
        n = 0
        for num in arr:
            if ln == num:
                n+=1
            else:
                ln = num
                n = 1
            if n>a:
                return ln
```

``` python
class Solution:
    def findSpecialInteger(self, arr: List[int]) -> int:
        c = collections.Counter(arr)
        return c.most_common(1)[0][0]
```

<!-- more-->

### No.5127 [删除被覆盖区间](https://leetcode-cn.com/problems/remove-covered-intervals/)

>给你一个区间列表，请你删除列表中被其他区间所覆盖的区间。
>
>只有当 c <= a 且 b <= d 时，我们才认为区间 [a,b) 被区间 [c,d) 覆盖。
>
>在完成所有删除操作后，请你返回列表中剩余区间的数目。
>
>示例：
>
>>输入：intervals = \[[1,4],[3,6],[2,8]]
>>输出：2
>>解释：区间 [3,6] 被区间 [2,8] 覆盖，所以它被删除了。
>
>提示：​​​​​​
>
>>- 1 <= intervals.length <= 1000
>>- 0 <= intervals[i][0] < intervals[i][1] <= 10^5
>>- 对于所有的 i != j：intervals[i] != intervals[j]

将所有区间按照左侧从小到大进行排序，排序后只需要按照顺序判断相邻的两个是否会被覆盖，如果有被覆盖的关系，删除被覆盖的区间，不移动，继续判断当前位置两个区间。如果两个区间没有覆盖关系，向右侧移动直到末尾即可。

时间复杂度：$O(nlogn+n)$  
排序 $nlogn$ 遍历 $n$

``` python
class Solution:
    def removeCoveredIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort()
        i = 0
        while i<len(intervals)-1:
            if intervals[i][0]<=intervals[i+1][0] and intervals[i][1]>=intervals[i+1][1]:
                intervals.pop(i+1)
            elif intervals[i][0]>=intervals[i+1][0] and intervals[i][1]<=intervals[i+1][1]:
                intervals.pop(i)
            else:
                i+=1
        return i+1
```
  
### No.5123 [字母组合迭代器](https://leetcode-cn.com/problems/iterator-for-combination/)

>请你设计一个迭代器类，包括以下内容：
>
>一个构造函数，输入参数包括：一个 有序且字符唯一 的字符串 characters（该字符串只包含小写英文字母）和一个数字 combinationLength 。
>函数 next() ，按 字典序 返回长度为 combinationLength 的下一个字母组合。
>函数 hasNext() ，只有存在长度为 combinationLength 的下一个字母组合时，才返回 True；否则，返回 False。
>
>示例：
>
>>CombinationIterator iterator = new CombinationIterator("abc", 2); // 创建迭代器 iterator
>>
>>iterator.next(); // 返回 "ab"
>>iterator.hasNext(); // 返回 true
>>iterator.next(); // 返回 "ac"
>>iterator.hasNext(); // 返回 true
>>iterator.next(); // 返回 "bc"
>>iterator.hasNext(); // 返回 false
>
>提示：
>
>>- 1 <= combinationLength <= characters.length <= 15
>>- 每组测试数据最多包含 10^4 次函数调用。
>>- 题目保证每次调用函数 next 时都存在下一个字母组合。

这道题我偷懒了，用了python自带的itertools，但是发现偷懒的结果是自己用的不熟悉，所以还需要查，最后做这道题也没有变快。。。。itertools有四种排列组合的方式，还是蛮常用的，这道题是组合，用next获得迭代器下一个值返回，顺便自己记一下返回了多少个判断是否结束。

``` python
class CombinationIterator:

    def __init__(self, characters: str, combinationLength: int):
        cc = characters
        ll = combinationLength
        self.gen = itertools.combinations(characters, combinationLength)
        self.l = math.factorial(len(cc))//math.factorial(len(cc)-ll)//math.factorial(ll)

    def next(self) -> str:
        self.l-=1
        return ''.join(next(self.gen))

    def hasNext(self) -> bool:
        return self.l>0
```

所以出于良心的不安，我又写了下面这版：维护一个cur数组，用来代表下一个需要输出的每一个元素位置。存一个last数组，用来存储最后一种状态，反向遍历cur，只要不是最后一种状态就+1，就可以遍历所有可能性了。除此之外为了单独判断最后一种状态，将last数组的第一个数字加1，这样通过cur数组的第一个数字就可以判断是否已经遍历完成

``` python
class CombinationIterator:

    def __init__(self, characters: str, combinationLength: int):
        self.cc = characters
        self.ll = combinationLength
        self.cur = list(range(self.ll))
        self.last = list(range(len(self.cc) - self.ll,len(self.cc)))
        self.last[0] += 1

    def next(self) -> str:
        a = ""
        for i in self.cur:
            a+=self.cc[i]
        for i in range(self.ll)[::-1]:
            if self.cur[i] != self.last[i]:
                self.cur[i] += 1
                for k in range(i+1,len(self.cur)):
                    self.cur[k] = self.cur[k-1]+1
                break
        return a

    def hasNext(self) -> bool:
        return self.cur[0] <= len(self.cc) - self.ll
```
  
### No.5129 [下降路径最小和  II](https://leetcode-cn.com/problems/minimum-falling-path-sum-ii/)

>给你一个整数方阵 arr ，定义「非零偏移下降路径」为：从 arr 数组中的每一行选择一个数字，且按顺序选出来的数字中，相邻数字不在原数组的同一列。
>请你返回非零偏移下降路径数字和的最小值。
>
>示例 1：
>
>>输入：arr = \[[1,2,3],[4,5,6],[7,8,9]]
>>输出：13
>>解释：
>>所有非零偏移下降路径包括：
>>[1,5,9], [1,5,7], [1,6,7], [1,6,8],
>>[2,4,8], [2,4,9], [2,6,7], [2,6,8],
>>[3,4,8], [3,4,9], [3,5,7], [3,5,9]
>>下降路径中数字和最小的是 [1,5,7] ，所以答案是 13 。
>
>提示：
>
>>- 1 <= arr.length == arr[i].length <= 200
>>- -99 <= arr[i][j] <= 99

这道题是个DP，（第一时间题意理解错了浪费了很久。。。还在想这个这样不是DP啊）
状态转移方程
$$dp[m][n] = arr[m][n] + max(dp[m-1][n'])(n' \not= n)$$
时间复杂度：$O(n^2)$

``` python
class Solution:
    def minFallingPathSum(self, arr: List[List[int]]) -> int:
        dp = [[0]*len(arr) for _ in range(len(arr))]
        for i in range(len(arr)):
            dp[0][i] = arr[0][i]
        for i in range(1,len(arr)):
            for k in range(len(arr)):
                dp[i][k] = arr[i][k]+min(dp[i-1][:k]+dp[i-1][k+1:])
        return min(dp[-1])
```
