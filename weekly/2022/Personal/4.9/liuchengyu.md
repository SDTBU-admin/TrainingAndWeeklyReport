# 补题报告

## Good morning(https://atcoder.jp/contests/abc245/tasks/abc245_a)

## 思路

if、else判断大小的题目，没啥说的

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int a,b,c,d;
    cin >> a >> b >> c >> d;
    if(a<c) cout << "Takahashi" << endl;
    else if(a==c)
    {
        if(b<=d) cout << "Takahashi" << endl;
        else cout << "Aoki" << endl;
    }
    else cout << "Aoki" << endl;
    return 0;
}
```

## Mex(https://atcoder.jp/contests/abc245/tasks/abc245_b)

## 思路

找出最小的没出现过的非负数，数据范围比较小，直接开数组标记即可。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 2005
int flag[N];
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++)
    {
        int number;
        cin >> number;
        flag[number]=1;
    }
    for(int i=0;i<=2000;i++)
    {
        if(!flag[i])
        {
            cout << i << endl;
            break;
        }
    }
    return 0;
}
```

## Choose Elements(https://atcoder.jp/contests/abc245/tasks/abc245_c)

## 思路

这题猛一上来真的是没什么思路，但是仔细读题，这题只是问我们是否可行，不涉及到最值的问题，考虑dp。dp[i][0]表示前i-1位已选完，第i位选择第一个数组的第i个元素是否可行，相应的，dp[i][1]表示前i-1位已选完，第i位选择第二个数组的第i个元素是否可行。由此很容易得到状态转移方程，当(dp[i-1][0]&&abs(a[i]-a[i-1])<=k)||(dp[i-1][1]&&abs(a[i]-b[i-1])<=k)时，dp[i][0]=1，否则为0；当(dp[i-1][0]&&abs(b[i]-a[i-1])<=k)||(dp[i-1][1]&&abs(b[i]-b[i-1])<=k)时dp[i][1]=1，否则为0。最后当dp[n][0]与dp[n][1]两者之中有一个为1时就输出"Yes"，否则输出"No"。

## 代码

```cpp
#include <iostream>
#include <cmath>
using namespace std;
#define N 200005
int a[N],b[N],dp[N][2];
int main()
{
    int n,k;
    cin >> n >> k;
    for(int i=1;i<=n;i++) cin >> a[i];
    for(int i=1;i<=n;i++) cin >> b[i];
    dp[1][0]=dp[1][1]=1;
    for(int i=2;i<=n;i++)
    {
        if((dp[i-1][0]&&abs(a[i]-a[i-1])<=k)||(dp[i-1][1]&&abs(a[i]-b[i-1])<=k)) dp[i][0]=1;
        if((dp[i-1][0]&&abs(b[i]-a[i-1])<=k)||(dp[i-1][1]&&abs(b[i]-b[i-1])<=k)) dp[i][1]=1;
    }
    if(dp[n][0]||dp[n][1]) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

## Polynomial division(https://atcoder.jp/contests/abc245/tasks/abc245_d)

## 思路

一个多项式的分解问题，自己手推一下样例一就能发现做法。在比赛的时候我是由低位向高位跑的，这样就涉及到一个除数可能为0的情况，运行错误了两发，发现由低位向高位跑不合适，就马上改变策略，由高位向低位跑，这样一来就不再会发生运行错误了。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 205
int a[N],b[N],c[N];
int main()
{
    int n,m;
    cin >> n >> m;
    for(int i=0;i<=n;i++) cin >> a[i];
    for(int i=0;i<=n+m;i++) cin >> c[i];
    for(int i=m;i>=0;i--)
    {
        b[i]=c[i+n]/a[n];
        for(int j=0;j<=n;j++) c[j+i]-=a[j]*b[i];
    }
    for(int i=0;i<=m;i++)
    {
        if(!i) cout << b[i];
        else cout << " " << b[i];
    }
    cout << endl;
    return 0;
}
```

## Wrapping Chocolate(https://atcoder.jp/contests/abc245/tasks/abc245_e)

## 思路

比赛的时候没做出来，想着是二分图最大匹配问题，不过这数据量也太大了，跑着会超时，再一看这道题也只是问我们能不能全放下，并没有问最多能放下多少块，就更不会了。看的题解，题解的做法是先把巧克力以及盒子都由大到小sort，之后开一个multiset用来存盒子的相关信息，巧克力和盒子都由大到小跑，如果当前的盒子大于当前的巧克力就把当前的盒子放进multiset里，继续检查下一个盒子是否满足条件，直到当前的盒子不满足条件为止。之后再在multiset里二分查找当前的巧克力适合用哪一个盒子来装，如果最后二分得到的答案是任何一个盒子都装不下，直接标记不可能达到目的，退出循环，否则把这个二分找到的盒子从multiset中删除，继续循环找下一个巧克力适合用哪一个盒子去装。如果循环退出之后flag标记有效则输出"Yes"，标记无效则输出"No"。

## 代码

```cpp
#include <iostream>
#include <set>
#include <algorithm>
using namespace std;
#define N 200005
multiset <int> s;
struct Pos
{
    int x;
    int y;
}a[N],b[N];
bool cmp(Pos x,Pos y)
{
    return x.x>y.x;
}
int main()
{
    int n,m;
    cin >> n >> m;
    for(int i=1;i<=n;i++) cin >> a[i].x;
    for(int i=1;i<=n;i++) cin >> a[i].y;
    for(int i=1;i<=m;i++) cin >> b[i].x;
    for(int i=1;i<=m;i++) cin >> b[i].y;
    sort(a+1,a+1+n,cmp);
    sort(b+1,b+1+m,cmp);
    int flag=1,pos=1;
    for(int i=1;i<=n;i++)
    {
        while(pos<=m&&b[pos].x>=a[i].x)
        {
            s.insert(b[pos].y);
            pos++;
        }
        auto it=s.lower_bound(a[i].y);
        if(it==s.end())
        {
            flag=0;
            break;
        }
        s.erase(it);
    }
    if(flag) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

## Endless Walk(https://atcoder.jp/contests/abc245/tasks/abc245_f)

## 思路

比赛的时候交了一发，结果答案不对，感觉思路很正确啊，就是一个拓扑排序加讨论标记的问题，赛后去atcoder上一看，AC了大部分的数据，只有一小部分是WA的，看来还是有一些小细节没考虑到。赛后看的题解，题解的做法很巧妙，建立反向图，反正如果是环的话反向之后还是个环。之后对这个反向的环进行拓扑排序，由于我们建立的是反向图，从反向图中撤下的点是原图中无法到达环的点，统计这些点的个数，最终的答案就是总点数减去从反向图拓扑排序中撤下的点的个数，题解的做法是真的巧妙。

## 代码

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
#define N 200005
int deg[N];
vector <int> edge[N];
int main()
{
    int n,m;
    cin >> n >> m;
    for(int i=1;i<=m;i++)
    {
        int u,v;
        cin >> u >> v;
        edge[v].push_back(u);
        deg[u]++;
    }
    queue <int> q;
    for(int i=1;i<=n;i++)
        if(!deg[i])
            q.push(i);
    int cnt=0;
    while(!q.empty())
    {
        cnt++;
        int u=q.front();
        q.pop();
        for(auto v:edge[u])
        {
            deg[v]--;
            if(!deg[v]) q.push(v);
        }
    }
    cout << n-cnt << endl;
    return 0;
}
```
