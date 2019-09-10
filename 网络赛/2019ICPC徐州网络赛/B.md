链接：https://nanti.jisuanke.com/t/41384

# B. so easy

There are $n$ points in an array with index from $1$ to $n$, and there are two operations to those points.

1: $1 \ x$ marking the point $x$ is not available

2: $2 \ x$ query for the index of the first available point after that point (including $x$ itself) .

### Input

$n\quad q$

$z_1 \quad x_1$

$\vdots$

$z_q\quad x_q$

$q$ is the number of queries, $z$ is the type of operations, and $x$ is the index of operations. $1≤x<n<10^9$， $1 \leq q<10^6$ and $z$ is $1$ or $2$.

### Output

Output the answer for each query.

#### 样例输入

```
5 3
1 2
2 2
2 1
```

#### 样例输出

```
3
1
```

#### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef double db;
const ll N=3e6+5;
const db inf=0x3f3f3f3f;
const db eps=1e-14;
int n,m;
int pre[N];
int degree[N];
int weight[N];
int Find(int x){
   return pre[x] == x ? pre[x] : pre[x] = Find(pre[x]);
}
void Union(int a,int b){
    int fa=Find(a);
    int fb=Find(b);
    pre[fa]=fb;
}
vector<int>v;
int getid(int x){return lower_bound(v.begin(),v.end(),x)-v.begin()+1;}
struct node{
    int i,z,d;
}a[N];

int main(){
    //std::ios::sync_with_stdio(false);
    //cin.tie(),cout.tie();
    scanf("%d%d",&n,&m);
    int x,y;
    for(int i=0;i<m;i++){
        cin>>a[i].z>>a[i].d;
        v.push_back(a[i].d);
        v.push_back(a[i].d+1);
        a[i].i=i;
    }
    //v.push_back(-1);
    sort(v.begin(),v.end());
    v.erase(unique(v.begin(),v.end()),v.end());
    for(int i=0;i<=3*m;i++) pre[i]=i;
    for(int i=0;i<m;i++){
        x=a[i].z,y=getid(a[i].d);
        //cout<<x<<' '<<y<<endl;
        if(x==1){
            pre[y]=Find(pre[y+1]);
        }
        else{
            printf("%d\n",v[Find(pre[y])-1]);
            //cout<<v[Find(pre[y])-1]<<endl;
        }
    }
    return 0;
}
```

