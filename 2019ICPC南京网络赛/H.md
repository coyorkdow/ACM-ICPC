题目链接：https://nanti.jisuanke.com/t/41305

# H. Holy Grail

As the current heir of a wizarding family with a long history,unfortunately, you find yourself forced to participate in the cruel Holy Grail War which has a reincarnation of sixty years.However,fortunately,you summoned a Caster Servant with a powerful Noble Phantasm.When your servant launch her Noble Phantasm,it will construct a magic field,which is actually a **directed** **graph** consisting of **$n$ vertices** and **$m$ edges**.More specifically,the graph satisfies the following restrictions :

- Does not have multiple edges(for each pair of vertices **$x$** and **$y$**, there is at most one edge between this pair of vertices in the graph) and does not have self-loops(edges connecting the vertex with itself).
- May have **negative-weighted edges**.
- Does not have a **negative-weighted loop**.
- **$n<=300$** , **$m<=500$**.

Currently,as your servant's Master,as long as you add extra **$6$** edges to the graph,you will beat the other 6 masters to win the Holy Grail.

However, you are subject to the following restrictions when you add the edges to the graph:

- Each time you add an edge whose cost is $c$, it will cost you c units of Magic Value.Therefore,you need to add an edge which has the lowest weight(it's probably that you need to add an edge which has a negative weight).
- Each time you add an edge to the graph,the graph must not have negative loops,otherwise you will be engulfed by the Holy Grail you summon.

### Input

Input data contains multiple test cases. The first line of input contains integer **$t$** — the number of testcases $(1 \le t \le 5)$.

For each test case,the first line contains two integers $n$, $m$, the number of vertices in the graph, the initial number of edges in the graph.

Then $m$ lines follow, each line contains three integers **$x, y$ and $w$** (**$0 \le x,y<n$ $0≤x,y<n$**,$-10^9≤w≤10^9$, $x \not = y$) denoting an edge from vertices $x$ to $y$ (**0-indexed**) of weight $w$.

Then $6$ lines follow, each line contains two integers **$s,t$** denoting **the starting vertex** and **the ending vertex** of the edge you need to add to the graph.

It is guaranteed that there is not an edge starting from **$s$ to $t$** before you add any edges and there must exists such an edge which has the lowest weight and satisfies the above restrictions, meaning the solution absolutely exists for each query.

### Output

For each test case,output $6$ lines.

Each line contains the weight of the edge you add to the graph.

#### 样例输入

```
1
10 15
4 7 10
7 6 3
5 3 3
1 4 11
0 6 20
9 8 25
3 0 9
1 2 15
9 0 27
5 2 0
7 3 -5
1 7 21
5 0 1
9 3 16
1 8 4
4 1
0 3
6 9
2 1
8 7
0 4
```

#### 样例输出

```
-11
-9
-45
-15
17
7
```

#### 代码
```c++
#include <bits/stdc++.h>
typedef long long ll;
typedef unsigned long long ull;
#define pii pair<int, int>
#define mem(a, x) memset(a, x, sizeof a)
#define precision(n) fixed << setprecision(n)
#define quickio ios::sync_with_stdio(false), cin.tie(0), cout.tie(0)
#define reg register
#define ednl endl
#define edn end
using namespace std;

bitset<(int)500> vis;
ll dis[500];
const int maxn = 4e5 + 10;
struct edge
{
    int to;
    ll w;
};
vector<edge> G[maxn];

bool bellman(int S)
{
    deque<int> Q;
    vis.reset();
    memset(dis,0x3f,sizeof(dis));
    dis[S] = 0;
    Q.push_back(S);
    vis.set(S);
    register ll tmp;
    while (!Q.empty())
    {
        int cur = Q.front();
        Q.pop_front();
        vis.reset(cur);
        for (vector<edge>::iterator it = G[cur].begin(); it != G[cur].end(); it++)
        {
            tmp = it->w + dis[cur];
            if (tmp < dis[it->to])
            {
                if (it->to == S)
                {
                    return false;
                }
                dis[it->to] = tmp;
                if (vis.test(it->to))
                    continue;
                vis.set(it->to);
                if (!Q.empty() && dis[Q.front()] < tmp)
                    Q.push_back(it->to);
                else
                    Q.push_front(it->to);
            }
        }
    }
    return true;
}

int main()
{
    quickio;
    int T;
    cin >> T;
    while (T--)
    {
        int n, m;
        int u, v, w;
        cin >> n >> m;
        for(int i=0;i<=n;i++)
            G[i].clear();
        for (int i = 0; i < m; i++)
        {
            cin >> u >> v >> w;
            G[u].push_back(edge{v, w});
        }
        for (int i = 0; i < 6; i++)
        {
            cin >> u >> v;
            G[u].push_back({v, 0});
            int ptr=G[u].size()-1;
            ll l = 0, r = 2 * n * (ll)1e9 + 1;
            ll base = n * (ll)1e9;
            while (l < r)
            {
                ll mid = (r + l) / 2;
                G[u][ptr].w=mid-base;
                if(!bellman(u))
                    l=mid+1;
                else
                    r=mid;
            }
            cout<<l-base<<ednl;
            G[u][ptr].w=l-base;
        }
    }
    return 0;
}
```