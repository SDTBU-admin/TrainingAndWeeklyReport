## 补题报告 SDTBU-ACM集训队 20 级 4 月第一场训练赛[比赛链接](https://vjudge.csgrandeur.cn/contest/487382)

- - -

### [C - Choose Elements](https://vjudge.csgrandeur.cn/contest/487382#problem/C)

#### 题意

> 给定长度为$N$的两个序列$A=(A_1,...,A_N)$和$B=(B_1,...,B_N)$,询问是否存在满足以下条件的长度为$N$的序列$X={X_1,...,X_N}$:
* 每一个$X_i$可以选择$A_i$或者$B_i$,其中$1\leqslant i \leqslant N$.
* 相邻两个数的绝对值差小于等于$K$,即$|X_i-X_{i+1}|\leqslant K$,其中$1 \leqslant i \leqslant N-1$.



#### 思路

> 动态规划,其中$a$序列和$b$序列使用一个二维序列表示即可($1$表示序列$a$,$2$表示序列$b$).

设有$dp[i][3]$:
* 若$dp[i][1]=0$,表示未选取$a_i$,反之$dp[i][1]=1$,表示选取$a_i$.
* 若$dp[i][2]=0$,表示未选取$b_i$,反之$dp[i][2]=1$,表示选取$b_i$.

从小到大枚举相邻之间成立的即可,答案为$dp[n][1]||dp[n][2]$


#### 代码

```cpp
    /**
     * @file    : C_-_Choose_Elements.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-04-01 周五 17:03:01
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


    ll n,k;
    ll a[3][MAXN];
    int dp[MAXN][3];


    int main()
    {
        //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
        //main函数由此开始
        cin >> n >> k;
        for(int i=1;i<=2;i++)
            for(int j=1;j<=n;j++)
                cin >> a[i][j];
        dp[1][1]=dp[1][2]=1;
        for(int i=2;i<=n;i++)
            for(int j=1;j<=2;j++)
                for(int kk=1;kk<=2;kk++)
                    if(dp[i-1][j]==1&&abs(a[kk][i]-a[j][i-1])<=k)
                        dp[i][kk]=1;

        if(dp[n][1]==1||dp[n][2]==1)
            cout << "Yes" << endl;
        else cout << "No" << endl;
        
        return 0;
    }
```


- - -

### [D - Polynomial division](https://vjudge.csgrandeur.cn/contest/487382#problem/D)

#### 题意

> 给定多项式$A$的深度为$N$,即$A(x)=A_Nx^N+A_{N-1}x^{N-1}+...+A_1x+A_0$,然后给定多项式$B$的深度为$M$,即$B(x)=B_Mx^M+B_{M-1}x^{M-1}+...+B_1x+B_0$.
> 令$C(x)=A(x)*B(x)=C_{N+M}x^{N+M}+C_{N+M-1}x^{N+M-1}+...+C_1x+C_0$.
> 已知$A_N,A_{N-1},...,A_1,A_0$和$C_{N+M},C_{N+M-1},...,C_1,C_0$,求$B_M,B_{M-1},...,B_{1},B_{0}$的值.


#### 思路

我们知道$C_{N+M}=A_N \times B_M$,可求出$B_M$,同时去掉当前$B_M$影响的$C_{M+i}$的值.
此时$C_{N+M-1}$已经去掉$B_{M} \times A_{N-1}$,即保留了$B_{M-1}A_{N}$,可求出$B_{M-1}$.同理可求出剩下的$B_i$.时间复杂度为$O(N\times M)$.

#### 代码

```cpp
    /**
     * @file    : D_-_Polynomial_division.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-04-01 周五 16:10:40
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
    int a[MAXN],c[2*MAXN],b[MAXN];


    int main()
    {
        //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
        //main函数由此开始
        cin >> n >> m;
        for(int i=0;i<=n;i++)
            cin >> a[i];
        for(int i=0;i<=n+m;i++)
            cin >> c[i];
        for(int i=m;i>=0;i--)
        {
            b[i]=c[n+i]/a[n];
            for(int j=0;j<=n;j++)
                c[i+j]-=a[j]*b[i];
        }
        for(int i=0;i<=m;i++)
            cout << b[i]  << ' ';
        return 0;
    }

```

- - -


### [E - Wrapping Chocolate](https://vjudge.csgrandeur.cn/contest/487382#problem/E)

#### 题意

> 有$N$块巧克力,每一块$i$都有对应的长$B_i$和宽$A_i$.然后由$M$个盒子,每个盒子都有对应的长$D_i$和宽$C_i$,其中每个盒子最多可装一块满足$B_i \leqslant D_i$并且$A_i \leqslant C_i$的巧克力.问能不能把$N$块巧克力都装进盒子里(盒子的数量不少于巧克力的数量).


#### 思路

> $贪心+数据结构$,将巧克力和盒子的长宽用结构体存储,然后进行排序,从大到小遍历所有的巧克力,把当前巧克力的长大的宽放入$multiset$里维护,保证$multiset$中的所有盒子的长都比以后的巧克力的长大.同时利用$二分$找到第一个比当前巧克力宽大的盒子,如果找到,就去掉$erase$,否则输入no即可.



#### 代码

```cpp
    /**
     * @file    : E_-_Wrapping_Chocolate.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-04-09 周六 16:57:54
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
    struct node{
        int x;
        int y;
    }s1[MAXN],s2[MAXN];

    int cmp(struct node a,struct node b)
    {
        if(a.x==b.x)
            return a.y>b.y;
        else return a.x>b.x;
    }

    multiset<int> ss;

    int main()
    {
        //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
        //main函数由此开始
        cin >> n >> m;
        for(int i=1;i<=n;i++)
            scanf("%d",&s1[i].x);
        for(int i=1;i<=n;i++)
            scanf("%d",&s1[i].y);
        for(int i=1;i<=m;i++)
            scanf("%d",&s2[i].x);
        for(int i=1;i<=m;i++)
            scanf("%d",&s2[i].y);
        sort(s1+1,s1+n+1,cmp);
        sort(s2+1,s2+m+1,cmp);
        int j=1;
        for(int i=1;i<=n;i++)
        {
            while(s1[i].x<=s2[j].x&&j<=m)
            {
                ss.insert(s2[j].y);
                j++;
            }
            auto temp=ss.lower_bound(s1[i].y);
            if(temp==ss.end())
            {
                printf("No\n");
                return 0;
            }
            else ss.erase(temp);
        }
        printf("Yes\n");
        return 0;
    }
```

- - -


### [F - Endless Walk](https://vjudge.csgrandeur.cn/contest/487382#problem/F)

#### 题意

> 给你一个有向图$G$,有$N$个点(点从$1-N$)和$M$条边(每条边$i$从$u_i$指向$v_i$).询问满足以下条件的点的个数:
* 从某个点出发，存在一条无限长的路径


#### 思路


反向拓扑,该点满足条件即该点在环上或者它可以通过路径到达环,故只需将未进入环的边处理即可,建立反向图把入度为0的点筛除即为结果.

#### 代码

```cpp
    /**
     * @file    : F_-_Endless_Walk.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-04-09 周六 17:43:49
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

    int n,m,deg[MAXN],res;//存储入度
    vector<int> v[MAXN];

    int main()
    {
        //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
        //main函数由此开始
        cin >> n >> m;
        for(int i=1;i<=m;i++)
        {
            int a,b;
            scanf("%d %d",&a,&b);
            v[b].push_back(a);//建反向图
            deg[a]++;
        }
        queue<int> q;
        for(int i=1;i<=n;i++)
            if(deg[i]==0)
            {
                q.push(i);
                res++;
            }
        while(q.size()>0)
        {
            int temp=q.front();
            q.pop();
            for(auto i:v[temp])
            {
                deg[i]--;
                if(deg[i]==0)
                {
                    q.push(i);
                    res++;
                }
            }
        }
        cout << n-res << endl;
        return 0;
    }
```

- - -

### [G - Foreign Friends](https://vjudge.csgrandeur.cn/contest/487382#problem/G)

#### 题意

> 给你$N$个人和$K$个国家,每个人$i$都有着对应的国家$A_i$,其中有$L$个名人$B$分别属于不同的国家,最开始每两人之间都不认识,存在$M$对关系,$Takahashi$能通过花费$C_i$使得$u_i$和$v_i$成为朋友(如果a认识b,b认识c,那么a认识c).
> 对于$N$个人中的每一个$i$都想认识一个不属于本国受欢迎的人,问每个人的最小花费为多少.

#### 思路

多源最短路$Dijkstra$,忽略不同的国家,如果找每个人成为名人的朋友最短花费,对于每个名人成为该人的朋友的最小,反向枚举每个名人作为起点,考虑两种情况:
* 当前人是名人,答案为最短路
* 当前人不是名人,答案为次短路.
  
#### 代码

```cpp
    /**
     * @file    : G_-_Foreign_Friends.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-04-09 周六 20:05:35
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
    #define MAXM 200010
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

    int n,m,k,l;
    int a[MAXN],b[MAXN],vis[MAXN],type[MAXN];
    int head[MAXN],ed[MAXM],nex[MAXM],idx;

    ll dis1[MAXN],dis2[MAXN],val[MAXM];

    void add(int start_,int end_,ll w)
    {
        ed[idx]=end_;
        val[idx]=w;
        nex[idx]=head[start_];
        head[start_]=idx++;
    }

    priority_queue< pair<ll,PII>,vector<pair<ll,PII> >,greater <pair<ll,PII> >  > q;


    void Dijkstra()
    {
        while(q.size()>0)
        {
            auto temp=q.top();
            q.pop();
            ll cost=temp.first;
            int nation=temp.second.second,pos=temp.second.first;
            if(vis[pos]<2&&type[pos]!=nation)
            {
                if(vis[pos]==0)
                    type[pos]=nation,dis1[pos]=cost;
                else dis2[pos]=cost;
                vis[pos]++;
                for(int i=head[pos];i!=-1;i=nex[i])
                    q.push({cost+val[i],{ed[i],nation}});
            }
        }
    }

    int main()
    {
        //ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
        //main函数由此开始
        memset(head,-1,sizeof(head));
        cin >> n >> m >> k >> l;
        for(int i=1;i<=n;i++)
            scanf("%d",&a[i]);
        for(int i=1;i<=l;i++)
        {
            scanf("%d",&b[i]);
            q.push({(ll)0,{b[i],a[b[i]]}});
        }
        for(int i=1;i<=m;i++)
        {
            int  start_,end_;
            ll w;
            cin >> start_ >> end_ >> w;
            add(start_,end_,w);
            add(end_,start_,w);
        }
        Dijkstra();
        for(int i=1;i<=n;i++)
        {
            if(type[i]!=a[i]&&dis1[i])
                cout << dis1[i] << ' ';
            else if(dis2[i])
                cout << dis2[i] << ' ';
            else cout << "-1 " ;
        }

        return 0;
    }
```

- - -

### [H - Product Modulo 2](https://vjudge.csgrandeur.cn/contest/487382#problem/H)

#### 题意

数论题


#### 思路

鸽了



#### 代码
```cpp
cout << "不会" <<endl;
```
