## 补题报告 SDTBU-ACM集训队 20 级3月训练赛Ⅰ[比赛链接](https://vjudge.csgrandeur.cn/contest/483122)

- - -

### [D - equeue](https://vjudge.csgrandeur.cn/contest/483122#problem/D)

#### 题意

给定一个双端队列$D$,其中包含有$N$个数$V_1,V_2,...,V_N$,现有$4$种操作方式：
> $1.$ 拿走双端队列最左边的元素，放入手中;
> $2.$ 拿走双端队列最右边的元素，放入手中;
> $3.$ 选择手中的一个元素放回双端队列的最左边(前提手中元素不为空) ;
> $4.$ 选择手中的一个元素放回双端队列的最右边(前提手中元素不为空) .

你可以从以上四种操作方式进行不超过$K$次操作，问最后你手中的元素和的最大值.

#### 思路

> 由于范围较小$1\leqslant  N \leqslant 50$,可暴力枚举所有可能区间$[1,l]$和$[r,n]$(注意考虑只选择最左边和最右边两种特殊情况),将两个区间的数保存排序,最后考虑剩下的$ k-当前的区间的数的个数$ 是否考虑被还原回去即可,满足条件的取$max$.

#### 代码

    /**
    * @file    : D_-_equeue.cpp
    * @author  : SDTBU_LY
    * @version : V1.0.0
    * @date    : 2022-03-09 周三 16:47:23
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

    #define MAXN 60
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
    const int MOD_998 = 998244353;

    // int dir[4][2]={{-1,0},{0,1},{1,0},{0,-1}};

    /*
    // int head[MAXN],ed[MAXM],nex[MAXM],val[MAXM],idx;

    // void add(int a,int b,int c){//建图
    //     ed[idx]=b,nex[idx]=head[a],val[idx]=c,head[a]=idx++;
    // }

    // int gcd(int a,int b){//最大公约数
    //     return b?gcd(b,a%b):a;
    // }

    // int lcm(int a,int b){//最小公倍数
    //     return a/gcd(a,b)*b;
    // }

    // int fast_pow(int a,int b,int p){//快速幂
    //     int res=1;
    //     while(b){
    //         if(b%2==1)//b&1
    //             res=res*a%p;
    //         a=a*a%p;
    //         b/=2;}//b>>=1;
    //     return res;
    // }

    // int fast_mul(int a,int b,int p){//快速乘
    //     int res=0;
    //     while(b){
    //         if(b%2==1)//b&1
    //             res=(res+a)%p;
    //         a=(a+a)%p;
    //         b/=2;}//b>>=1;
    //     return res;
    // }

    // int primes[MAXN],cnt,vis[MAXN];
    // void get_primes(int n){//欧拉筛
    //     for(int i=2;i<=n;i++){
    //         if(vis[i]==0)
    //             primes[cnt++]=i;
    //         for(int j=0;primes[j]<=n/i;j++){
    //             vis[primes[j]*i]=1;
    //             if(i%primes[j]==0)    break;}
    // }

    // int fa[MAXN];//并查集
    // void init(int n){
    //     for(int i=1;i<=n;i++)    fa[i]=i;}
    // int find(int x){
    //     if(fa[x]!=x)    fa[x]=find(fa[x]);
    //     return fa[x];}
    // void join(int x,int y){
    //     int fx=find(x),fy=find(y);
    //     if(fx!=fy)    fa[fx]=fy;}
    */

    int n,k,v[MAXN],res=-inf;
    int temp[MAXN];

    int main()
    {
        //main函数由此开始
        cin >> n >> k;
        for(int i=1;i<=n;i++)
            cin >> v[i];
        for(int l=0;l<=n;l++)
        {
            for(int r=n+1;r>l;r--)
            {
                int cnt=0;
                for(int i=1;i<=l;i++)
                    temp[++cnt]=v[i];
                for(int i=n;i>=r;i--)
                    temp[++cnt]=v[i];
                if(cnt>k)
                    continue;
                sort(temp+1,temp+cnt+1);
                int sum=0;
                for(int i=cnt;i>=0;i--)
                {
                    if(i<=k-cnt)
                        sum+=max(temp[i],0);
                    else sum+=temp[i];
                }
                res=max(res,sum);
            }
        }
        cout << res << endl;
        
        return 0;
    }

- - -

### [E - Sum Equals Xor ](https://vjudge.csgrandeur.cn/contest/483122#problem/E)

#### 题意

给定一个二进制的整数$L$,其中$ 1 \leqslant L \leqslant 2^{100 001}$,求解满足条件$a + b = a \bigoplus b \leqslant L$的正整数$a,b$的对数,结果对$10^9+7$取模


#### 思路

> $dp$问题,由于异或操作为不进位加法运算,并且
$a + b = a \bigoplus b $,使得对于$a,b$的每一二进制位均不能同时为1(可以是$1,0、0,1、0,0$).

> 令$dp[i][0]$表示$a \bigoplus b $小于$L$的前$i$位的方案数，则$dp[i][1]$表示$a \bigoplus b $等于$L$的前$i$位的方案数.

讨论$L$当前位的两种情况($1或0$):
$1.L$的当前位为$1$.
* 当前位相同的情况时,必然当前位相同是由上一位相同转化而来,同时当前位$a,b$的取值有两种(可以为$1,0、0,1$),故$dp[i][1]=dp[i-1][1] \times 2$;
* 当前位小于的情况时,又可分为两种：上一位小于,此时当前位$a,b$的取值有三种(可以为$1,0、0,1、0,0$);上一位等于,此时当前位$a,b$的取值仅有一种($0,0$),故$dp[i][0]=dp[i-1][0] \times 3 + dp[i-1][1]$.

$2.L$的当前位为$0$.
* 当前位相同的情况,必然当前位相同是由上一位相同转化而来，同时当前位的$a,b$的取值仅有一种($0,0$),故$dp[i][1]=dp[i-1][1]$;
* 当前位小于的情况时,即上一位小于,此时当前位$a,b$的取值有三种(可以为$1,0、0,1、0,0$),故 $dp[i][0]=dp[i-1][0] \times 3$.

> 最后的结果为$dp[len][0]+dp[len][1]$,其中$len$为二进制$L$的长度.
> 注意初始化$dp[0][1]=1$.

#### 代码

    /**
    * @file    : E_-_Sum_Equals_Xor.cpp
    * @author  : SDTBU_LY
    * @version : V1.0.0
    * @date    : 2022-03-09 周三 17:39:41
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

    #define MAXN 100010
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
    const int MOD_998 = 998244353;

    // int dir[4][2]={{-1,0},{0,1},{1,0},{0,-1}};

    /*
    // int head[MAXN],ed[MAXM],nex[MAXM],val[MAXM],idx;

    // void add(int a,int b,int c){//建图
    //     ed[idx]=b,nex[idx]=head[a],val[idx]=c,head[a]=idx++;
    // }

    // int gcd(int a,int b){//最大公约数
    //     return b?gcd(b,a%b):a;
    // }

    // int lcm(int a,int b){//最小公倍数
    //     return a/gcd(a,b)*b;
    // }

    // int fast_pow(int a,int b,int p){//快速幂
    //     int res=1;
    //     while(b){
    //         if(b%2==1)//b&1
    //             res=res*a%p;
    //         a=a*a%p;
    //         b/=2;}//b>>=1;
    //     return res;
    // }

    // int fast_mul(int a,int b,int p){//快速乘
    //     int res=0;
    //     while(b){
    //         if(b%2==1)//b&1
    //             res=(res+a)%p;
    //         a=(a+a)%p;
    //         b/=2;}//b>>=1;
    //     return res;
    // }

    // int primes[MAXN],cnt,vis[MAXN];
    // void get_primes(int n){//欧拉筛
    //     for(int i=2;i<=n;i++){
    //         if(vis[i]==0)
    //             primes[cnt++]=i;
    //         for(int j=0;primes[j]<=n/i;j++){
    //             vis[primes[j]*i]=1;
    //             if(i%primes[j]==0)    break;}
    // }

    // int fa[MAXN];//并查集
    // void init(int n){
    //     for(int i=1;i<=n;i++)    fa[i]=i;}
    // int find(int x){
    //     if(fa[x]!=x)    fa[x]=find(fa[x]);
    //     return fa[x];}
    // void join(int x,int y){
    //     int fx=find(x),fy=find(y);
    //     if(fx!=fy)    fa[fx]=fy;}
    */

    ll dp[MAXN][2];
    char s[MAXN];

    int main()
    {
        //main函数由此开始
        cin >> s+1;
        int len=strlen(s+1);
        dp[0][1]=1;
        for(int i=1;i<=len;i++)
        {
            if(s[i]=='0')
            {
                dp[i][1]=dp[i-1][1]%mod_1e9;
                dp[i][0]=dp[i-1][0]*3%mod_1e9;
            }
            else if(s[i]=='1')
            {
                dp[i][1]=dp[i-1][1]*2%mod_1e9;
                dp[i][0]=(dp[i-1][0]*3%mod_1e9+dp[i-1][1]%mod_1e9)%mod_1e9;
            }
        }
        
        cout << (dp[len][1]+dp[len][0])%mod_1e9 << endl;
        return 0;
    }


- - -


### [F - Roadwork](https://vjudge.csgrandeur.cn/contest/483122#problem/F)

#### 题意

给定一个数轴,在数轴上有$N$个障碍物,它们的位置分别位于$X_i$,它们分别会使得在它们给定时间范围区间$[S_i-0.5,T_i-0.5]$的人停止运动,此处时间范围区间可直接看为$[S_i,T_i)$.
给定$Q$个人他们出发的时间$D_i$,他们出发的地方均为原点$0$处,他们的速度均为$1$并朝着数轴正方向运动,输出他们停下来的位置，如果停不下来输出$-1$.


#### 思路

> 考虑每个障碍物可以阻碍的人,将障碍物的相关信息按照位置$x_i$进行从小到大的排序,利用$stl$中集合set放入每个人的出发时间$D_i$,遍历所有的障碍物,每次二分查找出发时间为$[max(S_i-X_i,0),max(T_i-X_i,0))$,更新答案,删除更新过的人.

#### 代码

    /**
    * @file    : F_-_Roadwork.cpp
    * @author  : SDTBU_LY
    * @version : V1.0.0
    * @date    : 2022-03-09 周三 19:07:40
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
    const int MOD_998 = 998244353;

    // int dir[4][2]={{-1,0},{0,1},{1,0},{0,-1}};

    /*
    // int head[MAXN],ed[MAXM],nex[MAXM],val[MAXM],idx;

    // void add(int a,int b,int c){//建图
    //     ed[idx]=b,nex[idx]=head[a],val[idx]=c,head[a]=idx++;
    // }

    // int gcd(int a,int b){//最大公约数
    //     return b?gcd(b,a%b):a;
    // }

    // int lcm(int a,int b){//最小公倍数
    //     return a/gcd(a,b)*b;
    // }

    // int fast_pow(int a,int b,int p){//快速幂
    //     int res=1;
    //     while(b){
    //         if(b%2==1)//b&1
    //             res=res*a%p;
    //         a=a*a%p;
    //         b/=2;}//b>>=1;
    //     return res;
    // }

    // int fast_mul(int a,int b,int p){//快速乘
    //     int res=0;
    //     while(b){
    //         if(b%2==1)//b&1
    //             res=(res+a)%p;
    //         a=(a+a)%p;
    //         b/=2;}//b>>=1;
    //     return res;
    // }

    // int primes[MAXN],cnt,vis[MAXN];
    // void get_primes(int n){//欧拉筛
    //     for(int i=2;i<=n;i++){
    //         if(vis[i]==0)
    //             primes[cnt++]=i;
    //         for(int j=0;primes[j]<=n/i;j++){
    //             vis[primes[j]*i]=1;
    //             if(i%primes[j]==0)    break;}
    // }

    // int fa[MAXN];//并查集
    // void init(int n){
    //     for(int i=1;i<=n;i++)    fa[i]=i;}
    // int find(int x){
    //     if(fa[x]!=x)    fa[x]=find(fa[x]);
    //     return fa[x];}
    // void join(int x,int y){
    //     int fx=find(x),fy=find(y);
    //     if(fx!=fy)    fa[fx]=fy;}
    */

    int n,q,pos[MAXN],res[MAXN];
    struct node{
        int l;
        int r;
        int x;
    }s[MAXN];

    int d[MAXN];

    set<int> ss;
    map<int,int> mp;

    int cmp(struct node a,struct node b)
    {
        return a.x<b.x;
    }


    int main()
    {
        //main函数由此开始
        memset(res,-1,sizeof(res));
        cin >> n >> q;
        for(int i=1;i<=n;i++)
        {
            cin >> s[i].l >> s[i].r >> s[i].x;
            s[i].l=max(s[i].l-s[i].x,0);
            s[i].r=max(s[i].r-s[i].x,0);
        }

        sort(s+1,s+n+1,cmp);

        for(int i=1;i<=q;i++)
        {
            cin >> d[i];
            mp[d[i]]=i;
            ss.insert(d[i]);
        }

        for(int i=1;i<=n;i++)
        {
            auto l=ss.lower_bound(s[i].l);
            auto r=ss.lower_bound(s[i].r);
            vector<int> v;
            for(auto j=l;j!=r;j++)
            {
                res[mp[*j]]=s[i].x;
                v.push_back(*j);
            }
            for(int j=0;j<v.size();j++)
                ss.erase(v[j]);
        }

        for(int i=1;i<=q;i++)
            cout << res[i] << endl;
        
        return 0;
    }

