# 补题报告

## Shampoo(https://atcoder.jp/contests/abc243/tasks/abc243_a)

## 思路

这题问洗发水到谁那不够用，先用v对a+b+c求余，算出到最后一轮还能剩下多少，再用剩下的跟a、a+b、a+b+c比较大小即可。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int v,a,b,c;
    cin >> v >> a >> b >> c;
    v%=(a+b+c);
    if(v<a) cout << "F" << endl;
    else if(v<a+b) cout << "M" << endl;
    else cout << "T" << endl;
    return 0;
}
```

## Hit and Blow(https://atcoder.jp/contests/abc243/tasks/abc243_b)

## 思路

这题无论是a数组还是b数组中的数字都是没有重复的，也就是说每个数字只出现一次，每个数字对应着一个唯一的下标，所以我们可以开两个map来存储a数组和b数组每个数字对应的下标。然后遍历存储了a数组数字对应下标的map，当存储了b数组数字对应下标的map中也有这个键时，看两者的值是否相同，如果相同ans1++，如果不同ans2++。

## 代码

```cpp
#include <iostream>
#include <map>
using namespace std;
#define N 1005
int a[N],b[N];
map <int,int> mp1,mp2;
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++)
    {
        cin >> a[i];
        mp1[a[i]]=i;
    }
    for(int i=1;i<=n;i++)
    {
        cin >> b[i];
        mp2[b[i]]=i;
    }
    int ans1=0,ans2=0;
    for(auto it:mp1)
    {
        if(mp2[it.first])
        {
            if(mp2[it.first]==it.second) ans1++;
            else ans2++;
        }
    }
    cout << ans1 << endl;
    cout << ans2 << endl;
    return 0;
}
```

## Collision 2(https://atcoder.jp/contests/abc243/tasks/abc243_c)

## 思路

这题问你是否会发生相撞，和上一题相同，依旧考虑开两个map，mp1[x]表示在x行向左走的最大横坐标，mp2[x]表示在x行向右走的最小横坐标，注意每当在输入数据时碰到一个新的行时就要给这一行的mp1和mp2初始化为无穷。然后依旧是遍历mp1，当某一行x的mp1[x]>mp2[x]时，表示当前行x最大的向左走的坐标大于最小的向右走的坐标，肯定会发生相撞，输出"No"，当所有的行都满足条件时则输出"Yes"。

## 代码

```cpp
#include <iostream>
#include <map>
using namespace std;
#define N 200005
int x[N],y[N];
map <int,int> mp1,mp2;
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++)
    {
        cin >> x[i] >> y[i];
        mp1[y[i]]=0x80000000;
        mp2[y[i]]=0x7fffffff;
    }
    string s;
    cin >> s;
    for(int i=0;i<n;i++)
    {
        if(s[i]=='L') mp1[y[i+1]]=max(mp1[y[i+1]],x[i+1]);
        if(s[i]=='R') mp2[y[i+1]]=min(mp2[y[i+1]],x[i+1]);
    }
    int flag=1;
    for(auto it:mp1)
    {
        if(it.second>mp2[it.first])
        {
            flag=0;
            break;
        }
    }
    if(flag) cout << "No" << endl;
    else cout << "Yes" << endl;
    return 0;
}
```

## Moves on Binary Tree(https://atcoder.jp/contests/abc243/tasks/abc243_d)

## 思路

这题乍一看是个水题，但仔细一想就会发现，当前点的编号可能会变态的大，就不知道咋做了。看了别人的代码，发现解法也是真的巧。仔细理解题目会发现这题在过程中产生的编号可能会变态的大，但是题目还说保证最后的答案在1e18之内，也就是说用long long就能存下，说明这题无论在过程中会向下走多深，在最后肯定会回到最上面这大概六十层之内。既然这样，我们就开一个cnt变量，如果从当前点再向下走时点的编号会超过long long的范围，就给cnt加1，如果向上走时发现cnt不为0，就给cnt减1。除此之外的操作，也就是点的坐标能用long long存下的操作，直接进行乘2除2的普通操作即可。

## 代码

```cpp
#include <iostream>
using namespace std;
#define INF 1e18
int main()
{
    int n;
    long long x;
    cin >> n >> x;
    string s;
    cin >> s;
    int cnt=0;
    for(int i=0;i<n;i++)
    {
        if(s[i]=='U')
        {
            if(cnt) cnt--;
            else x>>=1;
        }
        else
        {
            if(x<<1>INF) cnt++;
            else x=(x<<1)+(s[i]=='R');
        }
    }
    cout << x << endl;
    return 0;
}
```

## Edge Deletion(https://atcoder.jp/contests/abc243/tasks/abc243_e)

## 思路

这题也是不太会做，看的别人的代码。大佬的代码思路很简单，考虑用Floyd算法计算多源最短路，每当一条路径可以被更新时，如果这条路径没有被标记计入过答案，就给答案加1，将这条边标记计入过答案，最终的答案就是ans/2，因为是无向图，每条可以被删除的边都算了两次，所以答案应除2。

## 代码

```cpp
#include <iostream>
#include <cstring>
using namespace std;
#define N 305
int mp[N][N],dis[N][N];
int main()
{
    memset(dis,0x3f,sizeof(dis));
    int n,m;
    cin >> n >> m;
    for(int i=1;i<=m;i++)
    {
        int u,v,w;
        cin >> u >> v >> w;
        mp[u][v]=mp[v][u]=1;
        dis[u][v]=dis[v][u]=min(dis[u][v],w);
    }
    int ans=0;
    for(int k=1;k<=n;k++)
    {
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(dis[i][j]>=dis[i][k]+dis[k][j])
                {
                    dis[i][j]=dis[i][k]+dis[k][j];
                    ans+=mp[i][j];
                    mp[i][j]=0;
                }
            }
        }
    }
    cout << ans/2 << endl;
    return 0;
}
```
