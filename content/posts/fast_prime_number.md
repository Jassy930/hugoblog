---
title: 验证质数
comment: true
toc: true
tags:
  - Project Euler
  - Algorithm
abbrlink: 6057a4d0
date: 2019-06-05 16:58:35
updated: 2019-06-10 16:36:01
---

* [问题](#问题 )
* [暴力求解](#暴力求解 )
* [解答分析](#解答分析 )
* [对于循环次数](#对于循环次数 )
* [对于判断范围](#对于判断范围 )

> 若不是因为爱着你，怎么会夜深还没睡意。每个念头都关于你，我想你 想你 好想你。
> 若不是因为爱着你，怎会不经意就叹息。有种不完整的心情，爱你 爱你 爱着你。

## 问题

最近做了一些 Project Euler 的题目。目前只做了前十题，感觉和一般的编程题目比还是挺有意思的，单一案例，不限时间，不限做法，只要做出来的就可以，你愿意的话暴力求解，手动去算都可以，还是挺有意思的。

前面有几道关于质数 (Prime Number) 的题目，需要判断一个数是否是质数，也就是这次的问题了。

[**Problem 7.** 10001st prime](https://projecteuler.net/problem=7)

> By listing the first six prime numbers: 2, 3, 5, 7, 11, and 13, we can see that the 6th prime is 13.
>What is the 10 001st prime number?

[**Problem 10.** Summation of primes](https://projecteuler.net/problem=10)

>The sum of the primes below 10 is 2 + 3 + 5 + 7 = 17.
>Find the sum of all the primes below two million.

## 暴力求解

我在做题目的时候是将所有质数从小到大依次列出来，这里暂且不区分列出全部指数和判断一个数是否是质数，后面会提到区别。

直接代码，对应题目

``` c++
//求第10001个质数
vector<int> prime;
for (int i = 2; ; i++)
{
    int n = 0;
    for (int k = 2; k < i; k++)
    {
        if (i % k == 0) {
            n++; break;
        }
    }
    if (n == 0) { prime.push_back(i); cout << i << endl; }
    if (prime.size() == 10001) return prime.back();
}
```

从小到大判断每一个数字，对每一个数字 i 判断 2 到 i 中的每个数字是否可以被 i 整除，可以整除的话计数 +1，说明该数是合数，全部数字判断完 n=0 的话是质数。

简单吧，暴力吧，蠢吧。

## 解答分析

Project Euler 上提供的一份判断质数的程序如下

``` c
Function isPrime(n)
if n=1 then return false
else
if n<4 then return true //2 and 3 are prime
else
if n mod 2=0 then return false
else
if n<9 then return true //we have already excluded 4,6 and 8.
else
if n mod 3=0 then return false
else
r=floor( sqrt(n) ) // sqrt(n) rounded to the greatest integer r so that r*r<=n
f=5
 while f<=r
if n mod f=0 then return false (and step out of the function)
if n mod(f+2)=0 then return false (and step out of the function)
f=f+6
endwhile
return true (in all other cases)
End Function
```

前面一些都好理解，整个程序基于以下假设：

- 1 is not a prime.
- All primes except 2 are odd.
- All primes greater than 3 can be written in the form 6k+/-1.
- Any number n can have only one primefactor greater than sqrt(n) .
- The consequence for primality testing of a number n is: if we cannot find a number f less than or equal sqrt(n) that divides n then n is prime: the only primefactor of n is n itself

其中最重要的是第三条，**所有大于3的质数都可以写成 ```6k+/-1``` 的形式**。

证明如下：

大于6的自然数都可以写成 ```6k``` ```6k+1``` ```6k+2``` ```6k+3``` ```6k+4``` ```6k+5``` 六种形式（其中k>=1）。依次判断可以知道

-  ```6k``` 可被6整除
- ```6k+2=2(3k+1)``` 可被2整除
- ```6k+3=3(2k+1)``` 可被3整除
- ```6k+4=2(3k+2)``` 可被2整除

因此上面四项均为合数，质数必定存在于 ```6k+1``` 和 ```6k+5``` 两项中，其中 ```6k+5``` 又可以改写为 ```6k-1``` ，因此得证。


然后还有第四条，对于n的因数来讲，其因数必定小于sqrt(n)，所以不需要在大于sqrt(n)的范围内寻找n的因数。

## 对于循环次数

因此对判断质数的第一个改进是修改循环次数小于sqrt(n)，看代码：

``` c++
//求第10001个质数
vector<int> prime;
for (int i = 2; ; i++)
{
    int n = 0;
    int ss = sqrt(i);
    for (int k = 2; k <= ss; k++)
    {
        if (i % k == 0) {
            n++; break;
        }
    }
    if (n == 0) { prime.push_back(i); cout << i << endl; }
    if (prime.size() == 10001) return prime.back();
}
```

对每一个数i，只判断到sqrt(i)，如果除以2到sqrt(i)余数均不为0，则i为质数。

## 对于判断范围

另一个思考是关于 ```6k+/-1``` 的问题，但是这里感觉并没有必要按照 ```6k+/-1``` 的规则去写判断的代码，因为很明显这个结论的得来是通过对6以下的质数一个一个进行判断得到的，类似的规律如果我对30、甚至210去判断的话都能够得到计算量更少的结果。所以稍加思考其实可以想到我们对前面的很多判断是重复的，例如如果判断了2不是i的因数的话，4就不需要进行判断了，2的所有倍数都不需要进行判断了，这些判断都是无用的。以此类推的话是3，5。奇妙的是这个需要判断的数列很明显就是质数，所以我们在判断是否能够整除的时候，只判断小于sqrt(i)的质数就可以了，代码如下：

``` c++
//求第10001个质数
vector<int> prime;
for (int i = 2; ; i++)
{
    int n = 0;
    int ss = sqrt(i);
    for (int k = 0; k < prime.size(); k++)
    {
        if (i % prime.at(k) == 0) {
            n++; break;
        }
        if (prime.at(k) > ss) break;
    }
    if (n == 0) { prime.push_back(i); cout << i << endl; }
    if (prime.size() == 10001) return prime.back();
}
```

DONE! 在目前的意义上这个判断质数的程序已经比较优雅了，但是也不禁让人有新的思考。目前这个程序判断某个数需要把比这个数小的质数全部求出来，是否有更快捷的办法。是否可以通过提前计算一个质数表去加速程序的计算？我目前也对更好的办法一无所知，期待后面进行进一步的研究学习再回来更新。

