链接：https://ac.nowcoder.com/acm/problem/17339来源：牛客网

## 题目描述

Chiaki is interested in an infinite sequence $a1, a2, a3, ..$., which defined as follows: 
$$
\begin{cases}0 & n = 1 \\ a_{\lfloor \frac{n}{2} \rfloor} + (-1)^{\frac{n(n+1)}{2}} & n \ge 2\end{cases}
$$
Chiaki would like to know the sum of the first n terms of the sequence, i.e. $\sum\limits_{i=1}^{n}|a_i|$. As this number may be very large, Chiaki is only interested in its remainder modulo ($10^9 + 7$).

## 输入描述:

```
There are multiple test cases. The first line of input contains an integer T (1 ≤ T ≤ 10^5), indicating the number of test cases. For each test case:The first line contains an integer n (1 ≤ n ≤ 10^18).
```

## 输出描述:

```
For each test case, output an integer denoting the answer.
```

## 输入

```
10
1
2
3
4
5
6
7
8
9
10
```

## 输出

```
0
1
2
2
4
4
6
7
8
11
```

### 代码

```c++
// #pragma comment(linker, "/stack:200000000")
// #pragma GCC optimize("Ofast")
// #pragma GCC target("sse,sse2,sse3,ssse3,sse4,popcnt,abm,mmx,avx,tune=native")
#include <bits/stdc++.h>
using namespace std;
#define all(x) x.begin(), x.end()
#define revall(x) x.rbegin(), x.rend()
#define mem(a, x) memset(a, x, sizeof a)
#define precision(n) fixed << setprecision(n)
#define quickio                  \
	ios::sync_with_stdio(false); \
	cin.tie(0), cout.tie(0)
#define reg register
#define ednl endl
#define edn end
#define __pair(a, b) make_pair(a, b)
#define __for(a, x, y) for (auto a = x; a != y; a++)
typedef long long ll;
typedef unsigned long long ull;
typedef pair<int, int> pii;
const int mod = (int)1e9 + 7;
int dp[64][2][2][130];
int dfs(int pos, int pre, int sum, bool lead, bool limit, ull &number)
{
	if (pos == -1)
		return abs(sum - 64);
	if (~dp[pos][pre][lead][sum] && !limit)
		return dp[pos][pre][lead][sum];
	int ans = 0;
	if (lead)
	{
		ull l = (1ull << pos) & number;
		ans += dfs(pos - 1, 0, sum, lead, limit && !l, number);
		ans %= mod;
		if (!limit || l)
			ans += dfs(pos - 1, 1, sum, !lead, limit && l, number);
		ans %= mod;
	}
	else
	{
		ull l = (1ull << pos) & number;
		ans += dfs(pos - 1, 0, sum + (pre == 0 ? 1 : -1), lead, limit && !l, number);
		ans %= mod;
		if (!limit || l)
			ans += dfs(pos - 1, 1, sum + (pre == 1 ? 1 : -1), lead, limit && l, number);
		ans %= mod;
	}
	return limit ? ans : dp[pos][pre][lead][sum] = ans;
}
int main()
{
	quickio;
	int T;
	cin >> T;
	mem(dp, -1);
	while (T--)
	{
		ull n;
		cin >> n;
		cout << dfs(log2(n), 0, 64, true, true, n) << endl;
	}
	return 0;
}
```

