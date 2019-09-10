链接：https://nanti.jisuanke.com/t/41301

# D. Robots

Given a directed graph with no loops which starts at node $1$ and ends at node $n$.

There is a robot who starts at $1$, and will go to one of adjacent nodes or stand still with **equal probability** every day. Every day the robot will have durability consumption which equals to the number of passed days.

Please calculate the expected durability consumption when the robot arrives at node $n$.

It is guaranteed that there is **only one** node (node $1$) whose in-degree is equal to $0$, and there is **only one** node (node $n$) whose out-degree is equal to $0$. And there are no multiple edges in the graph.

### Input

The first line contains one integer $T (1 \le T \le 10)$.

For each case,the first line contains two integers $n (2 \le n \le 10^5)$ and $m (1 \le m \le 2 \times 10^5)$, the number of nodes and the number of edges, respectively.

Each of the next $m$ lines contains two integers $u$ and $v$ ($1 \le u, v \le n$) denoting a directed edge from $u$ to $v$.

It is guarenteed that $\sum n \le 4\times 10^5$, and $\sum m \le 5 \times 10^5$.

### Output

Output $T$ lines.Each line have a number denoting the expected durability consumption when the robot arrives at node $n$.

Please keep **two decimal places**.

#### 样例输入

```
1
5 6
1 2
2 5
1 5
1 3
3 4
4 5
```

#### 样例输出

```
9.78
```

#### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef pair<int, int> pii;
typedef long long ll;
typedef unsigned long long ull;
#define mem(a, x) memset(a, x, sizeof a)
#define precision(n) fixed << setprecision(n)
#define quickio ios::sync_with_stdio(false), cin.tie(0), cout.tie(0)
#define all(x) x.begin(), x.end()
#define scanfint(x) scanf("%d", &x)
#define scanfdouble(x) scanf("%lf", &x)
#define scanfu(x) scanf("%u", &x)
#define scanfll(x) scanf("%lld", &x)
#define scanfllu(x) scanf("%llu", &x)
#define reg register
#define ednl endl
#define edn end
const int maxn = 1e5 + 10;
vector<int> G[maxn];
int vis[maxn], cnt;
void dfs(int cur, vector<int> &topo)
{
    vis[cur]++;
    for (auto &i : G[cur])
    {
        if (!vis[i])
            dfs(i, topo);
    }
    vis[cur]++;
    topo[cnt--] = cur;
}
double dp1[maxn], dp2[maxn];
int main()
{
    quickio;
    int T;
    cin >> T;
    while (T--)
    {
        mem(vis, 0);
        int n, m, u, v;
        cin >> n >> m;
        for (int i = 1; i <= n; i++)
            G[i].clear();
        vector<int> topo(n + 1);
        for (int i = 0; i < m; i++)
        {
            cin >> u >> v;
            G[u].push_back(v);
        }
        cnt = n;
        dfs(1, topo);
        dp1[n] = 0;
        for (auto it = topo.rbegin(); it != topo.rend() - 1; it++)
        {
            if (*it == n)
                continue;
            double sum = 0;
            for (auto &i : G[*it])
                sum += dp1[i];
            dp1[*it] = (sum + G[*it].size() + 1) / G[*it].size();
        }
        dp2[n] = 0;
        for (auto it = topo.rbegin(); it != topo.rend() - 1; it++)
        {
            if (*it == n)
                continue;
            double sum = 0;
            for (auto &i : G[*it])
                sum += dp2[i];
            dp2[*it] = (sum + dp1[*it] * (G[*it].size() + 1)) / G[*it].size();
        }
        cout << precision(2) << dp2[1] << ednl;
    }
    return 0;
}
```

