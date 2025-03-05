---
title: LeetCode Contest No.131
comment: true
toc: true
tags:
  - LeetCode
  - Algorithm
abbrlink: 496db2cb
updated: 2019-04-08 14:27:39
---

  
## Contest 131

通过：4/4  排名：44/917  1:10:59  2WA

这次竞赛整体感觉难度一般，基本看到题目后直接就有思路，看了看其他人的做法，基本上思路一样。但是依旧~~~错的多、逻辑复杂、做题的时候感觉考虑清楚了但是做的时候细节还是有没考虑到的地方。速度嘛，不要求了，目前的目的还是能做出来吧。
### No.5016 [删除最外层的括号](https://leetcode-cn.com/contest/weekly-contest-131/problems/remove-outermost-parentheses/)

1WA

一开始使用迭代器进行 string 的遍历，然后在测试的时候，有时候测试结果是正确的，有时候是 out of range，最后也没有发现是什么原因，就先去做剩下三个题了，回来之后改成循环数字下标就好了，可能是最后一个位置超出了 string 的长度，以后还是多用下标比较合适，除非循环内要做的工作比较简单。

  <!-- more -->
### No.5017 [从根到叶的二进制数之和](https://leetcode-cn.com/contest/weekly-contest-131/problems/sum-of-root-to-leaf-binary-numbers/)

一开始初始值给错了，没有大问题。

### No.5018 [驼峰式匹配](https://leetcode-cn.com/contest/weekly-contest-131/problems/camelcase-matching/)

居然，做到一半的时候，忘记题目了！！！

做到一半的时候只记得要匹配对了就可以了，完全忘记了是只能插入小写字母，但是还好还好这道题给的测试用例比较全面，才通过了。

另外可以多接触一下库啊，isupper这种函数的用法，多知道一些总是好事。

### No.5019 [视频拼接](https://leetcode-cn.com/contest/weekly-contest-131/problems/video-stitching/)

1WA

完完全全忘记了那次错误提交是为什么，明明是昨天刚做过的题......

嗯，那就没啥说的了
