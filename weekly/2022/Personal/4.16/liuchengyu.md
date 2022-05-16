# 补题报告

## Four Points(https://atcoder.jp/contests/abc246/tasks/abc246_a)

## 思路

已知矩形的三个点，求剩下的一个点的坐标，直接统计已知三个点的横、纵坐标，输出出现次数为一的横、纵坐标即可。

## 代码

```cpp
#include <iostream>
using namespace std;
int cnt1[205],cnt2[205];
int main()
{
    for(int i=1;i<=3;i++)
    {
        int x,y;
        cin >> x >> y;
        cnt1[x+100]++;
        cnt2[y+100]++;
    }
    for(int i=0;i<=200;i++)
        if(cnt1[i]==1)
            cout << i-100 << " ";
    for(int i=0;i<=200;i++)
        if(cnt2[i]==1)
            cout << i-100 << endl;
    return 0;   
}
```

## Get Closer(https://atcoder.jp/contests/abc246/tasks/abc246_b)

## 思路

这题啥意思比赛的时候也没细看，直接看样例就能猜到是啥意思，大概意思就是已知一个点的坐标，求原点与这个点的连线与x轴产生的夹角的余弦值以及正弦值，直接算出距离拿横、纵坐标一比即可。

## 代码

```cpp
#include <iostream>
#include <cmath>
using namespace std;
int main()
{
    double x,y;
    cin >> x >> y;
    double z=sqrt(x*x+y*y);
    printf("%.12lf %.12lf\n",x/z,y/z);
    return 0;
}
```

## Coupon(https://atcoder.jp/contests/abc246/tasks/abc246_c)

## 思路

一道有意思的贪心题，有n件物品，k张打折券，每张打折券的面额是x元，每件物品的价格是ai元，问最少需要多少钱才能买完这n件物品。首先，我们先计算出每件物品在其最终成交价大于等于0的前提下能消耗多少张打折券，如果经过这次的循环计算打折券已经全部用完了，那么每件物品最终的成交价的总和就是答案。如果打折券还有剩余，就要进行下一步操作。下一步的操作要用到优先队列，将之前的每一件物品的成交价都放进一个优先队列里，每次都弹出最大值，注意此时优先队列里的值都是大于等于0小于打折券面额x的数，将弹出的最大的值减去，表示这件物品又用了一张打折券，现在已经可以白嫖了。循环执行上一步骤直到打折券用光或者优先队列为空，最后得到的结果即为最小花费。

## 代码

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
#define N 200005
int number[N];
priority_queue <int,vector <int> ,less <int> > q;
int main()
{
    int n,k,x;
    cin >> n >> k >> x;
    for(int i=1;i<=n;i++) cin >> number[i];
    long long ans=0;
    for(int i=1;i<=n;i++)
    {
        int num1=number[i]/x;
        int num2=min(num1,k);
        number[i]-=num2*x;
        k-=num2;
        ans+=number[i];
        q.push(number[i]);
    }
    while(k&&!q.empty())
    {
        int now=q.top();
        q.pop();
        ans-=now;
        k--;
    }
    cout << ans << endl;
    return 0;
}
```

## 2-variable Function(https://atcoder.jp/contests/abc246/tasks/abc246_d)

## 思路

说来惭愧，这道D题整个大一大二的只有一个人过了，还不错，至少不是没人做得出来。比赛的时候我打了下表，发现这题根本没有规律，赛后看题解，发现这题类似于二分但却不是二分。思路是这样的：题目说给的数不会超过1e18，由于是三次方，所以最大值就是1e6，初始时设左指针为0，右指针为1e6，计算两者得出的结果，如果两者得出的结果大于等于题目给的值，就令右指针减一，否则就令左指针加一，注意是加一减一的操作，而不是二分的操作。当左、右指针交换位置后，最终计算的结果即为答案。

## 代码

```cpp
#include <iostream>
using namespace std;
long long cul(long long x,long long y)
{
    return x*x*x+x*x*y+x*y*y+y*y*y;
}
int main()
{
    long long n;
    cin >> n;
    long long ans=0x7fffffffffffffff;
    long long l=0,r=1000000;
    while(l<=r)
    {
        long long res=cul(l,r);
        if(res>=n)
        {
            ans=min(ans,res);
            r--;
        }
        else
            l++;
    }
    cout << ans << endl;
    return 0;
}
```

## Bishop 2(https://atcoder.jp/contests/abc246/tasks/abc246_e)

## 思路

搜索题，大一的李旭也过了这题，但是他的代码能过就很离谱。暴搜，6000ms的时限他用了5929ms，极限。按理说，这道题暴搜是过不去的，大二的有很多人也都在暴搜，结果都是TLE。这道题我比赛时过代码只跑了150ms，效率是暴搜的40倍，思路是这样的：我们开两个vis数组进行标记，vis1[i][j]标记i、j这个点所在的主对角线是否被搜索过，vis2[i][j]表示i、j这个点所在的副对角线是否被搜索过。当我们在按主对角线的方向进行搜索时，如果碰到一个点它的主对角线标记为1就没有必要再搜索下去了，同样的，在按副对角线的方向进行搜索时，如果碰到一个点它的副对角线标记为1就没有必要再搜索下去了。

## 代码

```cpp
#include <iostream>
#include <queue>
using namespace std;
#define N 1505
struct Pos
{
    int x;
    int y;
    int step;
};
char mp[N][N];
int vis1[N][N],vis2[N][N];
int bfs(int st_x,int st_y,int ed_x,int ed_y)
{
    queue <Pos> q;
    vis1[st_x][st_y]=1,vis2[st_x][st_y]=1;
    q.push((Pos){st_x,st_y,0});
    int cnt;
    while(!q.empty())
    {
        Pos now=q.front();
        q.pop();
        if(now.x==ed_x&&now.y==ed_y) return now.step;
        cnt=1;
        while(mp[now.x-cnt][now.y-cnt]=='.'&&!vis1[now.x-cnt][now.y-cnt])
        {
            vis1[now.x-cnt][now.y-cnt]=1;
            q.push((Pos){now.x-cnt,now.y-cnt,now.step+1});
            cnt++;
        }
        cnt=1;
        while(mp[now.x-cnt][now.y+cnt]=='.'&&!vis2[now.x-cnt][now.y+cnt])
        {
            vis2[now.x-cnt][now.y+cnt]=1;
            q.push((Pos){now.x-cnt,now.y+cnt,now.step+1});
            cnt++;
        }
        cnt=1;
        while(mp[now.x+cnt][now.y-cnt]=='.'&&!vis2[now.x+cnt][now.y-cnt])
        {
            vis2[now.x+cnt][now.y-cnt]=1;
            q.push((Pos){now.x+cnt,now.y-cnt,now.step+1});
            cnt++;
        }
        cnt=1;
        while(mp[now.x+cnt][now.y+cnt]=='.'&&!vis1[now.x+cnt][now.y+cnt])
        {
            vis1[now.x+cnt][now.y+cnt]=1;
            q.push((Pos){now.x+cnt,now.y+cnt,now.step+1});
            cnt++;
        }
    }
    return -1;
}
int main()
{
    int n;
    cin >> n;
    int st_x,st_y;
    cin >> st_x >> st_y;
    int ed_x,ed_y;
    cin >> ed_x >> ed_y;
    for(int i=1;i<=n;i++) scanf("%s",mp[i]+1);
    int ans=bfs(st_x,st_y,ed_x,ed_y);
    cout << ans << endl;
    return 0;
}
```

## typewriter(https://atcoder.jp/contests/abc246/tasks/abc246_f)

## 思路

这题考查的居然是容斥原理！怎么看都看不出来，官方的题解已经看了，但是还是看不懂为啥就是容斥原理了。等明天打完昆明赛了再看看过的人的代码理解理解吧，先放一下。

## 代码

```cpp
占个坑
```
