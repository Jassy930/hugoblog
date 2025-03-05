---
title: Project Euler  题解：11~20
comment: true
toc: true
tags:
  - Project Euler
abbrlink: 2f6b4b2d
updated: 2019-06-17 10:32:39
---

[TOC]

继续上一次的Project Euler的题解，不见得最优，也不见得优雅，但是答案是对的，思路也是没问题的。这次是11~20题。

## [Problem 11:Largest product in a grid](https://projecteuler.net/problem=11)

在一个20x20的表格内在横竖斜四个方向上找连续四个数字的最大乘积。

暴力遍历就对了，输入直接复制过来通过文本输入，给一个二维的 ```vector<vector<int>>``` ，开始可劲的遍历，计算四个方向。为了避免边界条件判断，这里分成了两部分，一部分向右，下，右下遍历，一部分向左上遍历，同样能遍历所有情况，同时避免了边界条件判断。

``` c++
void problem11()
{
	int out = 0;
	string input ="08 02 22 97 38 15 00 40 00 75 04 05 07 78 52 12 50 77 91 08\
		49 49 99 40 17 81 18 57 60 87 17 40 98 43 69 48 04 56 62 00\
		81 49 31 73 55 79 14 29 93 71 40 67 53 88 30 03 49 13 36 65\
		52 70 95 23 04 60 11 42 69 24 68 56 01 32 56 71 37 02 36 91\
		22 31 16 71 51 67 63 89 41 92 36 54 22 40 40 28 66 33 13 80\
		24 47 32 60 99 03 45 02 44 75 33 53 78 36 84 20 35 17 12 50\
		32 98 81 28 64 23 67 10 26 38 40 67 59 54 70 66 18 38 64 70\
		67 26 20 68 02 62 12 20 95 63 94 39 63 08 40 91 66 49 94 21\
		24 55 58 05 66 73 99 26 97 17 78 78 96 83 14 88 34 89 63 72\
		21 36 23 09 75 00 76 44 20 45 35 14 00 61 33 97 34 31 33 95\
		78 17 53 28 22 75 31 67 15 94 03 80 04 62 16 14 09 53 56 92\
		16 39 05 42 96 35 31 47 55 58 88 24 00 17 54 24 36 29 85 57\
		86 56 00 48 35 71 89 07 05 44 44 37 44 60 21 58 51 54 17 58\
		19 80 81 68 05 94 47 69 28 73 92 13 86 52 17 77 04 89 55 40\
		04 52 08 83 97 35 99 16 07 97 57 32 16 26 26 79 33 27 98 66\
		88 36 68 87 57 62 20 72 03 46 33 67 46 55 12 32 63 93 53 69\
		04 42 16 73 38 25 39 11 24 94 72 18 08 46 29 32 40 62 76 36\
		20 69 36 41 72 30 23 88 34 62 99 69 82 67 59 85 74 04 36 16\
		20 73 35 29 78 31 90 01 74 31 49 71 48 86 81 16 23 57 05 54\
		01 70 54 71 83 51 54 69 16 92 33 48 61 43 52 01 89 19 67 48";
	vector<int> nums;
	stringstream ss;
	ss << input;
	int n;
	while (ss >> n) { nums.push_back(n); }
	for (int i = 0; i < 15; i++)
	{
		for (int k = 0; k < 15; k++)
		{
			int nn = nums.at(i + k * 20) * nums.at(i + k * 20 + 21) * nums.at(i + k * 20 + 2 * 21) * nums.at(i + k * 20 + 3 * 21);
			out = MAX(out, nn);
			nn = nums.at(i + k * 20) * nums.at(i + k * 20 + 20) * nums.at(i + k * 20 + 2 * 20) * nums.at(i + k * 20 + 3 * 20);
			out = MAX(out, nn);
			nn = nums.at(i + k * 20) * nums.at(i + k * 20 + 1) * nums.at(i + k * 20 + 2 * 1) * nums.at(i + k * 20 + 3 * 1);
			out = MAX(out, nn);
		}
	}
	for (int i = 0; i < 15; i++)
	{
		for (int k = 3; k < 19; k++)
		{
			int nn = nums.at(i + k * 20) * nums.at(i + k * 20 - 19) * nums.at(i + k * 20 - 2 * 19) * nums.at(i + k * 20 - 3 * 19);
			out = MAX(out, nn);
		}
	}
	cout << out << endl;;
}

```

## [Problem 12:Highly divisible triangular number](https://projecteuler.net/problem=12)

题目写的挺绕的，按步骤做就好了，先是一个```1 ~ n```的等差数列的和，我这里用的方法麻烦了，在每个周期上加```i```就可以了。然后计算```1 ~ sqrt(k)```之间的能被整除的数字数量，乘以二就是因数数量，这边没有单独判断```k```是平方数的情况（因为这种情况根本不存在，```k=i*(i+1)/2```），最后判断一下数量大于500输出结果即可。

``` c++
void problem12()
{
	for (int i = 1; ; i++)
	{
		int k = i * (i + 1) / 2;
		int count = 0;
		cout << k << endl;
		for (int j = 1; j <= sqrt(k); j++)
		{
			if (k % j == 0)
			{
				count += 2;
			}
		}
		if (count > 500) {
			cout << "ans:" << k << endl;
			return;
		}
	}
}
```

## [Problem 13:Large sum](https://projecteuler.net/problem=13)

把下列100个五十位的数字相加，取和的前十位。

本来考虑是个大整数相加，但是结果只要前十位啊！前十位！所以一个```long long```是足够用的，我们也只加前面的数字就可以，不需要加整个的50位数字。100个数字的话因为进位，需要加前12位，最后结果取前十位数字即可。为什么我取了11位，因为我随便写的，发现答案是对的就过去了:)

``` c++
void problem13()
{
	string num = "37107287533902102798797998220837590246510135740250\
	...
	53503534226472524250874054075591789781264330331690";
	vector<string> nums;
	nums = split(num, '\t');
	long long sum=0;
	for (int i = 0; i < nums.size(); i++)
	{
		nums[i] = nums.at(i).substr(0, 11);
		stringstream ss;
		ss << nums[i];
		long long nn;
		ss >> nn;
		cout << nn << endl;
		sum += nn;
	}

	for (auto i : nums)cout << i << endl;
	cout << sum;
}
```

## [Problem 14:Longest Collatz sequence](https://projecteuler.net/problem=14)

小时候玩的数学游戏里面的题目，长大了发现是个很有名的问题，甚至还没有被解决。被称作克拉兹问题（Collatz problem）也被叫做hailstone问题、3n+1问题、Hasse算法问题、Kakutani算法问题、Thwaites猜想或者Ulam问题。

这道题是问一百万以下的数字中，哪个数字的链最长，遍历就好了，记录步长。唯一需要注意的是，虽然开始计算的数字小于一百万，但是中间数字会超过int的最大值，所以这里使用```long long```计算

``` c++
void problem14() {
	int out = 1;
	int steps = 0;
	for (int k = 1; k < 1000000; k++)
	{
		long long i = k;
		int step = 0;
		while (i != 1)
		{
			step++;
			if (i % 2 == 0) i = i / 2;
			else i = i * 3 + 1;
		}
		if (step > steps) {
			steps = step;
			out = k;
		cout << k << "  " << step << endl;
		}
	}
	cout << "ans:" << out << endl;

}
```

## [Problem 15:Lattice paths](https://projecteuler.net/problem=15)

这个题是做过很多遍的了，简单的DP问题。

另外，也可以用排列组合的方法，四十个步骤中插入二十个右，$C_{40}^{20}$。

``` c++
void problem15() {
	// 2  6
	// 3  20
	// DP
#define edge 20
	long long nums[edge+1][edge+1];
	for (int i = 0; i < edge + 1; i++)
	{
		nums[0][i] = 1;
		nums[i][0] = 1;
	}
	for (int i = 1; i < edge + 1; i++)
	{
		for (int k = 1; k < edge + 1; k++)
		{
			nums[i][k] = nums[i - 1][k] + nums[i][k - 1];
		}
	}
	cout << "ans:" << nums[edge][edge] << endl;
}
```

## [Problem 16:Power digit sum](https://projecteuler.net/problem=16)

嗯，这个题用的大整数乘法，可以见之前的文章实现，最后累加每一位数就可以了。这道题用的乘2。

``` c++
void problem16()
{
	string num = "1";
	
	for(int ii=0; ii<1000; ii++)
	{
		string num2 = "";
		int up = 0;
		for (auto i = num.rbegin(); i != num.rend(); i++)
		{
			int n = (*i) - 48;
			n = n * 2 + up;
			if (n >= 10) up = 1;
			else up = 0;
			stringstream ss;
			ss << n%10;
			num2 = ss.str() + num2;
		}
		if (up == 1) num2 = "1" + num2;
		num = num2;
		cout << num2 << endl;
	}

	int sum = 0;
	for (int i = 0; i < num.size(); i++)
	{
		sum += num.at(i) - 48;
	}
	cout << "ans:" << sum;
}
```

## [Problem 17:Number letter counts](https://projecteuler.net/problem=17)

英语无能，挨个数。

## [Problem 18:Maximum path sum I](https://projecteuler.net/problem=18)

寻找一条从顶端到底端的路径，使得路径上的数字总和最大。DP

从底端向上算，记录每一个值到下一行的可能的最大值，算到最后剩下唯一一个值，即为题目要求的最大值。

``` c++
void problem18()
{
	string triangle = "75\
	95 64\
	17 47 82\
	18 35 87 10\
	20 04 82 47 65\
	19 01 23 75 03 34\
	88 02 77 73 07 63 67\
	99 65 04 28 06 16 70 92\
	41 41 26 56 83 40 80 70 33\
	41 48 72 33 47 32 37 16 94 29\
	53 71 44 65 25 43 91 52 97 51 14\
	70 11 33 28 77 73 17 78 39 68 17 57\
	91 71 52 38 17 14 91 43 58 50 27 29 48\
	63 66 04 68 89 53 67 30 73 16 69 87 40 31\
	04 62 98 27 23 09 70 98 73 93 38 53 60 04 23";
	vector<vector<int>> trianglenum;
	vector<string>lines = split(triangle, '\t');
	for (int i = 0; i < lines.size(); i++)
	{
		vector<int> numlines;
		vector<string> nums = split(lines.at(i), ' ');
		for (int k = 0; k < nums.size(); k++)
		{
			stringstream ss;
			ss << nums.at(k);
			int nn;
			ss >> nn;
			numlines.push_back(nn);
		}
		trianglenum.push_back(numlines);
	}
	cout << trianglenum.size() << endl;

	for (int i = trianglenum.size() - 2; i >= 0; i--)
	{
		for (int k = 0; k < trianglenum.at(i).size(); k++)
		{
			int nn = MAX(trianglenum.at(i + 1).at(k), trianglenum.at(i + 1).at(k + 1));
			trianglenum[i][k] = trianglenum.at(i).at(k) + nn;
		}
	}
	cout << "ans:" << trianglenum.at(0).at(0);
}
```

另外题目说明里说这道题的数量级可以使用遍历来实现，然后联动了[Problem 67:Counting Sundays](https://projecteuler.net/problem=67)，是同样的问题，但是有100行，做法与这道题一样，代码也贴到这里吧。

```c++
void problem67()
{
	string trianglefilename = "p067_triangle.txt";
	ifstream fin;
	fin.open(trianglefilename, ios::in);
	vector<string> lines;
	string s;
	while (getline(fin,s))
	{
		lines.push_back(s);
	}
	fin.close();

	vector<vector<int>> trianglenum;
	for (int i = 0; i < lines.size(); i++)
	{
		vector<int> numlines;
		vector<string> nums = split(lines.at(i), ' ');
		for (auto k = 0; k < nums.size(); k++)
		{
			stringstream ss;
			ss << nums.at(k);
			int nn;
			ss >> nn;
			numlines.push_back(nn);
		}
		trianglenum.push_back(numlines);
	}
	cout << trianglenum.size() << endl;

	for (int i = trianglenum.size() - 2; i >= 0; i--)
	{
		for (int k = 0; k < trianglenum.at(i).size(); k++)
		{
			int nn = MAX(trianglenum.at(i + 1).at(k), trianglenum.at(i + 1).at(k + 1));
			trianglenum[i][k] = trianglenum.at(i).at(k) + nn;
		}
	}
	cout << "ans:" << trianglenum.at(0).at(0);
}

```

## [Problem 19:Counting Sundays](https://projecteuler.net/problem=19)

数

## [Problem 20:Factorial digit sum](https://projecteuler.net/problem=20)

计算100的阶乘的数字和。和之前的题目一样，用大整数乘法，见之前的文章。

``` c++
void problem20()
{
	string out = "1";
	for (int i = 1; i < 101; i++)
	{
		stringstream ss;
		ss << i;
		out = BIGtimes(out,ss.str());
		cout << out << endl;
	}
	int outnum = 0;
	for (auto i : out)
		outnum += i - 48;
	cout << outnum << endl;
}
```
