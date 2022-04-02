# 补题报告

## Last Letter(https://atcoder.jp/contests/abc244/tasks/abc244_a)

## 思路

不解释。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    string s;
    cin >> s;
    cout << s[n-1] << endl;
    return 0;
}
```

## Go Straight and Turn Right(https://atcoder.jp/contests/abc244/tasks/abc244_b)

## 思路

开一个变量记录当前的朝向，根据不同的朝向对前进操作的坐标变化做讨论即可。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    string s;
    cin >> s;
    int x=0,y=0,dir=0;
    for(int i=0;i<n;i++)
    {
        if(s[i]=='R')
            dir=(dir+1)%4;
        else
        {
            if(dir==0) x++;
            else if(dir==1) y--;
            else if(dir==2) x--;
            else y++;
        }
    }
    cout << x << " " << y << endl;
    return 0;
}
```

## Yamanote Line Game(https://atcoder.jp/contests/abc244/tasks/abc244_c)

## 思路

这题也没啥好说的，根据题目的描述进行模拟即可，但是这题用scanf会T用cin就能AC，应该是涉及到刷新标准输出的缘故。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    string s[5],t[5];
    for(int i=1;i<=3;i++) cin >> s[i];
    for(int i=1;i<=3;i++) cin >> t[i];
    int cnt=0;
    for(int i=1;i<=3;i++)
        if(s[i]==t[i])
            cnt++;
    if(cnt!=1) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

## Swap Hats(https://atcoder.jp/contests/abc244/tasks/abc244_d)

## 思路

首先考虑对应帽子上颜色不同的情况，只有0，2，3三种情况，不可能出现对应帽子只有1个颜色不一样的情况，因为一旦两个帽子颜色相同，剩下的一个帽子颜色必然相同。接下来再考虑0，2，3这三种情况，0个帽子对应的颜色不同，最少只需要2次交换就能使它们再次相同，10^18次交换能正好交换完；2个帽子对应的颜色不同，最少只需要1此交换就能使它们再次相同，10^18交换不能正好交换完；3个帽子对应的颜色不同，最少只需要2次交换就能使它们再次相同，10^18次交换能正好交换完。综上，只有对应位置上有2个帽子颜色不同的话输出才是No，其余均为Yes。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    string s[5],t[5];
    for(int i=1;i<=3;i++) cin >> s[i];
    for(int i=1;i<=3;i++) cin >> t[i];
    int cnt=0;
    for(int i=1;i<=3;i++)
        if(s[i]==t[i])
            cnt++;
    if(cnt!=1) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

## King Bombee(https://atcoder.jp/contests/abc244/tasks/abc244_e)

## 思路

考虑dp，dp[i][j][0]表示第i步到达j这个点并且经过x这个点偶数次的方法数，相应的，dp[i][j][1]表示第i步到达j这个点并且经过x这个点奇数次的方法数。我们可以这样考虑问题：枚举下一步可以走到哪里，当然这里还有一个问题，就是下一步要走的点如果是x的话要分情况讨论，因为走过x的次数的奇偶性发生了改变，具体的状态转移方程见代码。最后的答案就记录在dp[k][t][0]之中，在计算的过程中不要忘记求余。

## 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;
#define N 2005
#define MOD 998244353
int dp[N][N][2];
vector <int> edge[N];
int main()
{
    int n,m,k,s,t,x;
    cin >> n >> m >> k >> s >> t >> x;
    for(int i=1;i<=m;i++)
    {
        int u,v;
        cin >> u >> v;
        edge[u].push_back(v);
        edge[v].push_back(u);
    }
    dp[0][s][0]=1;
    for(int i=0;i<k;i++)
    {
        for(int j=1;j<=n;j++)
        {
            for(int k=0;k<=1;k++)
            {
                if(dp[i][j][k])
                {
                    for(int l=0;l<edge[j].size();l++)
                    {
                        int pos=edge[j][l];
                        if(pos==x) dp[i+1][pos][!k]=(dp[i+1][pos][!k]+dp[i][j][k])%MOD;
                        else dp[i+1][pos][k]=(dp[i+1][pos][k]+dp[i][j][k])%MOD;
                    }
                }
            }
        }
    }
    cout << dp[k][t][0] << endl;
    return 0;
}
```
