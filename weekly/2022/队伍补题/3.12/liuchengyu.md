# 补题报告

## 平方数(https://ac.nowcoder.com/acm/contest/6112/A)

## 思路

这题让求离n最近的平方数，令m=sqrt(n)，比较n-m*m和(m+1)*(m+1)-n的大小即可，复杂度O(1)，官方题解用的居然是暴力枚举法，时间复杂度O(√n)，明明只有一组数据，真不知道为啥要暴力枚举

## 代码

```cpp
#include <iostream>
#include <cmath>
using namespace std;
int main()
{
    long long n;
    cin >> n;
    long long m=sqrt(n);
    if((n-m*m)<((m+1)*(m+1)-n)) cout << m*m << endl;
    else cout << (m+1)*(m+1) << endl;
    return 0;
}
```

## 异或图(https://ac.nowcoder.com/acm/contest/6112/B)

## 思路

异或图这题不看题解是真想不到做法，看完题解发现代码就几行，做法离谱极了。题目的意思有点复杂，可以点开题目链接自己理解理解啥意思。总之，做法就是从给出的x点开始对整个图进行广搜，说是广搜，其实图中只有a[u]^a[v]==k的两点u、v之间才有边，所以只需判断是否有点权为a[x]^k的点即可。如果有这样的点，继续广搜，来到第二层，有边的条件依然为a[u]^a[v]==k，那么如果有这样的点，这个点的点权会满足如下情况：a[u]^a[x]==0，即a[u]==a[x]。如果有这样的点，继续广搜，来到第三层，会发现此时所有满足条件的点已经在第一层中被走过了，由此可知，这样的异或图最多有两层。综上，如果a[x]^a[y]==k，说明从x到y只需要一步，如果a[x]^a[y]==0且存在一个点的点权为a[x]^k，说明x到y只需要两步，否则，从x无法到达y。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 1000005
int a[N],vis[1<<20];
int main()
{
    int n,m;
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)
    {
        scanf("%d",&a[i]);
        vis[a[i]]=1;
    }
    while(m--)
    {
        int k,u,v;
        scanf("%d%d%d",&k,&u,&v);
        if((a[u]^a[v])==k) printf("1\n");
        else if((a[u]^a[v])==0&&vis[a[u]^k]) printf("2\n");
        else printf("-1\n");
    }
    return 0;
}
```

## 公因子(https://ac.nowcoder.com/acm/contest/6112/C)

## 思路

这道题不看题解也做不出来……牛客练习赛难度还是不小的，能做出来两道都不错了。题意依然自行看题目理解，这道题是一道数学题，要用到gcd(a,b)=gcd(a,b-a)这个公式，先求出a数组的差分数组b，这样一来对数组a整体加x可简化为对b[1]加x，令A=b[1]+x,B=gcd(b[2],b[3],……,b[n])，则题目要我们求的问题可以转化成求最大的gcd(A+x,B)以及在满足gcd(A+x,B)最大的情况下最小的x，这时题目就好做了，只需让B整除A+x即可。注意，如果求出的B为负值需要取负号变成正值，并且根据A对B求余的结果的正负需要对x进行不同的操作，详细方法见代码（处理这么多可能是我太菜了）。

## 代码

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
#define N 1000005
long long number[N],sum[N];
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++)
    {
        cin >> number[i];
        sum[i]=number[i]-number[i-1];
    }
    long long g=sum[2];
    for(int i=2;i<=n;i++) g=__gcd(g,sum[i]);
    if(g<0) g=-g;
    long long m=sum[1]%g;
    if(m<=0) cout << g << " " << -m;
    else cout << g << " " << g-m << endl;
    return 0;
}
```
