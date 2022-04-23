### [A - Move Right](https://vjudge.csgrandeur.cn/contest/489306#problem/A)

#### 题意

> 给定长度为$4$的$01$字符串,让整体往右移一次,最右边的元素将会消失,同时前补$0$.

#### 思路

> <b><font color=Red>模拟</font></b>,结果为$'0'+substr(0,3)$.

#### 代码

```cpp
/**
  * @file    : A_-_Move_Right.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-16 周六 16:01:19
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

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    string s;
    cin >> s;
    cout << "0"+s.substr(0,3) << endl;
    return 0;
}
```

- - -

### [B - Unique Nicknames](https://vjudge.csgrandeur.cn/contest/489306#problem/B)

#### 题意

> 给定$n$个人,每个人都有一个姓氏$s_i$和名字$t_i$.
> 现在考虑给这$n$个人取昵称,第$i$个人的昵称为$a_i$,每个$a_i$应满足以下条件:
> * $a_i$可以从第$i$个人的姓氏$s_i$和名字$t_i$中选择;
> * $a_i$不能成为别人的姓氏$s_j$和名字$t_i$,即$i \neq j,a_i \neq s_j$并且$a_i \neq t_j$.
> 
> 问是否存在这样的一组满足以上条件的名字.

#### 思路

> 由于数据范围较小,可<b><font color=Red>暴力枚举</font></b>,保证名字与名字之间不能相同,同时姓氏与名字之间也不相同.


#### 代码

```cpp
/**
  * @file    : B_-_Unique_Nicknames.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-16 周六 16:04:33
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

#define MAXN 110

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
string s1[MAXN],s2[MAXN];

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> n;
    for(int i=1;i<=n;i++)
        cin >> s1[i] >> s2[i];
    
    for(int i=1;i<=n;i++)
        for(int j=i+1;j<=n;j++)
            if(s2[i]==s1[j]||s1[i]==s2[j]||s2[i]==s2[j])
            {
                cout << "No" << endl;
                return 0;
            }
    cout << "Yes" << endl;
    return 0;
}
```

- - -

### [C - 1 2 1 3 1 2 1](https://vjudge.csgrandeur.cn/contest/489306#problem/C)

#### 题意

> 给定序列$S_n$如下:
> * 序列$S_1$只存在单个元素$1$;
> * 序列$S_n(n\geqslant2)$包含序列$S_{n-1} , n ,S_{n-1}$.
> 
> 例如序列$S_2$为$1,2,1$,序列$S_3$为$1,2,1,3,1,2,1$;
> 给定一个$n$,打印出序列$S_n$.

#### 思路

> <b><font color=Red>递归打印</font></b>.

#### 代码

```cpp
/**
  * @file    : C_-_1_2_1_3_1_2_1.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-16 周六 16:16:05
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

int n;

void dfs(int n)
{
    if(n==1)
    {
        cout << 1 << ' ';
        return ;
    }
    dfs(n-1);
    cout << n << ' ' ;
    dfs(n-1);   
}

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> n;
    dfs(n);
    return 0;
}
```

- - -


### [D - Cylinder](https://vjudge.csgrandeur.cn/contest/489306#problem/D)

#### 题意

> 给你一个水平圆柱(容器),给你$Q$次询问,每次操作有以下两种情况:
> * $1$ $x$ $c$:向容器的右端插入$c$个价值为$x$的球;
> * $2$ $c$: 从容器的左端取出$c$个球,并输出拿出的这$c$个球的价值和.


#### 思路

> <b><font color=Red>双端队列模拟</font></b>.如果操作是$1$,将$x$和$c$放入双端队列的尾部,如果操作是$2$,将头部弹出直至$c$为$0$,去判断取出的个数$c$与头部元素的$c'$的关系.
> 注意开$long\ long$.


#### 代码

```cpp
/**
  * @file    : D_-_Cylinder.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-16 周六 16:26:56
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
typedef pair<ll,ll> PLL;

const int inf=0x3f3f3f3f;
const double dinf=0x3f3f3f3f;
const ll llinf=0x3f3f3f3f3f3f3f3f;
const double PI=acos(-1.0);
const double eps=1e-6;
const int mod_1e9 = 1000000007;
const int mod_998 = 998244353;

int n;
deque<PLL> dq;

int main()
{
    //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    //main函数由此开始
    cin >> n;
    for(int i=1;i<=n;i++)
    {
        int op;
        cin >> op;
        if(op==1)
        {
            ll num,cnt;
            cin >> num >> cnt;
            dq.push_back({num,cnt});
        }
        else if(op==2)
        {
            ll cnt;
            cin >> cnt;
            ll res=0;
            while(dq.size()>0&&cnt>0)
            {
                PLL k=dq.front();
                dq.pop_front();
                if(k.second<=cnt)
                {
                    res+=k.first*k.second;
                    cnt-=k.second;
                }
                else
                {
                    res+=cnt*k.first;
                    dq.push_front({k.first,k.second-cnt});
                    cnt=0;
                }
            }
            cout << res<< endl;
        }
    }
    return 0;
}
```

- - -

### [E - Max Min](https://vjudge.csgrandeur.cn/contest/489306#problem/E)

#### 题意

> 给定长度为$N$的序列$A$和两个整数$X,Y$,找出区间对$[L,R]$满足以下条件的对数:
> * $1 \leqslant L \leqslant R \leqslant N$;
> * $[L,R]$区间内的最大值为$X$,最小值为$Y$.

#### 思路

> <b><font color=Red>尺取</font></b>.对于那部分数$p$($p>X$或者$p<Y$)不在满足的区间$[L,R]$之间,可将其作为分割点,将可行的区间划分出来.例如:$A=(4,2,5,4,3,4,2),X=4,Y=2$,可划分为$(4,2),(4,3,4,2)$两大块,寻找既包含$X$和$Y$的区间数.


#### 代码

```cpp
/**
  * @file    : E_-_Max_Min.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-16 周六 16:47:24
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
// #define MAXM 
// #define MAXK 

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

int n,x,y;
int a[MAXN],idx=1;
vector<int> v[MAXN];


int main()
{
    //ios::sync_with_stdio(false);
    //main函数由此开始
    cin >> n >> x >> y;
    for(int i=1;i<=n;i++)
    	cin >> a[i];
    for(int i=1;i<=n;i++)
    {
    	if(a[i]<y||a[i]>x)
		{
			if(v[idx].size()>0)
				idx++;
		}
		else v[idx].push_back(a[i]);
    }

	ll res=0;
	for(int i=1;i<=idx;i++)
	{
		map<int,int> mp;
		for(int j=0,k=0;j<v[i].size();j++)
		{
			while((mp[x]==0||mp[y]==0)&&k<v[i].size())
			{
				mp[v[i][k]]++;
				k++;
			}
			if(mp[x]!=0&&mp[y]!=0)
				res=res+v[i].size()-k+1;
			mp[v[i][j]]--;
		}
	}
	cout << res << endl;
    return 0;
}
```

- - -

### [F - Cards](https://vjudge.csgrandeur.cn/contest/489306#problem/F)

#### 题意

> 给你$n$张卡片,每张卡片的正反面都有一个数字$p_i$和$q_i$,正反面各构成$1-n$的排列(两个排列可能相同),从这$n$张卡片中选出若干张,满足选出的卡片可以表示$1-n$的所有数字,问满足条件的方案数,结果对$998244353$求余.

#### 思路

> <b><font color=Red>置换群(环+并查集)+dp</font></b>.
> 将每张卡片看作是由$p_i$指向$q_i$的有向边,由于$p,q$都是一个排列,意味着图中的每个点的入度为$1$,出度为$1$,形成(置换群)环.转换成给定若干个不相连通的环,其中每个环,可以删除若干条边,满足每一个点都和一条边相连,即每两条相邻的边需要至少选一条,问满足的方案数.
> 判断环可以用<b><font color=Green>并查集</font></b>,并维护每个环的$size$.
> 每个环的方案数考虑使用<b><font color=Orange>dp预处理</font></b>,结果为这$k$个环的贡献$G_i$的乘积,即$\prod_{i=1}^k(G_i)$.
> 设$dp[i][0/1][0/1]$为考虑环里有$i$个点,第一个点是否选,最后一个点是否选的方案数,第一个点选有两种情况$s(s=0/1)$,讨论最后一个点$i$的选择情况:
> * 最后一个点$i$不选时由$i-1$个点**选**推出,$dp[i][s][0]=dp[i-1][s][1];$
> * 最后一个点$i$选时,由$i-1$个点**选**与**不选**两种情况推出,$dp[i][s][1]=dp[i-1][s][0]+dp[i-1][s][1]$.
> 
> 此时的$G_i=dp[i][1][1]+dp[i][0][1]+dp[i][1][0]$.

#### 代码

```cpp
/**
  * @file    : F_-_Cards.cpp
  * @author  : SDTBU_LY
  * @version : V1.0.0
  * @date    : 2022-04-22 周五 19:38:57
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

int n,p[MAXN],q[MAXN],fa[MAXN],siz[MAXN];
int dp[MAXN][2][2],res[MAXN];

void init()
{
    for(int i=1;i<=n;i++)
        fa[i]=i,siz[i]=1;
    dp[1][0][0]=dp[1][1][1]=res[1]=1;
    for(int i=2;i<=n;i++)
    {
        for(int j=0;j<=1;j++)
        {
            dp[i][j][0]=dp[i-1][j][1]%mod_998;
            dp[i][j][1]=((ll)dp[i-1][j][0]+dp[i-1][j][1])%mod_998;
        }
        res[i]=((ll)dp[i][0][1]+dp[i][1][0]+dp[i][1][1])%mod_998;
    }   
}

int find(int x)
{
    if(fa[x]!=x)
        fa[x]=find(fa[x]);
    return fa[x];
}

void join(int x,int y)
{
    int fx=find(x),fy=find(y);
    if(fx!=fy)
    {
        fa[fx]=fy;
        siz[fy]+=siz[fx];
    }
}

int main()
{
    //ios::sync_with_stdio(false);
    //main函数由此开始
    cin >> n;
    init();
    for(int i=1;i<=n;i++)
        cin >> p[i];
    for(int i=1;i<=n;i++)
        cin >> q[i];
    for(int i=1;i<=n;i++)
        join(p[i],q[i]);
    ll ans=1;
    for(int i=1;i<=n;i++)
        if(fa[i]==i)
            ans=((ll)ans%mod_998*res[siz[i]])%mod_998;
    cout << ans << endl;
    return 0;
}
```
