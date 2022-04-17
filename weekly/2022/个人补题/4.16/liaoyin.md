### [A - Four Points](https://vjudge.csgrandeur.cn/contest/488484#problem/A)

#### 题意

> 给定一个长方形的三条顶点的坐标$(x_i,y_i)$,输出另外的一个顶点的坐标,其中长方形的每条边都平行于$x$轴或者平行于$y$轴且长方形的面积不为$0$.

#### 思路

> <b><font color=Red>思维题</font></b>.
> 由于长方形的每条边要么平行于$x$轴,要么平行于$y$轴,即只需要找出<b><font color=Orange>横纵坐标出现一次的数</font></b>即为答案;
> 此处也可直接对三个点的横纵坐标分别进行<b><font color=Green>异或操作</font></b>,即为答案.


#### 代码

```cpp
/**
  * @file    : A_-_Four_Points.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-10 周日 16:00:15
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstdio>
#include<iomanip>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<cstdlib>
#include<queue>
#include<stack>
#include<set>
// #include<unordered_set>
#include<map>
// #include<unordered_map>
#include<vector>

using namespace std;

typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
typedef pair<double,double> PDD;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

map<int,int> mp1;
map<int,int> mp2;
int x[4],y[4];

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    for(int i=1;i<=3;i++)
    {
         cin >> x[i] >> y[i];
         mp1[x[i]]++;
         mp2[y[i]]++;
    }
    for(auto i:mp1)
        if(i.second==1)
            cout << i.first << " ";
    for(auto i:mp2)
        if(i.second==1)
            cout << i.first << endl;
    //cout << (x[1]^x[2]^x[3]) << ' ' << (y[1]^y[2]^y[3]) << endl;
    return 0;
}
```



- - -

### [B - Get Closer](https://vjudge.csgrandeur.cn/contest/488484#problem/B)

#### 题意

> 从二维平面上的点$(0,0)$出发,向点$(A,B)$方向移动距离$1$,求移动后的坐标为多少.

#### 思路

> <b><font color=Red>勾股定理+相似三角形</font></b>.
> 首先求出$(0,0)$到$(A,B)$的距离$d=\sqrt{A^2+B^2}$,然后所求的坐标为$(\frac{A}{d},\frac{B}{d})$,也可近似看成求$sinx$和$cosx$(因为$sin^2x+cos^2x=1$).


#### 代码

```cpp
/**
  * @file    : B_-_Get_Closer.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-10 周日 16:03:22
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstdio>
#include<iomanip>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<cstdlib>
#include<queue>
#include<stack>
#include<set>
// #include<unordered_set>
#include<map>
// #include<unordered_map>
#include<vector>

using namespace std;

typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
typedef pair<double,double> PDD;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

int a,b;

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> a >> b;
    double temp=sqrt(a*a+b*b);
    printf("%.12f %.12f",a/temp,b/temp);
    return 0;
}
```

- - -


### [C - Coupon](https://vjudge.csgrandeur.cn/contest/488484#problem/C)

#### 题意

> 给定$N$个物品,每个物品$i$的价格为$A_i$,$Takahashi$有$K$张优惠券(可优惠$X$).每张优惠券可用于这$N$个物品,每个物品可用任意小于等于$K$数量的优惠券,如果使用了$k$张,你只需要花费$max(A_i-kX,0)$.
> 问最小花费为多少.

#### 思路

> <b><font color=Red>贪心</font></b>.
> 要想使花费最小,就要尽可能多得使用优惠券,如果优惠券能减去商品价格大于$X$的话,就贪心地使用优惠券,如果所有商品剩下的价格都小于$X$元后还有剩余优惠券,就贪心选择剩下物品中价格更高的.


#### 代码

```cpp
/**
  * @file    : C_-_Coupon.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-10 周日 16:43:42
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstdio>
#include<iomanip>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<cstdlib>
#include<queue>
#include<stack>
#include<set>
// #include<unordered_set>
#include<map>
// #include<unordered_map>
#include<vector>

#define MAXN 200010

using namespace std;

typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
typedef pair<double,double> PDD;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

int n;
ll k,x,a[MAXN],res;
map<ll,ll> mp;

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> n >> k >> x;
    for(int i=1;i<=n;i++)
    {
        cin >> a[i];
        ll temp=min(a[i]/x,k);
        k-=temp,a[i]-=temp*x;
        res+=a[i];
        mp[a[i]]++;
    }
    for(auto i=mp.rbegin();i!=mp.rend();i++)
    {
        ll temp=min(i->second,k);
        k-=temp,res-=i->first*temp;
        if(k==0)
            break;
    }
    cout << res << endl;
    return 0;
}
```

- - -

### [D - 2-variable Function](https://vjudge.csgrandeur.cn/contest/488484#problem/D)

#### 题意

> 给你一个整数$N$,其中$1 \leqslant N \leqslant 10^{18}$,找一个最小的整数$X$满足以下条件:
> * $X$是大于等于$N$的整数;
> * 存在一组非负整数对$(a,b)$,使得$X=a^3+a^2b+ab^2+b^3$.

#### 思路

>  <b><font color=Red>枚举+二分</font></b>.
> 由于数据较大,暴力枚举必不现实,考虑从整数对$(a,b)$出发,鉴于$N$的范围是$N \leqslant 10^{18}$,可推出$a,b$的范围为$0-10^6$,跑两重循环也会超时,故考虑先考虑$a$,然后二分找出合适$b$的值使之$X \geqslant N$成立,时间复杂度为$O(10^6 \times log(10^6))$.

#### 代码

```cpp
/**
 * @file    : D_-_2_-variable_Function.cpp
 * @author  : SDTBU_LY
 * @version : V1.0.0
 * @date    : 2022-04-10 周日 16:09:47
 * @上联    : ac自动机fail树上dfs序建可持久化线段树
 * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstdio>
#include<iomanip>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<cstdlib>
#include<queue>
#include<stack>
#include<set>
// #include<unordered_set>
#include<map>
// #include<unordered_map>
#include<vector>

#define MAXN 1000000

using namespace std;

typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
typedef pair<double,double> PDD;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

ll n,res=llinf;

ll f(ll a,ll b)//防止中间相乘爆int
{
    return a*a*a+a*a*b+a*b*b+b*b*b;
}

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> n;
    for(ll i=0;i<=MAXN;i++)
    {
        if(i*i*i>n)
            continue;
        ll l=0,r=MAXN;
        while(l<r)
        {
            ll mid=(l+r)/2;
            if(f(i,mid)<n)
                l=mid+1;
            else r=mid;
        }
        ll j=l;
        res=min(res,f(i,j));
    }
    cout << res << endl;
    return 0;
}
```


- - -


### [E - Bishop 2](https://vjudge.csgrandeur.cn/contest/488484#problem/E)

#### 题意

> 给定一个$N \times N$的棋盘.每个位置$(i,j)$都有对应的值$S_{i,j}$.
> * 如果$S_{i,j}=$`.`,说明该位置为空;
> * 如果$S_{i,j}=$`#`,说明该位置存在障碍物.
> 给定一个象棋的起始点坐标$(A_x,A_y)$和象棋规则(斜着走,即四个方向`{{1,1},{1,-1},{-1,1},{-1,-1}}`移动,如果当前方向没有障碍物可以一直往这个方向前行不花费任何代价),问能否找到最少的移动步数到达$(B_x,B_y)$,如果不能到达,输出$-1$.


#### 思路

> <b><font color=Red>bfs</font></b>,~~这道题有可能卡bfs时间,我就中招了,是真的离谱~~.
>> * **最初的想法**:每个状态扩展的时候如果遇到之前被扩展过的结点,如果这个结点已经在$BFS$过程中<b><font color=Purple>已经出队</font></b>了,那么说明这个点被扩展过了,我们就不需要再扩展了,如果这个点被扩展过但是还<b><font color=DeepSkyBlue>没有出队</font></b>,那么我们需要跳过这个结点继续往下扩展,因为后面的结点可能没有被扩展过.最后发现有组数据极限的过不去($tle$).
>> * 转变思路: <b><font color=Orange>只通过更新dis距离</font></b>,而<u>不借助标记数组</u>就能过,~~目前我还是不知道原来的码错在哪~~.o((⊙﹏⊙))o

#### 代码

```cpp
/**
  * @file    : E_-_Bishop_2.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-11 周一 15:00:03
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstdio>
#include<iomanip>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<cstdlib>
#include<queue>
#include<stack>
#include<set>
// #include<unordered_set>
#include<map>
// #include<unordered_map>
#include<vector>

#define MAXN 1510

using namespace std;

typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
typedef pair<double,double> PDD;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

int n,sx,sy,ex,ey;
char g[MAXN][MAXN];
int dis[MAXN][MAXN];

int dir[4][2]={{1,1},{1,-1},{-1,1},{-1,-1}};

int bfs()
{
    //memset(dis,0x3f,sizeof(dis));
    queue<PII> q;
    q.push({sx,sy});
    dis[sx][sy]=0;
    while(q.size()>0)
    {
        PII temp=q.front();
        q.pop();
        if(temp.first==ex&&temp.second==ey)
            return dis[ex][ey];
        for(int i=0;i<=3;i++)
        {
            for(int k=1;;k++)
            {
                int dx=temp.first+k*dir[i][0],dy=temp.second+k*dir[i][1];
                if(dx>=1&&dx<=n&&dy>=1&&dy<=n&&g[dx][dy]!='#'&&dis[dx][dy]>=dis[temp.first][temp.second]+1)
                {
                    q.push({dx,dy});
                    dis[dx][dy]=dis[temp.first][temp.second]+1;
                }
                else break;
            }
        }
    }
    return -1;
}

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    scanf("%d %d %d %d %d",&n,&sx,&sy,&ex,&ey);
    for(int i=1;i<=n;i++)
    {
        scanf("%s",g[i]+1);
        //cout << g[i]+1 << endl;
        for(int j=1;j<=n;j++)
            dis[i][j]=inf;
    }
    printf("%d\n",bfs());
    return 0;
}
```

- - -

### [F - typewriter](https://vjudge.csgrandeur.cn/contest/488484#problem/F)

#### 题意

> 给定$N$个字符串集,每个字符串集的字符有小写字母($a,b,c,...x,y,z$组成),可以进行以下操作:
> * 选定第$i$个字符串集;
> * 选用其中的字符输出长度为$L$的字符串.
> 问最多能产生多少不同的字符串$L$,结果对$998244353$求余.


#### 思路

> <b><font color=Red>容斥原理+dfs(<font color=Blue>或者状态压缩,由于作者懒就不写状态压缩了</font>)+快速幂</font></b>.
> 根据容斥原理可得,设$A_i$为使用字符串集$s_i$打出长度为$L$的所有字符串的集合.容斥原理公式如下:
> ${\color{Red}\left| A_1 \cup A_2 \cup ... \cup A_n \right|=+(\left| A_1 \right|+\left| A_2 \right|+...+\left| A_n \right|)}$
> ${\color{Red}-(\left| A_1 \cap A_2 \right|+\left| A_1 \cap A_3 \right|+...+\left| A_{n-1} \cap A_n \right|)}$
> ${\color{Red}+(\left| A_1 \cap A_2 \cap A_3 \right|+\left| A_1 \cap A_2 \cap A_4 \right|+...+\left| A_{n-2} \cap A_{n-1} \cap A_n \right|)}$
> ${\color{Red}-...}$
> ${\color{Red}+...}$
> ${\color{Red}+(-1)^{k-1}\left| A_1 \cap A_2 \cap ... \cap A_n\right|}$(其中$k$为交集的个数)
> * 答案为${\color{Red}\left| A_1 \cup A_2 \cup ... \cup A_n \right|}$

* 通过枚举以上情况时间复杂度为$O(2^n)$,$N$个字母能打印的长度为$L$的字符串的个数为$N^L$,使用快速幂计算花费$O(logL)$的时间,总的时间复杂度为$O(2^nlogL)$.

#### 代码

```cpp
/**
  * @file    : F_-_typewriter.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-12 周二 11:50:53
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstdio>
#include<iomanip>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<cstdlib>
#include<queue>
#include<stack>
#include<set>
// #include<unordered_set>
#include<map>
// #include<unordered_map>
#include<vector>

#define MAXN 30

using namespace std;

typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
typedef pair<double,double> PDD;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

ll n,l,res;
int temp[MAXN],vis[MAXN][MAXN];

ll fast_pow(ll a,ll b,int p){//快速幂
    ll res=1;
    while(b){
        if(b%2==1)//b&1
            res=(ll)res*a%p;
        a=(ll)a*a%p;
        b/=2;}//b>>=1;
    return res;
}

void dfs(int u,int cnt)
{
    if(u==n)
    {
        if(cnt==0)
            return ;
        ll count=0;
        for(int i=0;i<=25;i++)
            if(temp[i]==1)
                count++;
        if(cnt%2==1)
            res=(res+fast_pow(count,l,mod_998))%mod_998;
        else res=(res-fast_pow(count,l,mod_998)+mod_998)%mod_998;
        return ;
    }
    int backup[MAXN];
    memcpy(backup,temp,sizeof(backup));
    for(int i=0;i<=25;i++)
        temp[i]&=vis[u][i];
    dfs(u+1,cnt+1);
    memcpy(temp,backup,sizeof(temp));
    dfs(u+1,cnt);
}

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> n >> l;
    for(int i=0;i<=n-1;i++)
    {
        string s;
        cin >> s;
        for(int j=0;j<s.size();j++)
            vis[i][s[j]-'a']=1;
    }
    for(int i=0;i<=25;i++)
        temp[i]=1;
    dfs(0,0);
    cout << res << endl;
    return 0;
}
```


- - -

### [G - Game on Tree 3](https://vjudge.csgrandeur.cn/contest/488484#problem/G)

#### 题意

> 给定一个总共有$n$个结点同时以$1$为根结点的树,除根节点外的每一个顶点$i$都有对应的权值$A_i$.初始有个纸片放在1号根结点位置,每轮$Aoki$(先手)可以将任意一点的权值变为$0$,$Takahashi$(后手)可以将纸片移动到一个相邻的结点,并且可以随时结束游戏.后手如果到达叶子结点自行结束游戏.游戏的得分是纸片最后在的位置上的点权,后手的目标是<b><font color=Pink>最大化</font></b>点权,先手的目标是<b><font color=Chocolate>最小化</font></b>点权.求双方采用最优策略能得到的最大得分.


#### 思路

> <b><font color=Red>二分+树上dp</font></b>.
> 要求最大得分可通过二分把问题转化为判别的问题.二分后问题就变成了判断是否能够取到大于等于$mid$的得分.由于采用最优策略必然不会往回走,可采用树形dp解决.
> 后手每往下走一步,那么先手就能把这个结点子树中某个结点变为$0$,考虑$f(u)$为纸片在当前结点时能走到的权值大于$mid$的结点的数目,显然$f(u)$等于其所有子树的$f$的贡献加上其本身的贡献,由于每往下走一步对手会删除一个点,结果需$-1$,最终判断是否成立取决于$f(1)>0$.


#### 代码

```cpp
/**
  * @file    : G_-_Game_on_Tree_3.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-12 周二 15:14:20
  * @上联    : ac自动机fail树上dfs序建可持久化线段树
  * @下联    : 后缀自动机的next指针DAG图上求SG函数
**/

#include<iostream>
#include<cstdio>
#include<iomanip>
#include<cstring>
#include<algorithm>
#include<cmath>
#include<cstdlib>
#include<queue>
#include<stack>
#include<set>
// #include<unordered_set>
#include<map>
// #include<unordered_map>
#include<vector>

#define MAXN 200010

using namespace std;

typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
typedef pair<double,double> PDD;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

int n,w[MAXN],f[MAXN];
vector<int> v[MAXN];

void dfs(int root,int fa,int val)
{
    f[root]=0;
    for(auto i:v[root])
        if(i!=fa)
        {
            dfs(i,root,val);
            f[root]+=f[i];
        }
    f[root]=max(f[root]-1,0);
    if(w[root]>=val)
        f[root]++;
}

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> n;
    for(int i=2;i<=n;i++)
        cin >> w[i];
    for(int i=1;i<=n-1;i++)
    {
        int a,b;
        cin >> a >> b;
        v[a].push_back(b);
        v[b].push_back(a);
    }
    int l=0,r=1e9;
    while(l<r)
    {
        int mid=(l+r+1)/2;
        dfs(1,-1,mid);
        if(f[1]!=0)
            l=mid;
        else r=mid-1;
    }
    cout << r << endl;
    return 0;
}
```
