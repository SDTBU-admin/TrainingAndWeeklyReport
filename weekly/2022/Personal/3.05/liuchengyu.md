# 补题报告

## Edge Checker(https://atcoder.jp/contests/abc240/tasks/abc240_a)

## 思路

略

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int x,y;
    cin >> x >> y;
    if(y-x==1||x==1&&y==10) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

## Count Distinct Integers(https://atcoder.jp/contests/abc240/tasks/abc240_b)

## 思路

直接开个map，别的不解释

## 代码

```cpp
#include <iostream>
#include <map>
using namespace std;
map <int,int> mp;
int main()
{
    int n;
    cin >> n;
    int ans=0;
    for(int i=1;i<=n;i++)
    {
        int number;
        cin >> number;
        if(!mp[number]) ans++;
        mp[number]++;
    }
    cout << ans << endl;
    return 0;
}
```

## Jumping Takahashi(https://atcoder.jp/contests/abc240/tasks/abc240_c)

## 思路

考虑dp，最直接的思路是dp[i][j]表示第i步是否能到j，其实可以将空间复杂度降到一维，但是开二维数组好理解空间复杂度1e6也可以接受。可以得到如下状态转移方程：if(dp[i-1][j]){dp[i][j+a[i]]=1;dp[i][j+b[i]]=1;}，时间复杂度1e6，可以接受还好理解。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 105
int a[N],b[N],dp[N][N*N];
int main()
{
    int n,x;
    cin >> n >> x;
    for(int i=1;i<=n;i++) cin >> a[i] >> b[i];
    dp[1][a[1]]=1,dp[1][b[1]]=1;
    for(int i=2;i<=n;i++)
    {
        for(int j=1;j<=10000;j++)
        {
            if(dp[i-1][j])
            {
                dp[i][j+a[i]]=1;
                dp[i][j+b[i]]=1;
            }
        }
    }
    if(dp[n][x]) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

## Strange Balls(https://atcoder.jp/contests/abc240/tasks/abc240_d)

## 思路

考虑开一个这样的栈来处理这道题：stack <pair <int,int> > s，其中，pair的第一个元素表示当前小球的标号，pair的第二个元素表示有多少个连续的，标号与当前小球相同的小球。这样一来，当s.top().first==s.top().second时将栈顶连续且相同的小球出栈，否则进栈。执行以上步骤n次，每次在循环的最后输出当前栈的大小即可，复杂度O(n)。

## 代码

```cpp
#include <iostream>
#include <stack>
using namespace std;
#define N 200005
int number[N];
stack <pair <int,int> > s;
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++) cin >> number[i];
    for(int i=1;i<=n;i++)
    {
        if(s.empty())
            s.push(make_pair(number[i],1));
        else
        {
            if(number[i]==s.top().first) s.push(make_pair(number[i],s.top().second+1));
            else s.push(make_pair(number[i],1));
        }
        if(s.top().first==s.top().second)
        {
            int flag=s.top().first;
            while(s.size()&&s.top().first==flag) s.pop();
        }
        cout << s.size() << endl;
    }
    return 0;
}
```

## Ranges on Tree(https://atcoder.jp/contests/abc240/tasks/abc240_e)

## 思路

这题要求如果一个子树属于另一个子树，那么这个子树所表示的区间要属于另一个子树所表示的区间，如果一个子树与另一个子树没有交集，那么这两个子树所表示的区间也没有交集。这题是一个典型的树的问题，考虑深搜从叶子节点向根遍历，由于每个叶子节点之间的关系可以认为是互不相关的子树，所以我们先遍历找到所有的叶子节点并给它们编号，用深搜。完成这一步之后，我们再来考虑其它节点表示的区间，可以发现其它节点表示的区间是以它为根的子树所包含的叶子节点的编号，所以我们再用一次深搜完成其它节点表示的区间即可。

## 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;
#define N 200005
#define INF 0x7fffffff
int cnt,l[N],r[N];
vector <int> edge[N];
void search_leaves(int fa,int u)
{
    if(u!=1&&edge[u].size()==1)
    {
        int number=++cnt;
        l[u]=number;
        r[u]=number;
        return ;
    }
    for(int i=0;i<edge[u].size();i++)
    {
        int v=edge[u][i];
        if(v==fa) continue;
        search_leaves(u,v);
    }
}
void dfs(int fa,int u)
{
    if(!l[u]) l[u]=INF;
    if(!r[u]) r[u]=-INF;
    for(int i=0;i<edge[u].size();i++)
    {
        int v=edge[u][i];
        if(v==fa) continue;
        dfs(u,v);
        l[u]=min(l[u],l[v]);
        r[u]=max(r[u],r[v]);
    }
}
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<n;i++)
    {
        int u,v;
        cin >> u >> v;
        edge[u].push_back(v);
        edge[v].push_back(u);
    }
    search_leaves(0,1);
    dfs(0,1);
    for(int i=1;i<=n;i++) cout << l[i] << " " << r[i] << endl;
    return 0;
}
```

## Sum Sum Max(https://atcoder.jp/contests/abc240/tasks/abc240_f)

## 思路

这个F题让我们求区间的前缀和的前缀和，通过我的思考发现，前缀和是将一个一个的数据连成一维，而前缀和的前缀和是将一维的前缀和连成二维，有积分的思想。个人认为这题类似于高中的求导求出递增区间之后再积分求面积。但是这题是离散的，可以通过条件判断递增区间的边界，再通过循环相加计算面积也就是最终的结果，但是这只是我的猜想，因为这题我敲不出来……

## 代码

鸽了
