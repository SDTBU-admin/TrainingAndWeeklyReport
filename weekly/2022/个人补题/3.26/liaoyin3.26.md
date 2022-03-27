## 补题报告 SDTBU-ACM集训队 20 级 3 月第三场训练赛[比赛链接](https://vjudge.csgrandeur.cn/contest/485267)

- - -

### [C - Collision 2](https://vjudge.csgrandeur.cn/contest/485267#problem/C)

#### 题意

> 给定$n$个人,每个人$i$的位置坐标$(X_i,Y_i)$(每个人的位置坐标不同),然后给定一个长度为$n$由$L和R$组成的字符串$S$,当$S_i=R$时,第$i$个人朝向右,当$S_i=L$时,第$i$个人朝向左.判断是否有人会相撞$?$


#### 思路

> 将每个人的坐标和朝向存入结构体,优先根据$Y$坐标排序,最后循环判断相邻且在同行中当前$i$为$'R'$,$i+1$为$L$即会被撞,否则不会相撞.


#### 代码

``` cpp
    /**
     * @file    : C_-_Collision_2.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-21 周一 19:35:39
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

    int n;
    char temp[MAXN];

    struct node{
        int x;
        int y;
        char op;
    }s[MAXN];

    int cmp(struct node a,struct node b)
    {
        if(a.y==b.y)
            return a.x<b.x;
        return a.y<b.y;
    }


    int main()
    {
        //main函数由此开始
        cin >> n;
        for(int i=1;i<=n;i++)
            cin >> s[i].x >> s[i].y;
        cin >> temp+1;
        for(int i=1;i<=strlen(temp+1);i++)
            s[i].op=temp[i];
        
        sort(s+1,s+n+1,cmp);

        for(int i=1;i<=n-1;i++)
        {
            if(s[i].y==s[i+1].y)
                if(s[i].op=='R'&&s[i+1].op=='L')
                {
                    cout << "Yes" << endl;
                    return 0;
                }
        }
        cout << "No" << endl;
        return 0;
    }

```



- - - 



### [D - Moves on Binary Tree](https://vjudge.csgrandeur.cn/contest/485267#problem/D)

#### 题意

> 有一颗非常大的二叉树有$2^{10^{1000}}-1$结点,$1$是根结点,结点$i$的左儿子结点为$2i$,右儿子结点为$2i+1$,给定它的起始点$X$和$N$次移动,移动过程用一个长度为$N$的字符串$S$表示.
> 当$S_i='U'$时,它可到达它的父结点,当$S_i='L'$时,它可到达它的左儿子结点,当$S_i='R'$时,它可到达它的右儿子结点.
> 问经过$N$次询问后到达的结点位置.


#### 思路

> 由于看似$X$的数很大,此时需要从当期位置经过$'RU'或'LU'$ 返回当前位置,用类似$栈$处理优化操作次数即可,避免使结果超过$long $ $long $范围

#### 代码

``` cpp
    /**
     * @file    : D_-_Moves_on_Binary_Tree.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-21 周一 20:42:44
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

    #define MAXN 1000010
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

    int n;
    ll x;
    char s[MAXN];
    string res;
    //stack<char> sta;


    int main()
    {
        //main函数由此开始
        cin >> n >> x >> s+1;
        //cout << s+1 << endl;
        int len=strlen(s+1);
        for(int i=1;i<=len;i++)
        {
            if(s[i]=='U')
            {
                if(res.size()&&res.back()!='U')
                    res.pop_back();
                else res+=s[i];
            }
            else res+=s[i];
        }

        for(int i=0;i<res.size();i++)
        {
            if(res[i]=='U')
                x/=2;
            else if(res[i]=='L')
                x*=2;
            else if(res[i]=='R')
                x=x*2+1;
        }

        cout << x << endl;
        
        return 0;
    }

```

- - -


### [E - Edge Deletion](https://vjudge.csgrandeur.cn/contest/485267#problem/E)

#### 题意

> 给定一个图有$N$个结点和$M$条边,每条边$i$连接着结点$A_i$和节点$B_i$并且长度为$C_i$.问最多能删除多少边，使得最终的图依然**连通**,并且任意两个点的**最短路之间的距离不变**


#### 思路

> 由于$n \leqslant 300$,考虑采用$floyd最短路$跑一遍,判断一条从$A$出发经过$C$的距离到达$B$的边能否被删除可分为以下两种:
* 直接计算:$dis[a][b]<c||num[a][b]>1$,即$a$到$b$的最短路长度小于$c$或者$a$到$b$的最短路的数量大于1.
* 间接计算:对于每对$a,b$,枚举中间结点$t$,如果$dis[a][b]=dis[a][t]+dis[t][b]$,说明该边是不能删除的.



#### 代码

``` cpp

    /**
     * @file    : E_-_Edge_Deletion.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-22 周二 08:41:03
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

    #define MAXN 310
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

    int n,m;
    struct node{
        int a;
        int b;
        int val;
    }s[MAXN*MAXN];
    int dis[MAXN][MAXN],num[MAXN][MAXN];


    int main()
    {
        //main函数由此开始
        cin >> n >> m;
        memset(dis,0x3f,sizeof(dis));
        for(int i=1;i<=m;i++)
        {
            cin >> s[i].a >> s[i].b >> s[i].val;
            dis[s[i].a][s[i].b]=dis[s[i].b][s[i].a]=s[i].val;
            num[s[i].a][s[i].b]=num[s[i].b][s[i].a]=1;
        }

        for(int k=1;k<=n;k++)
            for(int i=1;i<=n;i++)
                for(int j=1;j<=n;j++)
                    if(dis[i][j]>dis[i][k]+dis[k][j])
                    {
                        dis[i][j]=dis[i][k]+dis[k][j];
                        num[i][j]=num[i][k]*num[k][j];
                    }
                    else if(dis[i][j]==dis[i][k]+dis[k][j])
                        num[i][j]+=num[i][k]*num[k][j];
        int res=0;
        for(int i=1;i<=m;i++)
            if(dis[s[i].a][s[i].b]<s[i].val||num[s[i].a][s[i].b]>1)
                res++;

        cout << res << endl;
        
        return 0;
    }

```

- - -


### [F - Lottery](https://vjudge.csgrandeur.cn/contest/485267#problem/F)

#### 题意

> 给定$n$个物品,数量无限多,每个物品获得的可能性为$p_i=\frac{W_i}{\sum_{j=1}^{n}W_j}$,每个物品获得的概率是独立的，现在拿$K$次,问拿出$M$件不同的物品概率是多少,结果对$998244353$求余.


#### 思路

> $数学概率+dp+组合数学$
> 设每个物品拿出来$c_i$个,总共拿了$k$次,此时的概率为$p_{1}^{c_1} \times p_{2}^{c_2} \times ... \times p_{n}^{c_n} \times \frac{k!}{c_1!\times c_2! \times ...\times c_n!}$
令$f(i,j,k)$为考虑前$i$个物品,现在拿出$j$个不同的物品,总共拿了$k$个物品,状态转移方程为:
> $f(i+1,j+c_i!=0,k+c)=f(i+1,j+c_i!=0,k+c)+f(i,j,k)\times \frac{p_{i}^{c_i}}{c_i!}$.
> 结果为$f(n,m,k)$

#### 代码

``` cpp

    /**
     * @file    : F_-_Lottery.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-22 周二 14:25:15
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

    #define MAXN 55
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

    int n,m,k;
    ll fact[MAXN],infact[MAXN];
    ll dp[MAXN][MAXN][MAXN];
    ll sum,val[MAXN],p[MAXN],pw[MAXN][MAXN];


    ll fast_pow(ll a,ll b,int p){//快速幂
        ll res=1;
        while(b){
            if(b%2==1)//b&1
                res=(ll)res*a%p;
            a=(ll)a*a%p;
            b/=2;}//b>>=1;
        return res;
    }



    void init()
    {
        fact[0]=infact[0]=1;
        for(int i=1;i<MAXN;i++)
        {
            fact[i]=(ll)fact[i-1]*i%mod_998;
            infact[i]=(ll)infact[i-1]*fast_pow(i,mod_998-2,mod_998)%mod_998;
            cout << fact[i] << " " << infact[i] <<endl;
        }
    }


    int main()
    {
        //main函数由此开始
        init();
        
        cin >> n >> m >> k;
        for(int i=1;i<=n;i++)
        {
            cin >> val[i];
            sum+=val[i];
        }

        //cout<< sum << endl;
        ll inv=fast_pow(sum,mod_998-2,mod_998)%mod_998;
        //cout << inv << endl;
        for(int i=1;i<=n;i++)
        {
            p[i]=val[i]*inv%mod_998;
            pw[i][0]=1;
            for(int j=1;j<MAXN;j++)
                pw[i][j]=pw[i][j-1]*p[i]%mod_998;
        }

        dp[0][0][0]=1;

        for(int i=1;i<=n;i++)
        {
            for(int j=0;j<=m;j++)
                for(int k_=0;k_<=k;k_++)
                    for(int c=0;c<=k_;c++)
                        if(c==0)
                        {
                            dp[i][j][k_]+=dp[i-1][j][k_]*pw[i][c]%mod_998*infact[c]%mod_998;
                            dp[i][j][k_]%=mod_998;
                        }
                        else if(j>=1)
                        {
                            dp[i][j][k_]+=dp[i-1][j-1][k_-c]*pw[i][c]%mod_998*infact[c]%mod_998;
                            dp[i][j][k_]%=mod_998;
                        }
        }
        //cout << dp[n][m][k] << endl;
        ll res=dp[n][m][k]*fact[k]%mod_998;
        cout << res << endl;

        return 0;
    }

```

- - -


### [G - Sqrt](https://vjudge.csgrandeur.cn/contest/485267#problem/G)

#### 题意

> 给定$T$次询问,每次询问给定一个初始值$X$,然后一个长度初始为1的序列$A$,它可经过$10^{100}$次操作,每次操作选择序列$A$中的最后一个元素$Y$,然后选择一个在$[1,\sqrt{Y}]$的整数接在序列$A$的最后.
> 问最后可形成多少种不同的序列.


#### 思路

> $记忆化搜索$,令$dp(i)$为以当前数字$i$结尾形成的序列个数.
> 因为区间$[t,t^2+2t]$开根号结果相等(为撒右端点是$t^2+2t$?因为$(t+1)^2=t^2+2t+1$与$t^2+2t$相差$1$),故可根据以根号为间距,进行计数.
> 记当前根号区间为$[l,r]$,当前数为$x$,$dp[x]=dp[x]+(r-l+1)dp[l]$. 
> 答案就为$dp[n]$,利用$map$存储.


#### 代码

``` cpp

    /**
     * @file    : G_-_Sqrt.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-22 周二 21:07:17
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

    // #define MAXN 
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
    //             res=(ll)res*a%p;
    //         a=(ll)a*a%p;
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

    int t;
    ll x;
    map<ll,ll> dp;

    ll solve(ll n)
    {
        if(n==1)
            return 1;
        ll sq=sqrt((long double)n);
        if(dp.count(n))
            return dp[n];
        ll res=0;
        for(ll l=1,r;l<=sq;l=r+1)
        {
            ll temp=sqrt((long double)l);
            r=min(sq,temp*temp+2*temp);
            res+=(r-l+1)*solve(l);
        }
        return dp[n]=res;
    }


    int main()
    {
        //main函数由此开始
        cin >> t;
        while(t--)
        {
            cin >> x;
            cout << solve(x) << endl;
        }
        
        return 0;
    }

```


- - -
### [H - Builder Takahashi (Enhanced version)](https://vjudge.csgrandeur.cn/contest/485267#problem/H)

#### 题意

> 鸽了


#### 思路

> 鸽了


#### 代码

``` cpp


    罢工了(增强版不写也罢)



```

- - -


