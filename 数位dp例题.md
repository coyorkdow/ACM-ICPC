# 不要62

```c++
#include <bits/stdc++.h>
using namespace std;
int number[20], dp[20][2];
int dfs(int pos, int pre, int limit) {
  if (pos == 0) return 1;
  if (!limit && ~dp[pos][pre == 6]) return dp[pos][pre == 6];
  int ans = 0, n = limit ? number[pos] : 9;
  for (int i = 0; i <= n; i++)
    ans += (i == 4 || (pre == 6 && i == 2))
               ? 0
               : dfs(pos - 1, i, limit && i == number[pos]);
  return limit ? ans : dp[pos][pre == 6] = ans;
}
int cal(int x) {
  int len = 0;
  while (x) {
    number[++len] = x % 10;
    x /= 10;
  }
  return dfs(len, -1, true);
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  memset(dp, -1, sizeof(dp));
  int n, m;
  while (cin >> n >> m) {
    if (n == 0 && m == 0) break;
    cout << cal(m) - cal(n - 1) << endl;
  }
  return 0;
}
```

------

# Balanced Number

**Problem Description**

A balanced number is a non-negative integer that can be balanced if a pivot is placed at some digit. More specifically, imagine each digit as a box with weight indicated by the digit. When a pivot is placed at some digit of the number, the distance from a digit to the pivot is the offset between it and the pivot. Then the torques of left part and right part can be calculated. It is balanced if they are the same. A balanced number must be balanced with the pivot at some of its digits. For example, $4139$ is a balanced number with pivot fixed at $3$. The torqueses are $4*2 + 1*1 = 9$ and $9*1 = 9$, for left part and right part, respectively. It's your job
to calculate the number of balanced numbers in a given range $[x, y]$.

**Input**

The input contains multiple test cases. The first line is the total number of cases $T (0 < T ≤ 30)$. For each case, there are two integers separated by a space in a line, x and y. $(0 ≤ x ≤ y ≤ 10^{18})$.

**Output**

For each case, print the number of balanced numbers in the range [x, y] in a line.

**Sample Input**

```
2
0 9
7604 24324
```

**Sample Output**

```
10
897
```

**代码**

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int a[20];
ll dp[20][20][2005];
ll dfs(int pivot, int pos, int num, bool limit) {
  if (!limit && ~dp[pivot][pos][num]) return dp[pivot][pos][num];
  if (pos == 0) return num == 0;
  long long ans = 0;
  int n = limit ? a[pos] : 9;
  for (int i = 0; i <= n; i++) {
    ans += dfs(pivot, pos - 1, i * (pivot - pos) + num, limit && i == a[pos]);
  }
  return !limit ? dp[pivot][pos][num] = ans : ans;
}
ll cal(ll x) {
  int len = 0;
  while (x) {
    a[++len] = x % 10;
    x /= 10;
  }
  long long ans = 0;
  for (int i = 1; i <= len; ++i) ans += dfs(i, len, 0, true);
  return ans - len + 1;
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0), cout.tie(0);
  int T;
  ll x, y;
  cin >> T;
  memset(dp, -1, sizeof(dp));
  while (T--) {
    cin >> x >> y;
    cout << cal(y) - cal(x - 1) << endl;
  }
  return 0;
}
```

------

# 11038 - How Many O's

A Benedict monk No. 16 writes down the decimal representations of all natural numbers between and including $m$ and $n$, $m ≤ n$. How many $0$’s will he write down?

**Input**

Input consists of a sequence of lines. Each line contains two unsigned 32-bit integers $m$ and $n$, $m ≤ n$. The last line of input has the value of $m$ negative and this line should not be processed.

**Output**

For each line of input print one line of output with one integer number giving the number of $0$’s written down by the monk.

**Sample Input**

```
10 11
100 200
0 500
1234567890 2345678901
0 4294967295
-1 -1
```

**Sample Output**

```
1
22
92
987654304
3825876150
```

输入两个数字$A$和$B$($A\le B$)，计算从$A$写到$B$共需要写多少个$0$（包含$A,B$）？

可以用带上界不带下界的数位dp计算从$0$到$n$的数字共有多少个$0$（用``cal(n)``表示），答案便是``cal(B)-cal(A-1)``。特别地若$n<0$则``cal(n)``返回值为$0$。

状态设计为$dp[i][j][k][m(0/1)][n(0/1)]$，表示第$i$位（当前状态下的最高位）的值是$j$，且一共有$k$位为零的总数。$m$和$n$均为布尔量，分别表示是否是前导零和是否是上界，这对应了两种特殊情况。

在非特殊情况下，状态转移方程为
$$
\begin{cases}
dp[i][j][k]=\sum_{l}{dp[i-1][l][k-1]}& j=0 \\
dp[i][j][k]=\sum_ldp[i-1][l][k]& j\neq0 \\
\end{cases}
$$
下面通过一个例子解释前导零和上界这两种情况。

我们设计的状态转移方程是从低位向高位转移，即从第$i-1$位转移到第$i$位。考虑数字$14530529$，这是一个8位数，第6位是6而第5位是3。一般情况下，第6位是0到第6位是9都是合法的状态。但倘若在第8位是1而第7位是4的情况下，第6位大于5的情况就不能被转移到第7位：否则整体状态会大于$14530529$。我们称第8位是1，第7位是4这样的情况为”到达上界“，此时低位超过上界的状态不能转移（第6位不能大于5），而低位恰好到达上界的转移情况也需要特殊考虑：第6位刚好为5，而第5位到第1位的状态不能超过$30529$。因此我们需要为数位恰好为上界的情况设计一种特殊的状态：设当前为第$i$位，则第$i-1$位未达上界时转移普通状态；恰好为上界时转移到达上界的特殊状态；超过上界时不转移。

令$A_i$表示第$i$位的上界，对应到达上界的特殊情况的状态转移方程为
$$
\begin{cases}
dp[i][j][k][到达上界]=\sum_{l<A_{i-1}}{dp[i-1][l][k-1][未到上界]}+dp[i-1][A_{i-1}][k-1][到达上界]&若j=0\\
dp[i][j][k][到达上界]=\sum_{l<A_{i-1}}dp[i-1][l][k][未到上界]+dp[i-1][A_{i-1}][k][到达上界]&若j\neq0\\
\end{cases}
$$
状态转移到第8位时，前7位的状态可以是$0000000$，这是一种合法的状态。但如果第8位也是0，也就是整体状态是$00000000$时，它应该被视为只包含一个$0$：即等价于数字$“0”$。也就是说在最终的结果中需要忽略前导零，于是我们需要为数位恰好为零的情况设计另一种特殊的状态：设当前位为第$i$位，则第$i-1$位为零时转移前导零的状态，其余情况转移普通状态。

对应前导零的特殊情况的状态转移方程为
$$
dp[i][0][k][前导零]=\sum_{l\neq 0}{dp[i-1][l][k-1][非前导零]}+dp[i-1][0][k-1][前导零]
$$
代码如下：

```c++
#include <bits/stdc++.h>
using namespace std;
char A[15], B[15];
long long dp[15][10][15][2][2];
long long cal(char *A, int lenA) {
  memset(dp, 0, sizeof(dp));
  for (int i = 0; i <= 9; i++) dp[0][i][i == 0][0][0] = 1;
  dp[0][A[0]][A[0] == 0][0][1] = 1;
  dp[0][0][1][1][0] = 1;
  if (A[0] == 0) dp[0][0][1][1][1] = 1;
  for (int i = 1; i < lenA; i++) {
    for (int j = 0; j < 10; j++) {
      for (int k = 0; k <= i; k++) {
        if (j == A[i]) {
          for (int l = 0; l <= 9; l++) {
            dp[i][j][k + (j ? 0 : 1)][0][0] += dp[i - 1][l][k][0][0];
            if (j == 0) dp[i][j][k][1][0] += dp[i - 1][l][k][l ? 0 : 1][0];
            if (l > A[i - 1]) continue;
            dp[i][j][k + (j ? 0 : 1)][0][1] +=
                dp[i - 1][l][k][0][A[i - 1] == l];
            if (j == 0)
              dp[i][j][k][1][1] += dp[i - 1][l][k][l ? 0 : 1][A[i - 1] == l];
          }
        } else {
          for (int l = 0; l <= 9; l++) {
            dp[i][j][k + (j ? 0 : 1)][0][0] += dp[i - 1][l][k][0][0];
            if (j == 0) dp[i][j][k][1][0] += dp[i - 1][l][k][l ? 0 : 1][0];
          }
        }
      }
    }
  }
  long long ansA = 0;
  for (int i = 0; i <= A[lenA - 1]; i++)
    for (int j = 0; j <= lenA; j++)
      ansA += dp[lenA - 1][i][j][i ? 0 : 1][i == A[lenA - 1]] * j;
  return ansA;
}
int main() {
  long long int a, b;
  while (cin >> a >> b) {
    if (a < 0 || b < 0) break;
    strcpy(A, to_string(a - 1).c_str());
    strcpy(B, to_string(b).c_str());
    int lenA = strlen(A), lenB = strlen(B);
    for (int i = lenA - 1; i >= 0; i--) A[i] -= '0';
    for (int i = lenB - 1; i >= 0; i--) B[i] -= '0';
    for (int i = 0; i < lenA / 2; i++) swap(A[i], A[lenA - 1 - i]);
    for (int i = 0; i < lenB / 2; i++) swap(B[i], B[lenB - 1 - i]);
    long long ansA = a == 0 ? 0 : cal(A, lenA), ansB = cal(B, lenB);
    cout << ansB - ansA << endl;
  }
  return 0;
}
```

------

# Chiaki Sequence Reloaded

**题目描述**

Chiaki is interested in an infinite sequence $a1, a2, a3, ..$., which defined as follows: 
$$
\begin{cases}0 & n = 1 \\ a_{\lfloor \frac{n}{2} \rfloor} + (-1)^{\frac{n(n+1)}{2}} & n \ge 2\end{cases}
$$
Chiaki would like to know the sum of the first n terms of the sequence, i.e. $\sum\limits_{i=1}^{n}|a_i|$. As this number may be very large, Chiaki is only interested in its remainder modulo ($10^9 + 7$).

**输入描述:**

```
There are multiple test cases. The first line of input contains an integer T (1 ≤ T ≤ 10^5), indicating the number of test cases. For each test case:The first line contains an integer n (1 ≤ n ≤ 10^18).
```

**输出描述:**

```
For each test case, output an integer denoting the answer.
```

**输入**

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

**输出**

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

**代码**

```c++
#include <bits/stdc++.h>
using namespace std;
#define quickio                \
  ios::sync_with_stdio(false); \
  cin.tie(0), cout.tie(0)
typedef long long ll;
typedef unsigned long long ull;
const int mod = (int)1e9 + 7;
int dp[64][2][2][130];
int dfs(int pos, int pre, int sum, bool lead, bool limit, ull &number) {
  if (pos == -1) return abs(sum - 64);
  if (~dp[pos][pre][lead][sum] && !limit) return dp[pos][pre][lead][sum];
  int ans = 0;
  if (lead) {
    ull l = (1ull << pos) & number;
    ans += dfs(pos - 1, 0, sum, lead, limit && !l, number);
    ans %= mod;
    if (!limit || l) ans += dfs(pos - 1, 1, sum, !lead, limit && l, number);
    ans %= mod;
  } else {
    ull l = (1ull << pos) & number;
    ans +=
        dfs(pos - 1, 0, sum + (pre == 0 ? 1 : -1), lead, limit && !l, number);
    ans %= mod;
    if (!limit || l)
      ans +=
          dfs(pos - 1, 1, sum + (pre == 1 ? 1 : -1), lead, limit && l, number);
    ans %= mod;
  }
  return limit ? ans : dp[pos][pre][lead][sum] = ans;
}
int main() {
  quickio;
  int T;
  cin >> T;
  memset(dp, -1, sizeof(dp));
  while (T--) {
    ull n;
    cin >> n;
    cout << dfs(log2(n), 0, 64, true, true, n) << endl;
  }
  return 0;
}
```

