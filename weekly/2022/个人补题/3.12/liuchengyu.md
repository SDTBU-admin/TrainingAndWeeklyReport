# 补题报告

## A ↔ BB(https://atcoder.jp/contests/arc136/tasks/arc136_a)

## 思路

这题给我们一个只含A、B、C的字符串，我们可以将A变成BB，将BB变成A，求经过这样的操作后得到的字典序最小的字符串。这样考虑：先将所有的A换成BB，再从前往后遍历字符串，将BB再变成A，这样得到的字符串字典序是最小的。

## 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;
vector <char> v,ans;
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++)
    {
        char c;
        cin >> c;
        if(c=='A')
        {
            v.push_back('B');
            v.push_back('B');
        }
        else
            v.push_back(c);
    }
    for(int i=0;i<v.size();)
    {
        if(i<v.size()-1&&v[i]=='B'&&v[i+1]=='B')
        {
            ans.push_back('A');
            i+=2;
        }
        else
        {
            ans.push_back(v[i]);
            i+=1;
        }
    }
    for(int i=0;i<ans.size();i++) cout << ans[i];
    cout << endl;
    return 0;
}
```

## Switches(https://atcoder.jp/contests/abc128/tasks/abc128_c)

## 思路

题意见题目，题目让我们求有多少种开关的情况满足所有的灯都处于亮着的状态。可以发现开关的数量最多有10个，且只有开或关两种状态，考虑状态压缩，用N位二进制数表示每个开关的状态，1表示开，0表示关。这样一来，再根据每个开关的状态来判断某个灯是否处于亮着的状态，当所有灯都处于亮着的状态时，该种开关的状态满足条件，答案加一。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 15
int number[N],b[N][N],p[N];
int main()
{
    int n,m;
    cin >> n >> m;
    for(int i=1;i<=m;i++)
    {
        cin >> number[i];
        for(int j=1;j<=number[i];j++) cin >> b[i][j];
    }
    for(int i=1;i<=m;i++) cin >> p[i];
    int ans=0;
    for(int i=0;i<(1<<n);i++)
    {
        int flag=1;
        for(int j=1;j<=m&&flag;j++)
        {
            int cnt=0;
            for(int k=1;k<=number[j];k++)
                if(((i>>(b[j][k]-1))&1))
                    cnt++;
            if(cnt%2!=p[j]%2) flag=0;
        }
        if(flag) ans++;
    }
    cout << ans << endl;
    return 0;
}
```

## Typical Stairs(https://atcoder.jp/contests/abc129/tasks/abc129_c)

## 思路

考虑dp，dp[i]表示到达第i个台阶的情况总数，可以得到如下状态转移方程：当第i个台阶坏了时，dp[i]=0；当第i个台阶没坏时，dp[i]=(dp[i-2]+dp[i-1])%MOD，dp[n]即为最终的答案。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 100005
#define MOD 1000000007
int flag[N],dp[N];
int main()
{
    int n,m;
    cin >> n >> m;
    for(int i=1;i<=m;i++)
    {
        int number;
        cin >> number;
        flag[number]=1;
    }
    dp[0]=1;
    if(!flag[1]) dp[1]=1;
    for(int i=2;i<=n;i++)
    {
        if(flag[i]) dp[i]=0;
        else dp[i]=(dp[i-2]+dp[i-1])%MOD;
    }
    cout << dp[n] << endl;
    return 0;
}
```

## equeue(https://atcoder.jp/contests/abc128/tasks/abc128_d)

## 思路

题意见题目，可以发现这题无论是珠宝数还是操作数都不大，最多100，可以考虑枚举出所有可能的情况。可以发现操作C和操作D其实都是将你手上现有的珠宝放回去，只不过是放回的位置不同，但是对于已经进行完取的操作只剩下放的操作时，可认为操作C和操作D是同种操作。分三层循环枚举所有情况，第一层循环枚举从左边拿多少个珠宝，第二层循环枚举从右边拿多少个珠宝，第三层循环枚举放回去多少个珠宝。当进行完前两层循环后，也就进行完取珠宝的操作后，将现在已有的珠宝按照价值由小到大排序，第三层循环中将其中的小者去掉，每次枚举完后都更新最大值，这样枚举完后的最大值就是答案。

## 代码

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
#define N 55
int number[N];
int main()
{
    int n,k;
    cin >> n >> k;
    for(int i=1;i<=n;i++) cin >> number[i];
    int ans=0;
    for(int l=0;l<=min(n,k);l++)
    {
        for(int r=0;r<=min(n,k)-l;r++)
        {
            vector <int> v;
            int sum=0;
            for(int i=1;i<=l;i++)
            {
                v.push_back(number[i]);
                sum+=number[i];
            }
            for(int i=1;i<=r;i++)
            {
                v.push_back(number[n-i+1]);
                sum+=number[n-i+1];
            }
            sort(v.begin(),v.end());
            for(int i=0;i<min(k-l-r,(int)v.size());i++)
            {
                if(v[i]<0) sum-=v[i];
                else break;
            }
            ans=max(ans,sum);
        }
    }
    cout << ans << endl;
    return 0;
}
```

## Sum Equals Xor(https://atcoder.jp/contests/abc129/tasks/abc129_e)

## 思路

在比赛的时候想这个题想了快一个小时，发现这题是一道找规律的题，但硬是没总结出规律，只是大概知道是3^n+2^n这么个关系。今天回过头来看这道题，结合着网上的题解，总结出了规律。这个规律有什么数学含义我并不知道，所以也没法解释状态转移的过程（网上找的题解也是打表生凑的规律）。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 100005
#define MOD 1000000007
char s[N];
long long dp[N][2];
int main()
{
    cin >> s+1;
    dp[0][0]=0;
    dp[0][1]=1;
    int n=0;
    for(int i=1;s[i];i++)
    {
        dp[i][0]=(dp[i-1][0]*3)%MOD;
        if(s[i]=='1')
        {
            dp[i][1]=(dp[i-1][1]*2)%MOD;
            dp[i][0]=(dp[i][0]+dp[i-1][1])%MOD;
        }
        else
            dp[i][1]=dp[i-1][1];
        n++;
    }
    cout << (dp[n][0]+dp[n][1])%MOD << endl;
    return 0;
}
```
