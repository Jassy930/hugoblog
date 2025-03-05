---
title: 大整数加法和大整数乘法
comment: true
toc: true
abbrlink: b2990560
date: 2019-06-11 15:29:58
updated: 2019-06-11 15:29:58
tags:
---

> 有时候会在问自己，是不够爱，还是缺少爱的能力。

做 Project Euler 的过程中很多大整数运算，所以先撸了一个大整数加法和大整数,使用string作为输入输出参数

大整数加法代码：

``` c++
// 大整数加法
string BIGadd(vector<string> nums)
{
	if (nums.size() == 0) return "0";
	if (nums.size() == 1)return nums.at(0);
	string out = "";
	int sum = 0;
	int maxdig = 0;
	for (auto i : nums) maxdig = MAX(maxdig, i.size());
	for (int k = 0; ; k++)
	{
		for (auto i = 0; i < nums.size(); i++)
		{
			if (nums.at(i).size() > k)
				sum += nums.at(i).at(nums.at(i).size() - k - 1) - 48;
		}
		if (sum == 0 && k>=maxdig) return out;
		out = (char)(sum % 10 + 48) + out;
		sum = sum / 10;
	}
}
```
  <!-- more -->
大整数乘法代码：

``` c++ 
// 大整数乘法 配套上面的使用
string BIGtimes(string m, string n)
{
	vector<string> adds;
	for (int i = n.size() - 1; i >= 0; i--)
	{
		string line="";
		int  sum = 0;
		for (int k = m.size() - 1; k >= 0; k--)
		{
			sum += (n.at(i) - 48) * (m.at(k) - 48);
			line = (char)(sum % 10 + 48)+line;
			sum = sum / 10;
		}
		if(sum!=0)
		line = (char)(sum % 10 + 48)+line;
		for (int k = n.size()-1; k > i; k--)line = line + '0';
		adds.push_back(line);
	}
	return BIGadd(adds);
}

```
