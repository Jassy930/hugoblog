---
title: LeetCode Contest No.167
comment: true
toc: true
tags:
  - LeetCode
  - Algorithm
mathjax: true
abbrlink: dd79e3bb
updated: 2019-12-15 12:29:59
---
  
  
  
## Contest 167

通过：4/4  排名：107 / 1534  1:43:12  5WA

### No.5283 [二进制链表转整数](https://leetcode-cn.com/problems/convert-binary-number-in-a-linked-list-to-integer/)

>给你一个单链表的引用结点 head。链表中每个结点的值不是 0 就是 1。已知此链表是一个整数数字的二进制表示形式。
>请你返回该链表所表示数字的 十进制值 。
>
>示例 1：
>
>>输入：head = [1,0,1]
>>输出：5
>>解释：二进制数 (101) 转化为十进制数 (5)
>
>示例 2：
>
>>输入：head = [0]
>>输出：0
>
>示例 3：
>
>>输入：head = [1]
>>输出：1
>
>示例 4：
>
>>输入：head = [1,0,0,1,0,0,1,1,1,0,0,0,0,0,0]
>>输出：18880
>
>示例 5：
>
>>输入：head = [0,0]
>>输出：0
>
>提示：
>
>>- 链表不为空。
>>- 链表的结点总数不超过 30。
>>- 每个结点的值不是 0 就是 1。

嗯，水题。
时间复杂度：$O(n)$

``` python []
class Solution:
    def getDecimalValue(self, head: ListNode) -> int:
        ans = 0
        while head!=None:
            ans = ans*2+head.val
            head = head.next
        return ans
```



### No.5124 [顺次数](https://leetcode-cn.com/problems/sequential-digits/)

>我们定义「顺次数」为：每一位上的数字都比前一位上的数字大 1 的整数。
>请你返回由 [low, high] 范围内所有顺次数组成的 有序 列表（从小到大排序）。
>
>示例 1：
>
>>输出：low = 100, high = 300
>>输出：[123,234]
>
>示例 2：
>
>>输出：low = 1000, high = 13000
>>输出：[1234,2345,3456,4567,5678,6789,12345]
>
>提示：
>
>>- 10 <= low <= high <= 10^9

这个题经过了一点点思考，本来的想法还是从low遍历到high的，后来想不对啊，这数字只要第一个数字定了就都定了，所以大概定了个范围，把范围内的所有可能的数列出来，选出来low到high之间的数就可以了
后来发现大佬们的眼睛更尖一些，所有可能组出来的数字，最多只有36种可能性，所以最快的方法是将36个数在本地打表写在一个数组里，然后判断在low和high中间的就完事了，具体代码不写了

时间复杂度（大佬版）： $O(1)$

``` python
class Solution:
    def sequentialDigits(self, low: int, high: int) -> List[int]:
        ans = []
        num = ['1','2','3','4','5','6','7','8','9']
        lc = 0
        hc = 0
        ll = low
        hh = high
        while low>0:
            lc+=1
            low = low//10
        while high>0:
            hc+=1
            high = high//10
        for k in range(lc,hc+1):
            for i in range(9-k+1):
                ans.append(int(''.join(num[i:i+k])))
        o = []
        for a in ans:
            if a >=ll and a<=hh:
                o.append(a)
        return o
```
  
### No.5285 [元素和小于等于阈值的正方形的最大边长](https://leetcode-cn.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/)

>给你一个大小为 m x n 的矩阵 mat 和一个整数阈值 threshold。
>请你返回元素总和小于或等于阈值的正方形区域的最大边长；如果没有这样的正方形区域，则返回 0 。
>
>示例 1：
>
>>输入：mat = \[[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4
>>输出：2
>>解释：总和小于 4 的正方形的最大边长为 2。
>
>示例 2：
>
>>输入：mat = \[[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1
>>输出：0
>
>示例 3：
>
>>输入：mat = \[[1,1,1,1],[1,0,0,0],[1,0,0,0],[1,0,0,0]], threshold = 6
>>输出：3
>
>示例 4：
>
>>输入：mat = \[[18,70],[61,1],[25,85],[14,40],[11,96],[97,96],[63,45]], threshold = 40184
>>输出：2
>
>提示：
>
>>- 1 <= m, n <= 300
>>- m == mat.length
>>- n == mat[i].length
>>- 0 <= mat[i][j] <= 10000
>>- 0 <= threshold <= 10^5

第一次写写了五层循环。。。头太铁了，然后想到之前图像处理里面积分图的应用想到可以把整个二维数据累加，这样计算任意区块的累加和只需要用积分好的值加加减减就可以了，这样可以节省掉两层循环（其实应该叫前缀和）。除此之外另外有几点优化：1. 当边长为i的某一次验证没问题之后，直接跳到下一个i进行验证，不遍历其他的位置；2. 当某次边长遍历后没有符合项，直接返回当前最长的边长，因为更长的不可能满足要求了。

后来看别人的题解，其实还有可以优化的，一个是边长的遍历上，改成二分法遍历（因为边长是有界的，这样想的话其实所有题目只要是有界的遍历都改成二分，时间复杂度都是由$O(n)变成$O(lgn)$），另外一种方法是边长从大到小进行遍历，这样只要出现符合条件的就可以直接返回了，不需要遍历剩下的小的，因为题目要求返回**最大**的值。

所以记一下就是，只要是有界条件下的遍历，都用二分就好了。以前某道题也是，费了很多功夫计算公式去缩小遍历范围，但是不如直接从0到1000000000000直接二分计算。

时间复杂度：$O(n^3)$
二分的时间复杂度：$O(n^2lgn)$

``` python
class Solution:
    def maxSideLength(self, mat: List[List[int]], threshold: int) -> int:
        for i in range(len(mat)):
            for k in range(1,len(mat[0])):
                mat[i][k] += mat[i][k-1]
        for k in range(len(mat[0])):
            for i in range(1,len(mat)):
                mat[i][k] += mat[i-1][k]
        m = [[0]*(len(mat[0])+1)] +[[0]+mat[x] for x in range(len(mat))]
        mat = m
        ans = -1
        lans = ans
        for i in range(min(len(mat),len(mat[0]))):
            m = 0
            while m<len(mat)-i:
                n = 0
                while n<len(mat[0])-i:
                    sm = mat[m+i][n+i] + mat[m][n] - mat[m][n+i] - mat[m+i][n]
                    if sm <= threshold and i>ans:
                        ans = i
                        m = len(mat)
                        n = len(mat[0])
                    n+=1
                m+=1
            if ans == lans:
                return ans
            lans = ans
        return ans

```
  
### No.5286 [网格中的最短路径](https://leetcode-cn.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

>给你一个 m * n 的网格，其中每个单元格不是 0（空）就是 1（障碍物）。每一步，您都可以在空白单元格中上、下、左、右移动。
>如果您 最多 可以消除 k 个障碍物，请找出从左上角 (0, 0) 到右下角 (m-1, n-1) 的最短路径，并返回通过该路径所>需的步数。如果找不到这样的路径，则返回 -1。
>
>示例 1：
>
>>输入：
>>grid =
>>\[[0,0,0],
>> [1,1,0],
>> [0,0,0],
>> [0,1,1],
>> [0,0,0]],
>>k = 1
>>输出：6
>>解释：
>>不消除任何障碍的最短路径是 10。
>>消除位置 (3,2) 处的障碍后，最短路径是 6 。该路径是 (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> (3,2)> -> (4,2).
>
>
>示例 2：
>
>>输入：
>>grid = 
>>\[[0,1,1],
>> [1,1,1],
>> [1,0,0]],
>>k = 1
>>输出：-1
>>解释：
>>我们至少需要消除两个障碍才能找到这样的路径。
>
>提示：
>
>>- grid.length == m
>>- grid[0].length == n
>>- 1 <= m, n <= 40
>>- 1 <= k <= m*n
>>- grid[i][j] == 0 or 1
>>- grid[0][0] == grid[m-1][n-1] == 0

一个稍微有点变化的BFS，在格子中走的时候同时记录已经越过的障碍K的数量，同时在遍历过程中进行障碍数量的更新，已经超过要求的数量的弃置不用。同时留存一个dict进行已经经历过状态的存储，记录步数要最小即可。

``` python
class Solution:
    def shortestPath(self, grid: List[List[int]], k: int) -> int:
        if len(grid) == 1 and len(grid[0]) == 1:
            return 0
        ans = 99999999999
        stats = [(0,0,0,0)]
        sts = {(0,0,0):0}
        moves = [[0,1],[0,-1],[1,0],[-1,0]]
        i = 0
        while i<len(stats):
            for dx,dy in moves:
                x = stats[i][0] + dx
                y = stats[i][1] + dy
                if x>=0 and x<len(grid) and y>=0 and y<len(grid[0]):
                    step = stats[i][2]+1
                    ks = stats[i][3]+(1 if grid[x][y] == 1 else 0)
                    if ks<=k:
                        if ((x,y,ks) in sts and step<sts[(x,y,ks)]) or (x,y,ks) not in sts:
                            sts[(x,y,ks)] = step
                            if x == len(grid)-1 and y == len(grid[0])-1:
                                ans = min(ans,step)
                                if ans == len(grid)+len(grid[0]) - 2:
                                    return ans
                            stats.append((x,y,step,ks))
            i+=1
        return -1 if ans == 99999999999 else ans
```
