链接：https://nanti.jisuanke.com/t/41391

# I. query

Given a permutation $p$ of length $n$, you are asked to answer $m$ queries, each query can be represented as a pair ($l ,r\ $), you need to find the number of pair ($i\ ,j\ $) such that $l \le i < j \le r$ and $\min(p_i,p_j) = \gcd(p_i,p_j )$.

### Input

There is two integers $n$($1 \le n \le 10^5$), $m$($1 \le m \le 10^5$) in the first line, denoting the length of $p$ and the number of queries.

In the second line, there is a permutation of length $n$, denoting the given permutation $p$. It is guaranteed that $p$ is a permutation of length $n$.

For the next $m$ lines, each line contains two integer $l_i$ and $r_i$($1 \le l_i \le r_i \le n$), denoting each query.

### Output

For each query, print a single line containing only one integer which denotes the number of pair($i,\ j\ $).

#### 样例输入

```
3 2
1 2 3
1 3
2 3
```

#### 样例输出

```
2
0
```

#### 代码

```c++
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

template <typename T>
class fenwick
{
public:
	vector<T> arr;
	int n;
	fenwick(int _n) : n(_n)
	{
		arr.resize(n);
	}
	void modify(int x, T v)
	{
		while (x < n)
		{
			arr[x] += v;
			x |= (x + 1);
		}
	}
	T query(int x)
	{
		T v{};
		while (x >= 0)
		{
			v += arr[x];
			x = (x & (x + 1)) - 1;
		}
		return v;
	}
	typename vector<T>::iterator lower_bound(T val)
	{
		int first = 0, last = n;
		while (first < last)
		{
			int mid = first + ((last - first) >> 1);
			if (this->query(mid) < val)
				first = mid + 1;
			else
				last = mid;
		}
		return arr.begin() + first;
	}
	typename vector<T>::iterator upper_bound(T val)
	{
		int first = 0, last = n;
		while (first < last)
		{
			int mid = first + ((last - first) >> 1);
			if (!(val < this->query(mid)))
				first = mid + 1;
			else
				last = mid;
		}
		return arr.begin() + first;
	}
	typename vector<T>::iterator begin() { return arr.begin(); }
	typename vector<T>::iterator end() { return arr.end(); }
};

struct ele
{
	pii p;
	int tag, ans;
	bool operator<(const ele &r) const
	{
		if (p.first != r.p.first)
			return p.first < r.p.first;
		return tag < r.tag;
	}
};

int main()
{
	int n, m;
	scanf("%d%d", &n, &m);
	vector<int> pert(n + 1);
	for (int i = 1; i <= n; i++)
	{
		int tmp;
		scanf("%d", &tmp);
		pert[tmp] = i;
	}
	vector<ele> p;
	for (int i = 1; i <= n / 2; i++)
	{
		int l = pert[i];
		for (int j = 2 * i; j <= n; j += i)
		{
			int &r = pert[j];
			p.push_back(ele{__pair(min(l, r), max(l, r)), 0, -1});
		}
	}
	int l, r;
	for (int i = 0; i < m; i++)
	{
		scanf("%d%d", &l, &r);
		p.push_back(ele{__pair(l - 1, l - 1), 1, i});
		p.push_back(ele{__pair(r, r), 4, i});
		p.push_back(ele{__pair(l - 1, r), 2, i});
		p.push_back(ele{__pair(r, l - 1), 3, i});
	}
	sort(all(p));
	vector<ll> ans(m);
	fenwick<ll> bit(n + 5);
	for (auto &i : p)
	{
		if (!i.tag)
		{
			bit.modify(i.p.second, 1);
		}
		else
		{
			auto tmp = bit.query(i.p.second);
			switch (i.tag)
			{
			case 1:case 4:
				ans[i.ans]+=tmp;
				break;
			case 2:case 3:
				ans[i.ans]-=tmp;
			default:
				break;
			}
		}
	}
	for (auto &i : ans)
		cout << i << ednl;
	return 0;
}
```