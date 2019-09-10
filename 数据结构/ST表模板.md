```c++
template <typename T, class compare = less<T>>
class sparse_table
{
private:
	vector<vector<T>> st;
	compare rmq;
	T __rmq(const T& l, const T& r) const
	{
		return rmq(l, r) ? l : r;
	}
public:
	int n;
	sparse_table(int _n, const vector<T>& v) : n(_n)
	{
		st.resize(n);
		for (typename vector<vector<T>>::iterator it = st.begin(); it != st.end(); it++)
			it->resize((int)log2(n) + 1);
		for (int i = 0; i < n; i++)
			st[i][0] = v[i];
		for (int i = 1; (1 << i) <= n; i++)
		{
			for (int j = 0; j + (1 << i) - 1 < n; j++)
				st[j][i] = __rmq(st[j][i - 1], st[j + (1 << (i - 1))][i - 1]);
		}
	}
	T query(int l, int r)
	{
		int k = (int)log2((double)(r - l + 1));
		return __rmq(st[l][k], st[r - (1 << k) + 1][k]);
	}
};
```

