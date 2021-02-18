```c++
template <typename W>
class max_flow {
 private:
  int maxn, S, T;
  vector<int> dep, ptr;
  const W eps = (W)1e-9;
  vector<vector<int>>& G;
  W inf;
  bool bfs() {
    fill(all(dep), -1);
    queue<int> Q;
    Q.push(S);
    dep[S] = 0;
    while (!Q.empty()) {
      int cur = Q.front();
      Q.pop();
      for (auto& i : G[cur]) {
        edge& e = E[i];
        if (e.cap > eps && dep[e.to] == -1) {
          dep[e.to] = dep[cur] + 1;
          Q.push(e.to);
        }
      }
    }
    return dep[T] != -1;
  }
  W dfs(int cur, const W flow) {
    if (flow < eps) return 0;
    if (cur == T) return flow;
    W add{}, used{};
    for (; ptr[cur] != G[cur].size(); ptr[cur]++) {
      int nowa = G[cur][ptr[cur]];
      if (E[nowa].cap <= eps || dep[E[nowa].to] != dep[cur] + 1) continue;
      add = dfs(E[nowa].to,
                flow - used < E[nowa].cap ? flow - used : E[nowa].cap);
      if (add > eps) {
        used += add;
        E[nowa].cap -= add;
        E[nowa ^ 1].cap += add;
        if (used - flow <= eps) break;
      }
    }
    return used;
  }

 public:
  struct edge {
    int to;
    W cap;
  };
  vector<edge> E;
  max_flow(int n, vector<vector<int>>& g)
      : maxn(n), G(g), inf(numeric_limits<W>::max() / 2) {
    G.assign(maxn, vector<int>());
    dep.resize(maxn);
    ptr.resize(maxn);
  }
  inline int add_edge(const int& u, const int& v, const W& cap) {
    G[u].push_back(E.size());
    G[v].push_back(E.size() + 1);
    E.push_back(edge{v, cap});
    E.push_back(edge{u, 0});
    return E.size() - 2;
  }
  W get_maxflow(const int s, const int t) {
    S = s, T = t;
    W maxflow{};
    while (bfs()) {
      fill(all(ptr), 0);
      maxflow += dfs(s, inf);
    }
    return maxflow;
  }
};
```

