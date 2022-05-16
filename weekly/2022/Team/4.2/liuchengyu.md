# 补题报告

## 梦想赛道(https://ac.nowcoder.com/acm/contest/11180/A)

## 思路

一道看似吓人实则很简单的题，注意题目中的这一句话：“图中允许有重边”，这句话很重要，一下子就使这道题简单了许多。这样考虑这道题：图中允许有重边，那我们就给一个边权非1的边旁边再加上一个边权为1的边即可，这样我们原来的图就是一个次小生成树了。另外，当所有的边权均为1时，这时我们无法找到一颗次小生成树，因为无论怎么选得到的都是最小生成树。综上，当所有边的权值均为1时输出-1，否则输出所有边权相加再加1。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    int sum=0,flag=1;
    for(int i=1;i<n;i++)
    {
        int u,v,w;
        cin >> u >> v >> w;
        sum+=w;
        if(w!=1) flag=0;
    }
    if(flag) cout << -1 << endl;
    else cout << sum+1 << endl;
    return 0;
}
```

## 寒冬信使(https://ac.nowcoder.com/acm/contest/11180/B)

## 思路

一道博弈题，考虑缩小问题规模。可以发现一些操作是多余的，即使将一些牌翻转了将来也会翻转回来，那么考虑最小翻转次数，最小翻转次数的奇偶代表着谁能赢。那么现在问题转化成了求最小翻转次数，可以发现每一次翻转只对它本身及其前面一个牌产生影响，这样一来，从后向前翻的话就能算出最小翻转次数，如果该数是奇数的话则先手胜，否则先手败。

## 代码

```cpp
#include <iostream>
#include <string.h>
using namespace std;
#define N 100005
char s[N];
int main()
{
    int _;
    cin >> _;
    while(_--)
    {
        cin >> s+1;
        int len=strlen(s+1);
        int cnt=0;
        for(int i=len;i>=1;i--)
        {
            if(s[i]=='1')
            {
                s[i]='0';
                s[i-1]=s[i-1]=='1'?'0':'1';
                cnt++;
            }
        }
        if(cnt&1) cout << "T" << endl;
        else cout << "X" << endl;
    }
    return 0;
}
```

## 盾与战锤(https://ac.nowcoder.com/acm/contest/11180/C)

## 思路

这题让我们求的是能造成的最大伤害，注意可以多次破盾，造成的伤害是累积的。这题不太会，看的网上的思路，首先每次必定是能够把盾破开的，否则这些攻击没有意义。假设盾被破开i次，那么就能够抵抗dnf*i点伤害，我们可以使用i*k次攻击，由于抵御的伤害确定，所以我们肯定是选择最大的那些。所以按照伤害排序，然后对于每个k枚举破开多少次盾就好了。

## 代码

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
#define N 1000005
long long number[N],sum[N];
bool cmp(long long x,long long y)
{
    return x>y;
}
int main()
{
    int n;
    long long s;
    scanf("%d%lld",&n,&s);
    for(int i=1;i<=n;i++) scanf("%lld",&number[i]);
    sort(number+1,number+1+n,cmp);
    for(int i=1;i<=n;i++) sum[i]=sum[i-1]+number[i];
    for(int i=1;i<=n;i++)
    {
        long long ans=0;
        for(int j=1;j<=n;j+=i) ans+=max((long long)0,sum[min(j+i-1,n)]-sum[j-1]-s);
        printf("%lld\n",ans);
    }
    return 0;
}
```
