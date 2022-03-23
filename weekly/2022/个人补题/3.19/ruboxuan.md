# 基础离散数学练习题

[基础离散数学练习题](http://www.zjutacm.cn/problem/2001)

记录每个数出现次数，并找一个最大的数***x***使得**x = 其出现次数**

```c++
#include<math.h>
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<algorithm>
using namespace std;
const int maxn=2e5+10;
const int INF=1e9;
const int mode=1e9+7;
int num[maxn];
int cnt[maxn];
bool cmp(int a,int b)
{
    return a>b;
}
int main()
{
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        int x;
        scanf("%d",&x);
        num[i]=x;
        cnt[x]++;
    }
    sort(num,num+n,cmp);
    for(int i=0;i<n;i++)
    {
        if(num[i] == cnt[num[i]])
        {
            printf("%d\n",num[i]);
            return 0;
        }
    }
    printf("-1\n");
    return 0;
}
```

# 翻牌

[翻牌](http://www.zjutacm.cn/problem/2022)

根据题目要求，每次翻牌的左端点都要求朝上，因此能选的最大位即为**n-k+1**，依次将**n-k+1**、**n-2k+1**......**n-n/k+1**的状态记录，正面为**1**，反面为**0**，得到状态函数**f(n,k)**

可以发现函数初值为**n/k**，而根据抽屉原理两次相邻操作函数值变化为**1**，因此只需要根据**n/k**奇偶性即可判断胜负关系

**n/k**为奇数先手赢，偶数后手赢

```c++
#include<math.h>
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<algorithm>
using namespace std;
const int maxn=2e5+10;
const int INF=1e9;
const int mode=1e9+7;
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        int n,k;
        scanf("%d%d",&n,&k);
        if(n/k%2 != 0)
            printf("Steve\n");
        else
            printf("Hugin\n");
    }
    return 0;
}
```

## Minimize Ordering

[B - Minimize Ordering](https://atcoder.jp/contests/abc242/tasks/abc242_b)

求字典序最小字符串，水题

```c++
#include<math.h>
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<algorithm>
using namespace std;
const int maxn=2e5+10;
const int INF=1e9;
const int mode=1e9+7;
char s[maxn];
int main()
{
    int len;
    scanf("%s",s);
    len=strlen(s);
    int a[26]={0};
    for(int i=0;i<len;i++)
    {
        int t=s[i]-97;
        a[t]++;
    }
    for(int i=0;i<26;i++)
    {
        for(int j=0;j<a[i];j++)
        {
            printf("%c",i+97);
        }
    }
    return 0;
}
```

# **1111gal password**

[C - 1111gal password](https://atcoder.jp/contests/abc242/tasks/abc242_c)

简单dp

**1**与**9**时状态方程与常态不同，需要单独考虑

```c++
#include<math.h>
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<algorithm>
using namespace std;
const int maxn=2e6+10;
const int INF=1e9;
const int mode=998244353;
int dp[maxn][10];
int main()
{
    int n;
    scanf("%d",&n);
    for(int i=1;i<10;i++)
        dp[1][i]=1;
    for(int i=2;i<=n;i++)
    {
        for(int j=1;j<10;j++)
        {
            if(j == 1)
            {
                dp[i][j]=(dp[i-1][j]+dp[i-1][j+1])%mode;
            }
            else if(j == 9)
            {
                dp[i][j]=(dp[i-1][j-1]+dp[i-1][j])%mode;
            }
            else
                dp[i][j]=((dp[i-1][j-1]+dp[i-1][j])%mode+dp[i-1][j+1])%mode;
        }
    }
    int ans=0;
    for(int i=1;i<10;i++)
    {
        ans+=dp[n][i]%mode;
        ans%=mode;
    }
    printf("%d\n",ans);
    return 0;
}
```

