## 补题报告 SDTBU-ACM集训队 20 级 3 月第四场训练赛[比赛链接](https://vjudge.csgrandeur.cn/contest/486427)

- - -

### [E - King Bombee](https://vjudge.csgrandeur.cn/contest/486427#problem/E)

#### 题意

> 给定一个有$N$个结点和$M$条边的无向图，结点从$1$~$N$,边从$1$到$M$,每条边$i$连接着结点$U_i$和结点$V_i$.然后给定$4$个整数$K,S,T,X$,$K+1$表示所求序列$A$的长度(下标从$0$开始,$K$结束,其中$A_i$可到达$A_{i+1}$),其中,第$0$个元素为$S$,第$K$个元素为$T$,并且满足在序列$A$中出现$X$的次数为偶数次,问有多少种这样的序列?结果对$998244353$求余.


#### 思路

> $树上dp$,令$dp(i,j,k)$表示到达$i$点,经过$j$步,并且经过$x$的奇偶性(奇数为$1$,偶数为$0$)为$k$的方案数.
> 初始化$dp(s,0,0)=1$,首先遍历走的步数,然后遍历当前点可到达的点,同时考虑途径的点是否经过$x$.

令可从起点经过$i$步并且可从当前点$j$到达的点为$kk$:
* 如果到达的点$kk$是$X$时:
    * 经过点$X$偶数次的状态转移方程为:$dp(kk,i,0)=dp(kk,i,0)+dp(j,i-1,1)$;
    * 经过点$X$奇数次的状态转移方程为:$dp(kk,i,1)=dp(kk,i,1)+dp(j,i-1,0)$;
* 如果到达的点$kk$不是$X$时:
    * 经过点$X$偶数次的状态转移方程为:$dp(kk,i,0)=dp(kk,i,0)+dp(j,i-1,0)$
    * 经过点$X$奇数次的状态转移方程为:$dp(kk,i,1)=dp(kk,i,1)+dp(j,i-1,1)$
> 最终结果为$dp(t,k,0)$

#### 代码

```cpp

    /**
     * @file    : E_-_King_Bombee.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-26 周六 19:29:14
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

    #define MAXN 2010
    #define MAXM 4010
    #define MAXK 2010

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

    int n,m,k,s,t,x;
    vector<int> v[MAXN];
    int dp[MAXN][MAXN][2];

    int main()
    {
        //main函数由此开始
        cin >> n >> m >> k >> s >> t >> x;
        for(int i=1;i<=m;i++)
        {
            int a,b;
            cin >> a >> b;
            v[a].push_back(b);
            v[b].push_back(a);
        }
        dp[s][0][0]=1;
        for(int i=1;i<=k;i++)
            for(int j=1;j<=n;j++)
                for(int kk=0;kk<v[j].size();kk++)
                {
                    if(v[j][kk]==x)
                    {
                        dp[v[j][kk]][i][0]=((ll)dp[v[j][kk]][i][0]+dp[j][i-1][1])%mod_998;
                        dp[v[j][kk]][i][1]=((ll)dp[v[j][kk]][i][1]+dp[j][i-1][0])%mod_998;
                    }
                    else
                    {
                        dp[v[j][kk]][i][1]=((ll)dp[v[j][kk]][i][1]+dp[j][i-1][1])%mod_998;
                        dp[v[j][kk]][i][0]=((ll)dp[v[j][kk]][i][0]+dp[j][i-1][0])%mod_998;
                    }
                    
                }
        
        cout << dp[t][k][0] << endl;
        
        
        return 0;
    }

```

******

### [F - Shortest Good Path](https://vjudge.csgrandeur.cn/contest/486427#problem/F)

#### 题意

> 给定一个有$N$个结点和$M$条边的无向图，结点从$1$~$N$,边从$1$到$M$,每条边$i$连接着结点$u_i$和结点$v_i$.令长度为$k$的序列(路径)$A_{1,...,k}$,对长度为$k$的$01$串$S$满足:
* 如果$S_i=0$,序列$A$中有偶数个$i$;
* 如果$S_i=1$,序列$A$中有奇数个$i$.
对于所有的$S=0,...,2^k-1$,存在一个满足该条件的$01$串$S$,记满足$S$要求的路径序列最短为$F(S)$,求$\sum_{S=0}^{2^k-1}F(S)$的结果.


#### 思路

$状压dp$,定义符合条件的$S$且最后一个是$u$的状态编号为$sta[S][u]$.

对于当前的$u->v$,可将$sta[S][u]->sta[S$^$(1<<v)][v]$,
初始状态的连边:$source->sta[1<<u][u]$,跑$bfs$求出每个状态所需的最小长度.故$F(S)=min(dis[sta[S][u]]$.

#### 代码

```cpp
    /**
     * @file    : F_-_Shortest_Good_Path.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-30 周三 15:50:17
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

    #define MAXN 17
    #define MAXM 1 << MAXN
    #define MAXK 5000010

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

    // int exgcd(int a,int b,int &x,int &y){//扩展欧几里得
    //     if(b==0){
    //         x=1,y=0;
    //         return a;}
    //     int d=exgcd(b,a%b,y,x);
    //     y-=a/b*x;
    //     return d;}

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

    // int fact[MAXN],infact[MAXN];//用于组合
    // void init(){
    //     fact[0]=infact[0]=1;
    //     for(int i=1;i<MAXN;i++){
    //         fact[i]=(ll)fact[i-1]*i%mod_998;
    //         infact[i]=(ll)infact[i-1]*fast_pow(i,mod_998-2,mod_998)%mod_998;
    //         //cout << fact[i] << ' ' << infact[i] << endl;
    //     }
    // }

    // ll C(int a,int b,int mod){//求C_{a}^{b}%mod结果
    //     if(b<0||b>n)    return 0;
    //     return (ll)fact[a]*infact[b]%mod*infact[a-b]%mod;}

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
    int id[MAXM][MAXN],cnt,dis[MAXK];
    vector<int> v[MAXK];

    queue<int> q;


    int main()
    {
        //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
        //main函数由此开始
        cin >> n >> m;
        int S=(1<<n)-1;
        for(int s=0;s<=S;s++)
            for(int u=0;u<=n-1;u++)
                id[s][u]=++cnt;
        
        for(int i=1;i<=m;i++)
        {
            int a,b;
            cin >> a >> b;
            a--,b--;
            for(int s=0;s<=S;s++)
            {
                v[id[s][a]].push_back(id[s^(1<<b)][b]);
                v[id[s][b]].push_back(id[s^(1<<a)][a]);
            }
        }
        for(int i=0;i<=n-1;i++)
            v[0].push_back(id[1<<i][i]);
        memset(dis,0x3f,sizeof(dis));
        dis[0]=0;
        q.push(0);
        while(q.size()>0)
        {
            int u=q.front();
            q.pop();
            for(auto i:v[u])
                if(dis[i]>dis[u]+1)
                {
                    dis[i]=dis[u]+1;
                    q.push(i);
                }
        }

        ll res=0;
        for(int s=1,temp;s<=S;s++)
        {
            temp=inf;
            for(int u=0;u<=n-1;u++)
                temp=min(temp,dis[id[s][u]]);
            res+=temp;
        }
        cout << res << endl;
        return 0;
    }

```

- - -

### [G - Construct Good Path](https://vjudge.csgrandeur.cn/contest/486427#problem/G)

#### 题意

好像和F题很像,升级版构造题

#### 思路

> 鸽了


#### 代码

```cpp

鸽了

```

---

### [H - Linear Maximization](https://vjudge.csgrandeur.cn/contest/486427#problem/H)

#### 题意

> 初始化给定一个空的点集合$S$,给定$Q$次询问,每次询问给你点的坐标$(X_i,Y_i)$和$A_i$和$B_i$,并把该点放入点集合$S$中,求在点集$S$中$max(A_i\times x +B_i \times y)$.

#### 思路

> 鸽了

#### 代码


```cpp

无
```
