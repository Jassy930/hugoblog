---
title: Project Euler  题解：21~30
comment: true
toc: true
tags:
  - Project Euler
abbrlink: 71d000bc
updated: 2019-06-30 10:32:39
---

[TOC]

继续上一次的Project Euler的题解，不见得最优，也不见得优雅，但是答案是对的，思路也是没问题的。这次是21~30题。

## [Problem 21:Amicable numbers](https://projecteuler.net/problem=21)

对于x，除自己以外的因数的和定义为d(x)，求出1000以下的所有满足```d(a)=d(b) && a!=b```的数对并求和。

遍历->求所有因数->求和记录->遍历判断并求和

``` c++
void problem21()
{
    map<int, int> mm;
    int out = 0;
    for (int i = 1; i < 10000; i++)
    {
        int sum = 0;
        for (int k = 1; k <= (i / 2); k++)
        {
            if (i % k == 0) sum += k;
        }
        mm.insert({ i, sum });
        //cout << i << "  " << sum << endl;
    }
    for (auto i : mm)
    {
        if (mm[i.second] == i.first && i.first != i.second) {
            cout << i.first << endl;
            out += i.first;
        }
    }
    cout << out << endl;
}
```

## [Problem 22:Names scores](https://projecteuler.net/problem=22)

有一群名字，要求排序，排序之后每个名字的数字和乘以位数再求和。
emmmmm，这个题似乎做得有一点点抄近路，直接STL排序，然后遍历求和即可。

``` c++
void problem22()
{
    string trianglefilename = "p022_names.txt";
    ifstream fin;
    fin.open(trianglefilename, ios::in);
    vector<string> lines;
    string s;
    while (getline(fin, s))
    {
        lines.push_back(s);
    }
    fin.close();

    long out = 0;
    lines = split(lines.at(0), ',');
    for (int i = 0; i < lines.size(); i++)
    {
        lines.at(i) = lines.at(i).substr(1, lines.at(i).size() - 2);
    }
    sort(lines.begin(), lines.end());
    for (int i = 0; i < lines.size(); i++)
    {
        int sum = 0;
        for (int i : lines.at(i))
        {
            sum += i - 64;
        }
        out += sum * (i + 1);
    }
    cout << out << endl;
}
```

## [Problem 23:Non-abundant sums](https://projecteuler.net/problem=23)

完美数，除了本身以外的因数的和等于自身。
deficient，因数和小于自身；abundant，因数和大于自身。
这题太难解释了，直接题目抄过来自己看题目吧。

>A perfect number is a number for which the sum of its proper divisors is exactly equal to the number. For example, the sum of the proper divisors of 28 would be 1 + 2 + 4 + 7 + 14 = 28, which means that 28 is a perfect number.
>
>A number n is called deficient if the sum of its proper divisors is less than n and it is called abundant if this sum exceeds n.
>
>As 12 is the smallest abundant number, 1 + 2 + 3 + 4 + 6 = 16, the smallest number that can be written as the sum of two abundant numbers is 24. By mathematical analysis, it can be shown that all integers greater than 28123 can be written as the sum of two abundant numbers. However, this upper limit cannot be reduced any further by analysis even though it is known that the greatest number that cannot be expressed as the sum of two abundant numbers is less than this limit.
>
>Find the sum of all the positive integers which cannot be written as the sum of two abundant numbers.

也没啥说的，遍历呗，在考虑要不要这种题就不说了，直接贴代码好了。。。。都是暴力遍历，并没有什么可说的

``` c++
void problem23()
{
    vector<int> vv;
    map<int, int> mm;
    int out = 0;
    for (int i = 1; i < 28123; i++)
    {
        int sum = 0;
        for (int k = 1; k < i; k++)
        {
            if (i % k == 0) sum += k;
        }
        if (sum > i) { vv.push_back(i); mm[i] = 1; }
    }
    for (int i = 1; i < 28124; i++)
    {
        if (i % 100 == 0) cout << i << endl;
        int flag = 0;
        for (int k = 0; (k < vv.size()) &&(i>vv.at(k)) ; k++)
        {
            if (mm[i-vv.at(k)] == 1) { flag += 1; break; }
        }
        if (flag == 0)out += i;
    }
    cout << out << endl;
}
```

## [Problem 24:Lexicographic permutations](https://projecteuler.net/problem=24)

>A permutation is an ordered arrangement of objects. For example, 3124 is one possible permutation of the digits 1, 2, 3 and 4. If all of the permutations are listed numerically or alphabetically, we call it lexicographic order. The lexicographic permutations of 0, 1 and 2 are:
>
>012   021   102   120   201   210
>
>What is the millionth lexicographic permutation of the digits 0, 1, 2, 3, 4, 5, 6, 7, 8 and 9?

收回上一题说的话！这道题就很有意思了，看上去是一道排列组合的题啊，但是下面代码跑出来的结果并不是直接的最终答案。这道题可以用STL的```next_permutation```函数做，但是，无趣。

所以稍加分析可以知道，最小的数字是0123456789，然后如果前八位不的话，有2种可能性。前七位不动的话，有$2\times3$种可能性。也就是说，n位数字变动，有```n!```的可能性。那么我们可以看做这个排列是每一位是```n!```进位制的。这样将1000000尽可能高的整除下来，计算出来的数字就是某一位该**进几位**。

举个简单的例子，比如求0,1,2,3的第14小的数字。进位分别为6,2,1,14减1后整除，$13=6\times2+2\times0+1\times1$分别进位2,0,1位；然后在每一位上选取未选过的第进位数字小的数，这样第一个数字为0进位2位，是2，第二个数字是0进位0位是0，第三个数字是1进位1位是3，第四个数字是剩下一个数字1，四个数字为2,0,3,1，所以最后答案为2031。

``` c++
void problem24()
{
    vector<int> nn;
    nn.push_back(1);
    for (int i = 0; i < 9; i++)nn.push_back(nn.back() * (nn.size() + 1));
    for (auto i : nn)cout << i << endl;
    int num = 1000000-1;
    string out = "";
    for (auto i = nn.rbegin(); i != nn.rend(); i++)
    {
        cout << (*i) << "  " << (char)(num / (*i) + 48) << endl;
        out = out + (char)(num / (*i) + 48);
        num = num % (*i);
    }
    cout << out << endl;
}
```

## [Problem 25:1000-digit Fibonacci number](https://projecteuler.net/problem=25)

求第几个斐波那契数列的值，拥有1000位。

依旧用之前写的大整数加法

``` c++
void problem25()
{
    vector<string> fib;
    fib.push_back("1");
    fib.push_back("1");
    while (fib.back().size() < 1000)
    {
        fib.push_back(BIGadds(fib.back(), fib.at(fib.size() - 2)));
    }
    cout << fib.size() << endl;
}
```

## [Problem 26:Reciprocal cycles](https://projecteuler.net/problem=26)

好像他们管这个东西叫循环节，那就是求1000以下的数字d，使得1/d拥有最长循环节。
用浮点数是不可能的了，用脚指头想也是不可能的。只能用模拟手动除法的方式去做。当除不尽的时候除数乘以十并加一位计数。
所以关键是怎么判断出现了循环呢，判断数字一样并不能表示出现了循环，关键是判断除数出现了循环，（这边看到有地方说是出现了1，但是并不一定是这样的，比如1/6就不是），做除法的同时将除数放到一个map内，并将当前除数的位数存到map内，当存储时map的对应key内已经有值的时候说明出现了循环，两个位数相减即是循环节的长度。

``` c++
void problem26()
{
    // 4.29s
    int out = 0;
    int true_out = 0;
    for (int i = 1; i < 1000; i++)
    {
        cout << i << "\t";
        int num = 1;
        map<int, int> mm;
        for (int j = 1;; j++)
        {
            num = num * 10;
            num = num % i;
            if (num == 0) { cout << "0" << endl; break; }
            if (mm[num] == 0) mm[num] = j;
            else {
                cout << j - mm[num] << endl;
                if((j-mm[num]) > out)true_out = i;
                out = MAX(out, j - mm[num]);
                break;
            }
        }
    }
    cout << out << endl;
    cout << true_out << endl;
}
```

## [Problem 27:Quadratic primes](https://projecteuler.net/problem=27)

暴力遍历，另外这里更新了一下判断质数的程序，能快一点，也贴在下面吧。
但是这个公式蛮有意思的，想想老一辈的数学家们可能一个奇思妙想或者费劲心思的计算验证，能都得到一个很神奇的结果，但是到我们这代的时候甚至可以把这种结果通用化的去验证和计算，并且毫不费力，科技的发展真的是巨大的动力，将旧时代的成果毫不费力的碾压过去。

``` c++
void problem27()
{
    int realout = 0;
    int out = 0;
    for (int a = -999; a < 1000; a++)
    {
        cout << a << endl;
        for (int b = -1000; b <= 1000; b++)
        {
            for (int i = 0; ; i++)
            {
                if (!isPrime(i*i+a*i+b))
                {
                    if (i > out) {
                        out = MAX(out, i);
                        realout = a * b;
                    }
                    break;
                }
            }
        }
    }
    cout << out << endl;
    cout << realout << endl;
}
```

```c++
bool isPrime(int number)
{
    static set<int> prime;
    prime.insert(2);
    if ((*prime.rbegin()) > number) return (prime.count(number) == 1);
    else for (int i = (*prime.rbegin()) + 1; ; i++)
    {
        int n = 0;
        int ss = sqrt(i);

        for (auto k = prime.begin(); k != prime.end(); k++)
        {
            if (i % (*k) == 0) {
                n++; break;
            }
            if ((*k) > ss)break;
        }
        if (n == 0) {
            prime.insert(i);
            //cout << "new Prime:" << i << endl;
            if (i > number)  return isPrime(number);
        }
    }
}

```

## [Problem 28:Number spiral diagonals](https://projecteuler.net/problem=28)

手动计算的，多写几圈找一下规律，每一圈几个数差多少，圈与圈之间差多少，就可以了。

``` c++
void problem28()
{
    long long out = 1;
    int i = 3;
    int j = 2;
    int k = 10;
    for(int m=0;m<500;m++)
    {
        out += i*4+j*6;
        i += k;
        k += 8;
        j += 2;
    }
    cout << out;
}
```

## [Problem 29:Distinct powers](https://projecteuler.net/problem=29)

这道题做的真的是，太抄近路，直接用大整数乘法+set解决了。仔细想想如果判断的话，挨个数从大到小判断因数似乎也是可行的，但是情况太多，可能漏掉，赞美科技，赞美数据结构。

``` c++
void problem29()
{
    set<string> ss;
    for (int i = 2; i < 101; i++)
    {
        cout << i << endl;
        for (int k = 2; k < 101; k++)
        {
            string num = "1";
            stringstream s;
            s << i;
            for (int j = 0; j < k; j++)
            {
                num = BIGtimes(num, s.str());
            }
            ss.insert(num);
        }
    }

    cout << ss.size();
    //Time:541.863000
}
```

## [Problem 30:Digit fifth powers](https://projecteuler.net/problem=30)

嗯，水仙花数还是啥来着，算就对了。

``` c++
void problem30()
{
    int out = 0;
    for (int i = 2; i<400000; i++)
    {
        //if (i % 10000 == 0) cout << i << endl;
        int m = i;
        int n = 0;
        while (m != 0)
        {
            n += (m % 10) * (m % 10) * (m % 10) * (m % 10) * (m % 10);
            m = m / 10;
        }
        if (i == n) { cout << i << endl; out += i; }
    }
    cout << out << endl;
}
```
