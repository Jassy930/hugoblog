---
title: Project Euler  题解：31~40
comment: true
toc: true
tags:
  - Project Euler
abbrlink: 3f1bfcb
updated: 2019-07-01 10:51:26
---

> 罗马不是一天建成的，你也不会一下成为梦想中的人，但是只要在学习在努力，就是越来越接近的。

继续上一次的Project Euler的题解，不见得最优，也不见得优雅，但是答案是对的，思路也是没问题的。这次是31~40题。

(奇怪，这人怎么又开始用python写了


## [Problem 31: Coin sums](https://projecteuler.net/problem=31)

嗯。。挺经典挺常见的完全背包DP问题，但是我满脑子不知道在想什么，愣是没做出来，后来看了看别人的提示才想到应该按照硬币种类去循环。一直在想怎么去处理重复的情况。

[这里](https://projecteuler.net/overview=031)是这道题的详细解释

[这里](https://blog.csdn.net/na_beginning/article/details/62884939)是完全背包、01背包、多重背包的比较的参考资料（我没看

```python
def pro31():
    # 完全背包DP
    coin = [1, 2, 5, 10, 20, 50, 100, 200]
    methods = [0]*300
    methods[0] = 1
    for k in coin:
        for i in range(1, 210):
            if i >= k:
                methods[i] += methods[i-k]
    print(methods[200])
```

## [Problem 32: Pandigital products](https://projecteuler.net/problem=32)

很蠢的一个做法，每一个去遍历判断的

```python
def pro32():
    out = []
    for i in range(10, 10000):
        numl = []
        k = i
        while k != 0:
            if k % 10 not in numl:
                numl.append(k % 10)
                k = k//10
            else:
                break
        if k == 0:
            for j in range(2, int(math.sqrt(i))):
                if i % j == 0:
                    numll = []
                    k = j
                    while k != 0:
                        if (k % 10 not in numl) and (k % 10 not in numll):
                            numll.append(k % 10)
                            k = k//10
                        else:
                            break
                    if k == 0:
                        k = i//j
                        while k != 0:
                            if (k % 10 not in numl) and (k % 10 not in numll):
                                numll.append(k % 10)
                                k = k//10
                            else:
                                break
                        if (k == 0) and (len(numl)+len(numll) == 9) and (0 not in numl) and (0 not in numll):
                            print("%d  %d  %d" % (i, j, i//j))
                            if i not in out:
                                out += [i]
    print(sum(out))
```

## [Problem 33: Digit cancelling fractions](https://projecteuler.net/problem=3)

这个问题第一次考虑的时候考虑错了，以为分子和分母相同的数字肯定一个在十位一个在个位的，导致一开始答案是错误的。其实可以在外层循环增加一些判断，这样速度会快一些。。。

```python
def pro33():
    for i in range(10, 98):
        for k in range(i+1, 99):
            for j in str(i):
                if j in str(k):
                    if i % 10 == int(j) and (k % 10) != 0:
                        if ((i//10) / (int(str(k).replace(j, '', 1)))) == i/k:
                            print("%d %d" % (i, k))
                    elif k % 10 != 0:
                        if ((i % 10) / (int(str(k).replace(j, '', 1)))) == i/k:
                            print("%d %d" % (i, k))
```

## [Problem 34: Digit factorials](https://projecteuler.net/problem=34)

算就对了

```python
def pro34():
    out = 0
    for i in range(3, 100000):
        k = i
        sum = 0
        while k != 0:
            sum += math.factorial(k % 10)
            k = k//10
        if sum == i:
            print(i)
            out += i
    print(out)
```

## [Problem 35: Circular primes](https://projecteuler.net/problem=35)

遍历，判断质数的程序又写了python版的，在最后贴上来

```python
def pro35():
    out = 4
    for i in range(11, 1000000, 2):
        if isPrime(i):
            if i == 11:
                i = 11
            num = 0
            for k in range(1, len(str(i))):
                j = int(str(i)[k:]+str(i)[:k])
                if not isPrime(j):
                    num += 1
                    break
            if num == 0:
                print(i)
                out += 1
    print(out)
```

## [Problem 36: Double-base palindromes](https://projecteuler.net/problem=36)

遍历，不讲（不过python确实行数少很多

```python
def pro36():
    out = 0
    for i in range(1, 1000000):
        k = str(i)
        if k == k[::-1]:
            k = bin(i)[2:]
            if k == k[::-1]:
                print(i)
                out += i
    print(out)
```

## [Problem 37: Truncatable primes](https://projecteuler.net/problem=37)

遍历。。。

```python
def pro37():
    out = 0
    for i in range(11, 1000000, 2):
        if isPrime(i):
            n = 0
            for k in range(1, len(str(i))):
                if not isPrime(int(str(i)[k:])):
                    n += 1
                    break
                if not isPrime(int(str(i)[:k])):
                    n += 1
                    break
            if n == 0:
                print(i)
                out += i
    print(out)
```

## [Problem 38: Pandigital multiples](https://projecteuler.net/problem=38)

同上。。。（这次题没有好玩的呀

```python
def pro38():
    for i in range(5, 10000):
        nums = []
        leng = 0
        k = i
        while k != 0:
            if k % 10 not in nums:
                nums.append(k % 10)
            k = k//10
        if len(nums) != len(str(i)):
            continue
        leng += len(nums)
        for j in range(2, 10):
            k = j*i
            while k != 0:
                if k % 10 not in nums:
                    nums.append(k % 10)
                k = k//10
            if len(nums)-leng != len(str(j*i)):
                break
            leng = len(nums)
            if (leng == 9) and (0 not in nums):
                print(i)
```

## [Problem 39: Integer right triangles](https://projecteuler.net/problem=39)

遍历。。，这个题判断条件可以仔细思考一下，能节约很多无效的计算

```python
def pro39():
    # solution计算有问题，有重复情况，但是题目不要求~~~
    solution = 0
    out = 0
    for p in range(10, 1000):
        so = 0
        for a in range(p//3, p-2):
            for b in range(1, a-1):
                c = p-a-b
                if c > 0 and a*a == b*b+c*c:
                    so += 1
        if so > solution:
            solution = so
            out = p
    print(out)
```

## [Problem 40: Champernowne's constant](https://projecteuler.net/problem=40)

蠢遍历

```python
def pro40():
    # 蠢办法 is coming
    out = 1
    count = 1
    nums = [1, 10, 100, 1000, 10000, 100000, 1000000]
    for i in range(1, 100000):
        k = str(i)
        while len(k) != 0:
            if count in nums:
                out = out*int(k[0])
                print(k[0])
            count += 1
            k = k[1:]
    print(out)
```

## Prime (python)

```python
def isPrime(n):
    if not hasattr(isPrime, 'prime'):
        isPrime.prime = [2, 3, 5, 7]
    if n < isPrime.prime[-1]:
        return n in isPrime.prime
    else:
        i = isPrime.prime[-1]+2
        while n > isPrime.prime[-1]:
            num = 0
            for k in isPrime.prime:
                if i % k == 0:
                    num += 1
                    break
                if k > math.sqrt(i):
                    break
            if num == 0:
                isPrime.prime.append(i)
            i += 2
        return n in isPrime.prime
```

```python
def isPrimeS(n):
    if n < 2:
        return False
    if n % 2 == 0:
        return False
    if n % 3 == 0:
        return False
    for k in range(2, int(math.sqrt(n))):
        if n % k == 0:
            return False
    return True
```
















