链接：https://nanti.jisuanke.com/t/41303
# F. Greedy Sequence

You're given a permutation $a$ of length $n (1 \le n \le 10^5)$.

For each $i \in [1,n]$, construct a sequence $s_i$ by the following rules:

1. $s_i[1]=i$;
2. The length of $s_i$ is $n$, and for each $j \in [2, n]$, $s_i[j] \le s_i[j-1]$;
3. First, we must choose all the possible elements of $s_i$ from permutation $a$. If the index of $s_i[j]$ in permutation $a$ is $pos[j]$, for each $j \ge 2$, $|pos[j]-pos[j-1]|\le k∣$ $(1 \le k \le 10^5)$. And for each $s_i$, every element of $s_i$ must occur in $a$ **at most once**.
4. After we choose all possible elements for $s_i$, if the length of $s_i$ is smaller than $n$, the value of **every undetermined element** of $s_i$ is 00;
5. For each $s_i$, we must make its weight high enough.

Consider two sequences $C = [c_1, c_2, ... c_n]$ and $D=[d_1, d_2, ..., d_n]$ , we say the weight of $C$ is **higher than** that of $D$ if and only if there exists an integer $k$ such that $1 \le k \le n$, $c_i=d_i$ for all $1 \le i < k$, and $c_k > d_k$.

If for each $i \in [1,n]$, $c_i=d_i$, the weight of $C$ is equal to the weight of $D$.

For each $i \in [1,n]$, print the number of non-zero elements of $s_i$ separated by a space.

It's guaranteed that there is only one possible answer.

### Input

There are multiple test cases.

The first line contains one integer $T(1 \le T \le 20)$, denoting the number of test cases.

Each test case contains two lines, the first line contains two integers $n$ and $k$ $(1 \le n,k \le 10^5)$, the second line contains $n$ distinct integers $a_1, a_2, ..., a_n$ $(1 \le a_i \le n)$ separated by a space, which is the permutation $a$.

### Output

For each test case, print one line consists of $n$ integers $|s_1|, |s_2|, ..., |s_n|$ separated by a space.

$|s_i|$ is the number of non-zero elements of sequence $s_i$.

There is no space at the end of the line.

#### 样例输入

```
2
3 1
3 2 1
7 2
3 1 4 6 2 5 7
```

#### 样例输出

```
1 2 3
1 1 2 3 2 3 3
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

int dfs(int cur, const vector<int> &nxt, vector<int> &ans)
{
    if (nxt[cur] == -1)
        return ans[cur] = 1;
    else if (ans[cur])
        return ans[cur];
    else
        return ans[cur] = dfs(nxt[cur], nxt, ans) + 1;
}
int main()
{
    int T;
    scanfint(T);
    while (T--)
    {
        int n, k;
        scanfint(n);
        scanfint(k);
        vector<int> arr(n);
        vector<int> nxt(n);
        vector<pii> pos(n);
        for (int i = 0; i < n; i++)
        {
            scanfint(arr[i]);
            pos[i].first = arr[i];
            pos[i].second = i;
        }
        sort(all(pos));
        auto lb = [&](int x) {
            int l = 0, r = pos.size();
            while (l < r)
            {
                int mid = l + ((r - l) >> 1);
                if (pos[mid].first < x)
                    l = mid + 1;
                else
                    r = mid;
            }
            return pos[l].second;
        };
        set<int> S;
        int back = 0, j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && j - i <= k)
                S.insert(arr[j++]);
            if (i - back > k)
                S.erase(arr[back++]);
            auto it = S.find(arr[i]);
            if (it == S.begin())
                nxt[i] = -1;
            else
                nxt[i] = lb(*--it);
        }
        vector<int> ans(n);
        for (int i = 0; i < n; i++)
            dfs(i, nxt, ans);
        for (auto it = pos.begin(); it != pos.end(); it++)
        {
            if (!(it == pos.begin()))
                putchar(' ');
            printf("%d", ans[it->second]);
        }
        putchar('\n');
    }
    return 0;
}
```

