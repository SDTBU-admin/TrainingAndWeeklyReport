# 补题报告

## T-shirt(https://atcoder.jp/contests/abc242/tasks/abc242_a)

## 思路

简单的if、else语句判断题，不多说了。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int a,b,c,x;
    scanf("%d%d%d%d",&a,&b,&c,&x);
    double ans;
    if(x<=a) ans=1;
    else if(x<=b) ans=(double)c/(b-a);
    else ans=0;
    printf("%.12lf\n",ans);
    return 0;
}
```

## Minimize Ordering(https://atcoder.jp/contests/abc242/tasks/abc242_b)

## 思路

给一个字符串，输出由这个字符串组成的字典序最小的字符串。直接统计每个字符出现的次数，然后由a到z按照每个字符出现的次数输出即可。

## 代码

```cpp
#include <iostream>
#include <string>
using namespace std;
int cnt[200];
int main()
{
    string s;
    cin >> s;
    for(int i=0;i<s.length();i++) cnt[s[i]]++;
    for(char i='a';i<='z';i++)
        for(int j=1;j<=cnt[i];j++)
            cout << i;
    cout << endl;
    return 0;
}
```

## 1111gal password(https://atcoder.jp/contests/abc242/tasks/abc242_c)

## 思路

考虑dp，dp[i][j]表示第i次操作后以j结尾的情况的个数，易得状态转移方程为：dp[i][j]=(dp[i-1][j-1]+dp[i-1][j]+dp[i-1][j+1])%MOD，由于1没有下限，9没有上限，需要额外处理，简单变化一下状态转移方程即可。最终的答案是dp[n][1]+……dp[n][9]，记得求余。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 1000005
#define MOD 998244353
long long dp[N][10];
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=9;i++) dp[1][i]=1;
    for(int i=2;i<=n;i++)
    {
        for(int j=1;j<=9;j++)
        {
            if(j==1) dp[i][j]=(dp[i-1][j]+dp[i-1][j+1])%MOD;
            else if(j==9) dp[i][j]=(dp[i-1][j-1]+dp[i-1][j])%MOD;
            else dp[i][j]=(dp[i-1][j-1]+dp[i-1][j]+dp[i-1][j+1])%MOD;
        }
    }
    long long ans=0;
    for(int i=1;i<=9;i++) ans=(ans+dp[n][i])%MOD;
    cout << ans << endl;
    return 0;
}
```

## (∀x∀)(https://atcoder.jp/contests/abc242/tasks/abc242_e)

## 思路

这题给定一个字符串，求长度与这个字符串相同且字典序不大于这个字符串的回文串的个数。由于是回文，我们可以将原字符串分成前一半来考虑，通过总结从简单的字符串上发现的规律可以发现，当当前位上的字符是s[i]时，情况总数就变成了ans=(ans+(s[i]-'A'))*26，其中ans代表情况总数。最后还要判断一下字典序最大的一种情况是否大于原字符串，如果不大，答案就再加上1，同时别忘了每次对ans操作都需要求余。

## 代码

```cpp
#include <iostream>
#include <string>
using namespace std;
#define MOD 998244353
int main()
{
    int _;
    cin >> _;
    while(_--)
    {
        int n;
        cin >> n;
        string s;
        cin >> s;
        string t=s;
        long long ans=0;
        for(int i=0;i<=(n-1)/2;i++)
        {
            t[n-1-i]=s[i];
            ans=(ans*26)%MOD;
            ans=(ans+(s[i]-'A'))%MOD;
        }
        if(t<=s) ans=(ans+1)%MOD;
        cout << ans << endl;
    }
    return 0;
}
```

## Range Pairing Query(https://atcoder.jp/contests/abc242/tasks/abc242_g)

## 思路

标准莫队题，由于我板子都丢了（这是一个悲伤的故事），所以这道题是我纯手码的，说实话左右移的顺序还是忘了，自己又手推了一下。莫队的板子题，没什么好说的，增删对于答案的影响也很好想，还有什么不明白的看代码就行。

## 代码

```cpp
#include <iostream>
#include <cmath>
#include <algorithm>
using namespace std;
#define N 1000005
int len,sum,number[N],cnt[N],ans[10*N];
struct Q
{
    int l;
    int r;
    int id;
}q[N];
bool cmp(Q x,Q y)
{
    if(x.l/len!=y.l/len) return x.l<y.l;
    else return x.r<y.r;
}
void del(int pos)
{
    if(cnt[number[pos]]%2==0) sum--;
    cnt[number[pos]]--;
}
void add(int pos)
{
    if(cnt[number[pos]]%2==1) sum++;
    cnt[number[pos]]++;
}
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++) cin >> number[i];
    int m;
    cin >> m;
    for(int i=1;i<=m;i++)
    {
        cin >> q[i].l >> q[i].r;
        q[i].id=i;
    }
    len=sqrt(n);
    sort(q+1,q+1+m,cmp);
    int l=1,r=0;
    for(int i=1;i<=m;i++)
    {
        while(l>q[i].l) add(--l);
        while(r<q[i].r) add(++r);
        while(l<q[i].l) del(l++);
        while(r>q[i].r) del(r--);
        ans[q[i].id]=sum;
    }
    for(int i=1;i<=m;i++) cout << ans[i] << endl;
    return 0;
}
```
