```c++
template <class _EW = int, class _TW = long long>
class max_flow {
private:
	int *dep, *ptr, S, T, size;
	vector<vector<int> > &G;
	_EW inf;
	const double eps;
	bool bfs() {
		memset(dep, -1, sizeof(int) * (maxn + 5));
		queue<int> Q;
		Q.push(S);
		dep[S] = 0;
		while (!Q.empty()) {
			int cur = Q.front();
			Q.pop();
			int size = G[cur].size();
			for (register int i = 0; i < size; i++) {
				edge &e = E[G[cur][i]];
				if (e.cap > eps && dep[e.to] == -1) {
					dep[e.to] = dep[cur] + 1;
					Q.push(e.to);
				}
			}
		}
		return dep[T] != -1;
	}
	_EW dfs(int cur, const _EW flow) {
		if (flow < eps)
			return 0;
		if (cur == T)
			return flow;
		_EW add{}, used{};
		int size = G[cur].size();
		for (; ptr[cur] != size; ptr[cur]++) {
			int nowa = G[cur][ptr[cur]];
			if (E[nowa].cap < eps || dep[E[nowa].to] != dep[cur] + 1)
				continue;
			add = dfs(E[nowa].to, flow - used < E[nowa].cap ? flow - used : E[nowa].cap);
			if (add > eps) {
				used += add;
				E[nowa].cap -= add;
				E[nowa ^ 1].cap += add;
				if (fabs(used - flow) < eps)
					break;
			}
		}
		return used;
	}
public:
	int maxn;
	struct edge {
		int to;
		_EW cap;
	};
	vector<edge> E;
	max_flow(int n, vector<vector<int> > &g, int *_ptr, int *_dep)
		: maxn(n), G(g), ptr(_ptr), dep(_dep), size(0), eps(1e-6) {
		memset(&inf, 0x4f, sizeof(_EW));
	}
	inline void reset(int n) {
		maxn = n;
		for (int i = 0; i <= n; i++)
			G[i].clear();
		E.clear();
		size = 0;
	}
	inline int add_edge(const int &u, const int &v, const _EW &cap) {
		G[u].push_back(size++);
		G[v].push_back(size++);
		E.push_back(edge{v, cap});
		E.push_back(edge{u, 0});
		return size - 2;
	}
	_TW get_max_flow(const int s, const int t) {
		S = s, T = t;
		_TW maxflow{};
		while (bfs()) {
			memset(ptr, 0, sizeof(int) * (maxn + 5));
			maxflow += dfs(s, inf);
		}
		return maxflow;
	}
};
```

