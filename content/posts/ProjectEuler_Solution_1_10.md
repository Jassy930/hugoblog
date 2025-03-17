---
title: Project Euler  题解：1~10
comment: true
toc: true
tags:
  - Project Euler
abbrlink: e9742231
date: 2019-06-12 16:51:05
updated: 2019-06-12 16:51:05
---

> Working is not boring, Earning money is boring.

自己做的Project Euler的题解，不见得最优，也不见得优雅，但是答案是对的。基本上可以暴力求解的都是直接暴力求的。

## [Problem 1:Multiples of 3 and 5](https://projecteuler.net/problem=1)

暴力遍历，能整除3或者5的话累加

``` c++
int problem1()
{
	long num = 0;
	for (int i = 0; i < 1000; i++)
	{
		if (i % 3 == 0 || i % 5 == 0)
			num += i;
	}
	return num;
}
```

## [Problem 2:Even Fibonacci numbers](https://projecteuler.net/problem=2)

把4000000以下的 fib 数列都求出来，再遍历一般累加奇数。

这里其实 fib 数列的奇偶性是有规律的，按照 奇-奇-偶 的形式无限循环，所以也可以用数列的第几项是否能被3整除来判断。

``` c++
int problem2()
{
	long num = 0;
	vector<int> fib;
	fib.push_back(1);
	fib.push_back(2);
	while (fib.back() < (4000000))
	{
		fib.push_back(fib.at(fib.size() - 1) + fib.at(fib.size() - 2));
	}
	for (auto f : fib)
	{
		if (f < 4000000) {
			cout << f << endl;
			if (f % 2 == 0) num += f;
		}
	}
	cout << endl;
	return num;
}

```

## [Problem 3:Largest prime factor](https://projecteuler.net/problem=3)

从小打到大找因数，当找到一个因数之后将原始数字除以这个因数，再继续判断。

``` c++
long long problem3()
{
	vector<long long > nums;
	long long num = 600851475143;
	long long n = 2;
	while (n <= num)
	{

		if (num % n == 0)
		{
			nums.push_back(n);
			num = num / n;
		}
		else {
			n++;
		}
		cout << n << endl;
	}
	cout << endl << endl;
	for (auto i : nums)
		cout << i << endl;
	cout << endl << endl;
	return nums.back();
}

```

## [Problem 4:Largest palindrome product](https://projecteuler.net/problem=4)

暴力遍历，判断是回文数后记录最大值。

``` c++
int problem4()
{
	int num = 0;
	int n = 100;
	int m = 100;
	for (int m = 100; m <= 999; m++)
	{
		for (int n = 100; n <= 999; n++)
		{
			stringstream ss;
			int nn = n * m;
			ss << nn;
			string aa = ss.str();
			string bb = aa;
			reverse(bb.begin(), bb.end());
//			cout << aa << "  " << bb;
			if (aa.compare(bb) == 0)
			{
				num = MAX(num, n * m);
			}
		}
		cout << m << endl;
	}
	return num;
}

```

## [Problem 5:Smallest multiple](https://projecteuler.net/problem=5)

这个没写程序，直接在计算器里乘了一下。。。

## [Problem 6:Sum square difference](https://projecteuler.net/problem=6)

嗯，算数题，算就对了。

``` c++
int problem6()
{
	int num1=0;
	int num2 = 0;
	for (int i = 1; i <= 100; i++)
	{
		num1 += i * i;
		num2 += i;
	}
	num2 = num2 * num2;
	return num2 - num1;

}

```

## [Problem 7:10001st prime](https://projecteuler.net/problem=7)

判断素数，前面有一篇文章有详细解释。

``` c++
int problem7()
{
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
}

```

## [Problem 8:Largest product in a series](https://projecteuler.net/problem=8)

暴力遍历

``` c++
int problem8()
{
	long long out = 0;
	string number = "7316717653133062491922511967442657474235534919493496983520312774506326239578318016984801869478851843858615607891129494954595017379583319528532088055111254069874715852386305071569329096329522744304355766896648950445244523161731856403098711121722383113622298934233803081353362766142828064444866452387493035890729629049156044077239071381051585930796086670172427121883998797908792274921901699720888093776657273330010533678812202354218097512545405947522435258490771167055601360483958644670632441572215539753697817977846174064955149290862569321978468622482839722413756570560574902614079729686524145351004748216637048440319989000889524345065854122758866688116427171479924442928230863465674813919123162824586178664583591245665294765456828489128831426076900422421902267105562632111110937054421750694165896040807198403850962455444362981230987879927244284909188845801561660979191338754992005240636899125607176060588611646710940507754100225698315520005593572972571636269561882670428252483600823257530420752963450";
	vector<int> numbers;
	for (auto i : number)
	{
		numbers.push_back(i - 48);
	}
	for (int i = 0; i < numbers.size() - 13; i++)
	{
		long long num = 1;
		for (int k = i; k < i + 13; k++)
		{
			num = num * numbers.at(k);
		}
		cout << i <<"  "<<out<< endl;
		out = MAX(out, num);
	}

	/*for (int i = 0; i < 13; i++)
	{
		num = num * numbers.at(i);
		out = num;
	}
	for (int i = 13; i < numbers.size(); i++)
	{
		num = num * numbers.at(i);
		num = num / numbers.at(i - 13);
		out = MAX(out, num);
	}*/
	return out;
}

```

## [Problem 9:Special Pythagorean triplet](https://projecteuler.net/problem=9)

暴力遍历

``` c++
void problem9()
{
	int a = 1;
	int b = 1;
	for (a = 1; a < 1000; a++)
	{
		for (b = 1; b < 1000; b++)
		{
			int cc = a * a + b * b;
			int c = 1000 - a - b;
			if (c * c == cc)
				cout << "a" << a << "b" << b << "c" << c << "out:"<<a*b*c<<endl;
		}
	}
}

```

## [Problem 10:Summation of primes](https://projecteuler.net/problem=10)

同 Problem 7 看之前的文章。

``` c++
void problem10()
{
	vector<int> prime;
	long long sum=0;
	for (int i = 2; i<2000000; i++)
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
		//for (int k = 2; k < i; k++)
		//{
		//	if (i % k == 0) {
		//		n++; break;
		//	}
		//}
		if (n == 0) { prime.push_back(i); sum += i; cout << i << endl;; }
	}
	cout << sum;
}

```

