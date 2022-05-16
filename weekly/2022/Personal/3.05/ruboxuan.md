# C - Jumping Takahashi

[C-Jumping Takahashi](https://vjudge.csgrandeur.cn/contest/481846#problem/C)

问经过***n***步能否由给定的每步的距离刚好从坐标点***0***走到坐标系***x***

可以用动态规划来解决

我们用状态***1***来表示能走到，用状态***0***表示无法走到，通过初状态***f(0,0)=1***可以得到以下公式：

<img src="http://latex.codecogs.com/svg.latex?f(i,a&plus;j)=f(i-1,j)/f(i,b&plus;j)=f(i-1,j)" title="http://latex.codecogs.com/svg.latex?f(i,a+j)=f(i-1,j)/f(i,b+j)=f(i-1,j)" />
这其中***i***表示经过的步数，***j***表示当前状态所在坐标，当然超过x点的部分我们可以不考虑，因此可以加一个条件进行判断。

```c++
#include<math.h>
#include<iostream>
#include<stdlib.h>
#include<string.h>
#include<algorithm>
using namespace std;
const int maxn=1e5+10;
const int INF=1e9;
const int mode=1e9+7;
int dp[110][maxn];
int main()
{
    int n,x;
    dp[0][0]=1;
    scanf("%d%d",&n,&x);
    for(int i=1;i<=n;i++)
    {
        int a,b;
        scanf("%d%d",&a,&b);
        for(int j=0;j<=x;j++)
        {
            if(dp[i-1][j] == 1)
            {
                if(a+j <= x)
                    dp[i][a+j]=dp[i-1][j];
                if(b+j <= x)
                    dp[i][b+j]=dp[i-1][j];
            }
        }
    }
    if(dp[n][x] == 1)
        printf("Yes\n");
    else
        printf("No\n");
    return 0;
}
```

# D - Strange Balls

[D-Strange Balls](https://vjudge.csgrandeur.cn/contest/481846#problem/D)

向一个桶里每次塞一个球，每个球都有相应编号，当正好编号个球摞在一起时，这些球消除，实时问桶里球个数

用栈的思想储存，模拟整个过程即可

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
int stk[maxn],v[maxn];
int main()
{
    int n;
    int cnt=0;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        int x;
        scanf("%d",&x);
        v[++cnt]=x;
        if(cnt == 1||v[cnt] != v[cnt-1])
            stk[cnt]=1;
        else
            stk[cnt]=stk[cnt-1]+1;
        if(stk[cnt] == x)
            cnt-=x;
        printf("%d\n",cnt);
    }
    return 0;
}
```
