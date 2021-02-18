链接：https://nanti.jisuanke.com/t/41298

Here is a square matrix of$\ n∗n\ $, each lattice has its value ($\ n$ must be odd), and the center value is $ n∗n $. Its spiral decline along the center of the square matrix (the way of spiral decline is shown in the following figure:)

![img](https://res.jisuanke.com/img/upload/20190826/3b1ac98800e8ff9958638769f0ea80956b2c552f.png)

![img](https://res.jisuanke.com/img/upload/20190826/e188ba2f470e2afbf1d21ac4d1887e2f225dd40b.png)

**The grid in the lower left corner is (1,1) and the grid in the upper right corner is (n , n)**

Now I can choose $m$ squares to build palaces, The beauty of each palace is equal to the digital sum of the value of the land which it is located. Such as (the land value is $123213123213$,the beautiful values of the palace located on it is $1+2+3+2+1+3=12$) ($666$ -> $18$) ($456$ ->$15$)

Next, we ask p*p* times to the sum of the beautiful values of the palace in the matrix where the lower left grid$(x_1,y_1)$, the upper right square $(x_2,y_2)$.

### Input

The first line has only one number $T$ .Representing $T$-group of test data $(T\le 5)$

The next line is three number: $n \ m \ p$

The $m$ lines follow, each line contains two integers the square of the palace $(x, y )$

The $p$ lines follow, each line contains four integers : the lower left grid $(x_1,y_1)$ the upper right square $(x_2,y_2)$

### Output

Next, $p_1+p_2...+p_T$ lines: Represent the answer in turn$(n \le 10^6)(m , p \le 10^5)$

#### 样例输入

```
1
3 4 4
1 1
2 2
3 3
2 3
1 1 1 1
2 2 3 3
1 1 3 3
1 2 2 3
```

#### 样例输出

```
5
18
23
17
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
#define reg register
#define ednl endl
#define edn end

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
};
ll get(const int &n, const int &i, const int &j)
{
    const int ci = n / 2 + 1, cj = n / 2 + 1;
    ll center = (ll)n * n;
    ll radius = max(abs(ci - i), abs(cj - j)) - 1;
    if (j == cj + radius + 1)
    {
        if (i == ci + radius + 1)
            return center - (radius * 2 + 1 + 2) * (radius * 2 + 1 + 2) + 1;
        return center - (radius * 2 + 1) * (radius * 2 + 1) - (ci + radius - i);
    }
    ll ans = center - (radius * 2 + 1) * (radius * 2 + 1) - (radius * 2 + 1);
    if (i == ci - radius - 1)
    {
        return ans - (cj + radius + 1 - j);
    }
    ans -= (radius * 2 + 2);
    if (j == cj - radius - 1)
    {
        return ans - (i - (ci - radius - 1));
    }
    ans -= (radius * 2 + 2);
    return ans - (j - (cj - radius - 1));
}
inline int getdigits(ll x)
{
    int ans = 0;
    while (x)
    {
        ans += x % 10;
        x /= 10;
    }
    return ans;
}
struct ele
{
    int x, y, flag, id, yptr;
    bool operator<(const ele &r) const
    {
        if (x != r.x)
            return x < r.x;
        if (yptr != r.yptr)
            return yptr < r.yptr;
        return flag < r.flag;
    }
};
struct __ans
{
    int _1, _2, _3, _4;
};
int main()
{
    int T;
    scanf("%d", &T);
    while (T--)
    {
        int n, m, p;
        scanf("%d%d%d", &n, &m, &p);
        vector<ele> Q(m + 4 * p);
        vector<int> __2D(m + 3 * p);
        vector<__ans> ans(p);
        int x, y, a, b, c, d;
        int size_1 = 0, size_2 = 0;
        for (int i = 0; i < m; i++)
        {
            scanf("%d%d", &x, &y);
            Q[size_1++] = ele{x, y, 0, 0, 0};
            __2D[size_2++] = y;
        }
        for (int i = 0; i < p; i++)
        {
            scanf("%d%d%d%d", &a, &b, &c, &d);
            Q[size_1++] = ele{a - 1, b - 1, 1, i, 0};
            __2D[size_2++] = b;
            Q[size_1++] = ele{c, d, 4, i, 0};
            __2D[size_2++] = d;
            __2D[size_2++] = b - 1;
            Q[size_1++] = ele{a - 1, d, 2, i, 0};
            Q[size_1++] = ele{c, b - 1, 3, i, 0};
        }
        sort(__2D.begin(), __2D.end());
        __2D.resize(distance(__2D.begin(), unique(__2D.begin(), __2D.end())));
        for (auto &i : Q)
            i.yptr = lower_bound(__2D.begin(), __2D.end(), i.y) - __2D.begin();
        sort(Q.begin(), Q.end());
        fenwick<int> bit(__2D.size());
        for (auto &i : Q)
        {
            if (!i.flag)
                bit.modify(i.yptr, getdigits(get(n, i.x, i.y)));
            else if (i.flag == 1)
                ans[i.id]._1 = bit.query(i.yptr);
            else if (i.flag == 2)
                ans[i.id]._2 = bit.query(i.yptr);
            else if (i.flag == 3)
                ans[i.id]._3 = bit.query(i.yptr);
            else
                ans[i.id]._4 = bit.query(i.yptr);
        }
        for (auto &i : ans)
            printf("%d\n", i._4 - i._2 - i._3 + i._1);
    }
    return 0;
}
```

