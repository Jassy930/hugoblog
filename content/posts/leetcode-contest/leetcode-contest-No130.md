---
title: LeetCode Contest No.130
comment: true
toc: true
tags:
  - LeetCode
  - Algorithm
mathjax: true
abbrlink: 3e6a825d
updated: 2019-04-02 15:31:23
---
  
  
  
## Contest 130
  
  
通过：4/4  排名：59/1293  1:58:42  6WA
### No.1018 [可被 5 整除的二进制前缀](https://leetcode-cn.com/problems/binary-prefix-divisible-by-5/ )
  
  
思考过多，还在想那样做对不对，做的太少所以对题目类型不熟悉。
  
### No.1017 [负二进制转换](https://leetcode-cn.com/problems/convert-to-base-2/ )
  
  
2WA
  
emmmmmm，这道题怎么就过了呢。。。
  
思考过程：用几个数字尝试拼凑了一下，感觉上是要拼凑到最近的2的幂次上去，但是并没有找到绝对的规律。
  
然后发现某一位数字可以通过其他位来拼凑，如 $(2)^1=(-2)^2+(-2)^1$ ，然后将原始数字以4进制做拆分，再由低位至高位依次处理大于1的情况。例如：$9=1&#x5C;times(-2)^0+2&#x5C;times(-2)^2$，负二进制表示为``` 201 ```，然后从低位到高位，分别处理2、3、4的情况，``` 200 ``` 和 ``` 11000 ``` 相等，``` 300 ``` 和 ``` 11100 ```相等， ``` 400 ``` 和 ``` 10000 ``` 相等。最后即可得到结果。（好麻烦啊）
  
  
看了一下别人的回答，类似于二进制转换，得到 %2 后再进行 /2 的操作。区别只是在奇数位时+1而不是-1。不知道如何证明这样是正确的，但是感觉上是正确的，嗯。不懂得东西先记住就好了。
  
另外还有一点思考，如何证明负二进制是可以表示所有数字的呢，类似的，负三进制是否可以，负四进制是否可以，待求证。
  
### No.1019 [链表中的下一个更大节点](https://leetcode-cn.com/problems/next-greater-node-in-linked-list/ )
  
  
1WA
  
以前做过一道类似的题，维护一个栈，当下一个数字比栈顶的数字大时，取出栈顶的数字并记录，否则将下一个数字入栈，全部循环后若栈内还有数字，均输出0。
  
这道题对比了一下我的做法和别人的，思路是一样的，但是代码量差距很大，自己在这种问题的边界条件和条件分类处理上有点死板和脑袋不清楚，所以代码量大，代码思路也不是十分清晰，还需要多多练习。
  
### No.1020 [飞地的数量](https://leetcode-cn.com/problems/number-of-enclaves/ )
  
  
3WA
  
前面大量时间浪费在了一个错误的思路上，第一反应是和做过的类似，进行一个DFS的遍历，遍历每一个“陆地”，然后向四个方向去搜索，看是否能够找到依然是陆地并接触到边界，如果能，那么不是飞地，如果不能，是飞地，然后标记，进行计数。后来发现越做思路越乱，且复杂，在进行深度搜索的时候还需要维护一个表去标记该地是否是已经搜索过的，极度复杂。然后转回去做第二题，做第二题的途中想到，应该反向去搜索，从接触边界的陆地进行深度搜索，每个搜索过的陆地都进行标记，为不是飞地的陆地，全部搜索遍历结束后，再进行遍历计数。
  
两个可优化的点：一个是根据看别人的代码发现，我是使用迭代进行深度搜索的，有人是使用一个 queue，需要进行搜索的陆地都推入 queue 中，在搜索过程中新搜索到的相邻的不是飞地的地也推入 queue 中，直至整个 queue 清空为止，感觉这种方法思路更加清晰一些，下次做到类似的题需要尝试一下。
  
另外一个是搜索上下左右四个方向的时候，建立一个数组 ``` [[0,1],[0,-1],[1,0],[-1,0]] ```，然后对每个点循环加这四个变量即可，这种方法在前面做题过程中有见到提过，但是自己仍然没有用过，后面遇到一样的题也是需要尝试一下使用这种方法实现。
  