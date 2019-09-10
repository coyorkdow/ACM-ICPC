链接：https://nanti.jisuanke.com/t/41387

# E. XKC's basketball team

XKC , the captain of the basketball team , is directing a train of nn team members. He makes all members stand in a row , and numbers them $1 \cdots n1⋯n$ from left to right.

The ability of the $i$-th person is$ w_i$, and if there is a guy whose ability is not less than $w_i+m$ stands on his right , he will become angry. It means that the $j$-th person will make the $i$-th person angry if $j>i$ and $w_j \ge w_i+m$ 

We define the anger of the ii-th person as the number of people between him and the person , who makes him angry and the distance from him is the longest in those people. If there is no one who makes him angry , his anger is $−1$ .

Please calculate the anger of every team member .

### Input
The first line contains two integers nn and $m$($2\leq n\leq 5*10^5$, $0\leq m \leq 10^9$).

The following  line contain nn integers $w_1..w_n$($0\leq w_i \leq 10^9$).

### Output
A row of nn integers separated by spaces , representing the anger of every member .

#### 样例输入

```
6 1
3 4 5 6 2 10
```

#### 样例输出

```
4 3 2 1 0 -1
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
class segment_tree
{
private:
	vector<T> tree;
	vector<T> lazy;
	T zero;
	inline void down(int k, int len)
	{
		lazy[k << 1] += lazy[k];
		lazy[k << 1 | 1] += lazy[k];
		tree[k << 1] += lazy[k];
		tree[k << 1 | 1] += lazy[k];
		lazy[k] = zero;
	}
public:
	int size;
	segment_tree(int n) : size(n), zero(T{})
	{
		tree.resize(4 * n + 5);
		lazy.resize(4 * n + 5);
	}
	void modify(const int l, const int r, const T value, int ll = 1, int rr = -1, int k = 1)
	{
		if (rr == -1)
			rr = size;
		if (ll >= l && rr <= r)
		{
			tree[k] += value * (rr - ll + 1);
			lazy[k] += value;
			return;
		}
		if (lazy[k] != zero)
			down(k, rr - ll + 1);
		int mid = (ll + rr) >> 1;
		if (r > mid)
			modify(l, r, value, mid + 1, rr, k << 1 | 1);
		if (l <= mid)
			modify(l, r, value, ll, mid, k << 1);
		tree[k] = max(tree[k << 1], tree[k << 1 | 1]);
	}
	T query(const int l, const int r, int ll = 1, int rr = -1, int k = 1)
	{
		if (rr == -1)
			rr = size;
		if (ll >= l && rr <= r)
			return tree[k];
		if (lazy[k] != zero)
			down(k, rr - ll + 1);
		int mid = (ll + rr) >> 1;
		T ans{};
		if (r > mid)
			ans = max(ans, query(l, r, mid + 1, rr, k << 1 | 1));
		if (l <= mid)
			ans = max(ans, query(l, r, ll, mid, k << 1));
		return ans;
	}
};
struct ele
{
	pii w;
	int tag;
	bool operator<(const ele&r)const{
		if(w.first!=r.w.first)
			return w.first<r.w.first;
		return tag<r.tag;
	}
};
int main()
{
	int n, m;
	scanf("%d%d", &n, &m);
	vector<ele> w(2*n);
	int _w, size=0;
	for (int i = 0; i < n; i++)
	{
		scanf("%d", &_w);
		w[size].w=__pair(_w,i);
		w[size].tag=0;
		size++;
		w[size].w=__pair(_w-m,i);
		w[size].tag=1;
		size++;
	}
	sort(all(w));
	segment_tree<ll> tree(n+1);
	vector<int>ans(n);
	int pos=0;
	for(int i=2*n-1;i>=0;i--)
	{
		if(w[i].tag==1){
			tree.modify(w[i].w.second+1,w[i].w.second+1,w[i].w.second);
		}else{
			auto tmp=tree.query(w[i].w.second+2,n+1);
			if(tmp==0)ans[w[i].w.second]=-1;
			else ans[w[i].w.second]=tmp-w[i].w.second-1;
		}
	}
	for(int i=0;i<n;i++)
	{
		if(i)putchar(' ');
		printf("%d",ans[i]);
	}
	putchar('\n');
	return 0;
}
```

