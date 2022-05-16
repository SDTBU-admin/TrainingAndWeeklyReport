## 补题报告 SDTBU-ACM集训队 20 级 3 月第二场训练赛[比赛链接](https://vjudge.csgrandeur.cn/contest/484224)

- - -

### [C - 1111gal password](https://vjudge.csgrandeur.cn/contest/484224#problem/C)

#### 题意

> 给定长度为$n$的数字$X$,求满足以下条件的方案数(结果对$998244353$求余):
 * 每一个数字$X_i$是$1 \sim 9$之间的数:$1\leqslant X_i\leqslant 9$ ($1\leqslant i\leqslant n$);
 * 每两个相邻数字差的绝对值$\leqslant 1$:$\left| X_{i+1} -X_i\right|\leqslant 1$($1\leqslant i\leqslant n-1 $).

#### 思路

> 利用$dp$进行解决,设$dp(i,j)$表示前$i$位数中以$j$结尾的合法方案数,$dp(i,j)$可由前$i-1$位数以$j,j-1,j+1$结尾的方案数构成,注意考虑边界情况$j-1\geqslant 1$或$j+1 \leqslant 9$,即状态转移方程为:$dp[i][j]=dp[i-1][j-1]+(j-1\geqslant 1?1:0) \times dp[i-1][j-1]+(j+1 \leqslant 9?1:0) \times dp[j-1][j+1]$
> 注意初始化$dp[1][i]=1(1\leqslant i \leqslant n)$


#### 代码

``` cpp

    /**
     * @file    : C_-_1111_gal_password.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-14 周一 19:03:01
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
    int dp[MAXN][10];

    int main()
    {
        //main函数由此开始
        cin >> n;
        for(int i=1;i<=9;i++)
            dp[1][i]=1;
        for(int i=2;i<=n;i++)
        {
            for(int j=1;j<=9;j++)
            {
                dp[i][j]=dp[i-1][j];
                if(j-1>=1)
                    dp[i][j]=(dp[i][j]+dp[i-1][j-1])%mod_998;
                if(j+1<=9)
                    dp[i][j]=(dp[i][j]+dp[i-1][j+1])%mod_998;
            }
        }
        int res=0;
        for(int i=1;i<=9;i++)
            res=(res+dp[n][i])%mod_998;
        cout << res << endl;
        
        
        return 0;
    }

```



- - - 


### [D - ABC Transform](https://vjudge.csgrandeur.cn/contest/484224#problem/D)

#### 题意

> 给定由大写字母$A,B,C$构成的字符串$S$,令$S_0=S$,$S_i=S_{i-1}$将$A$变为$BC$,将$B$变为$CA$,将$C$变为$AB$,然后给定$Q$次询问,对于第$i$次询问有两个数$t_i$和$k_i$,问第$S_{t_{i}}$个字符串的第$k_i$个字符是什么.

#### 思路

> 将原字符串$S$的每个字符$S_i$形成一个类似于二叉树,进行递归搜索,重点写在递归函数中,此处不作解释.

#### 代码

```cpp

    /**
     * @file    : D_-_ABC_Transform.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-14 周一 20:13:48
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

    int q;
    string s;

    char ask(ll t,ll k)//第t层,第k位
    {
        if(t==0)//递归底层，返回当前字符
            return s[k];
        if(k==0)//访问到第一个字符,加上层数求余
            return 'A'+(s[0]-'A'+t)%3;
        return 'A'+(ask(t-1,k/2)-'A'+k%2+1)%3;
        /*
        k%2+1判断左右子树,左子树是2*root(偶数),右子树是2*root+1(奇数)故左子树+1,右子树+2
        ABC-->BCCAAB,变化后第二个C是B的左子树,在它的父结点基础上+1,使得B-->C
        变化后的第一个A是B的右子树,在它的父结点基础上+2,使得B-->A
        */
    }

    int main()
    {
        //main函数由此开始
        cin >> s;
        cin >> q;
        while(q--)
        {
            ll t,k;
            scanf("%lld %lld",&t,&k);
            printf("%c\n",ask(t,k-1));
        }
        return 0;
    }


```



- - -

### [E - (∀x∀)](https://vjudge.csgrandeur.cn/contest/484224#problem/E)

#### 题意

> 有$t$组测试样例,每组给定一个整数$N$和一个字符串$S$,求满足以下条件的字符串$X$的方案数,结果对$998244353$求余:
* 字符串$X$的长度与字符串$S$的长度均为$N$且仅有大写字母组成;
* 字符串$X$是回文串;
* 该字符串$X$的字典序**小于等于**字符串$S$的字典序.

#### 思路

> 一个长度为$N$的回文串可由它的前一半的子串通过**翻转**唯一确定,设字符串$K$是$S$字符串的前一半,长度$len$为$\left \lceil \frac{N}{2} \right \rceil$:
* 字典序小于$T$的字符串,总方案数为$\sum_{i=1}^{len}{26^{len-i}\times(S_i-'A')}$
* 字典序等于$T$的字符串,特判一下翻折后是否超过原字符串$S$


#### 代码

```cpp
    /**
     * @file    : E_-_x.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-15 周二 10:49:14
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

    int t,n,a[MAXN];
    string s;

    int main()
    {
        //main函数由此开始
        cin >> t;
        while(t--)
        {
            cin >> n >> s;
            int len=(n+1)/2;
            a[0]=1;
            for(int i=1;i<=len;i++)
                a[i]=(ll)a[i-1]*26%mod_998;
            int res=0;
            for(int i=0;i<len;i++)
            {
                res=(res+(ll)a[len-1-i]*(s[i]-'A'))%mod_998;   
            }
            string temp=s.substr(0,n-len);
            reverse(temp.begin(),temp.end());
            temp=s.substr(0,len)+temp;
            if(temp<=s)
                res=(res+1)%mod_998;
            cout << res << endl;
        }
        return 0;
    }


```



- - -
### [F - Black and White Rooks](https://vjudge.csgrandeur.cn/contest/484224#problem/F)

#### 题意

> 在一个 $N \times M$的矩阵中放$B$个黑子和$W$个白子,求满足以下条件的方案数,结果对$998244353$求余:
* 所有的黑白子各放在一个格子里并且每个格子最多放一个黑白子;
* 两个不同颜色的子不在同一行或者同一列.

#### 思路

> 保证两个颜色的子不在同一行或一列,假设白子占用了$i$行$j$列,此时黑子不能再这些行列放置,黑子能放置的格子个数为$n \times m- i \times m - j \times n + i \times j$个($i \times j$为重复相交部分).

> 定义$F(i,j)$为占用$i$行$j$列的方案数
当前$i,j$贡献的答案为:$C_{n}^{i} \times C_{m}^{j} \times F(i,j) \times C_{n \times m- i \times m - j \times n + i \times j}^{B}$
从$n$行里面选出$i$行，$m$列里面选出$j$列，白子完全占用这$i$行$j$列，剩下的格子放置黑子.
令$F(i,j)$=$C_{i \times j}^{W}$(在$i \times j$里放置$W$个白子)
> 由于有些放置方案是占满不了$i$行$j$列，需枚举$i \times j $中有$x$行未被占用,$y$列未被占用，剩下的是被完全占用,其值为$F(i-x,j-y)$.
> 对于枚举到的$x,y$,结果为$F(i,j)-C_{i}^{x} \times C_{j}^{y} \times F(i-x,j-y)$

#### 代码

> 鸽了求模运算麻烦
```cpp
    /**
     * @file    : F_-_Black_and_White_Rooks.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-15 周二 11:57:36
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

    int n,m,b,w;
    int res;
    ll C[MAXN*MAXN][MAXN*MAXN];
    ll F[MAXN][MAXN];

    void init()
    {
        C[0][0]=C[1][0]=C[1][1]=1;
        for(int i=2;i<=2500;i++)
        {
            C[i][0]=1;
            for(int j=1;j<=i;j++)
                C[i][j]=(C[i-1][j]%mod_998+C[i-1][j-1]%mod_998+mod_998)%mod_998;
        }

    }


    int main()
    {
        //main函数由此开始
        init();
        cin >> n >> m >> b >> w;
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j<=m;j++)
            {
                
                if(i*j>=w)
                {
                    //cout << "???" << endl;
                    F[i][j]=C[i*j][w];
                    for(int x=0;x<i;x++)
                    {
                        for(int y=0;y<j;y++)
                        {
                            if(x==0&&y==0)
                                continue;
                            F[i][j]=(F[i][j]-((C[i][x]%mod_998*C[j][y]%mod_998)%mod_998*F[i-x][j-y]%mod_998)%mod_998+mod_998)%mod_998;
                        }
                    }
                    res=(res+((C[n][i]%mod_998*C[m][j]%mod_998)%mod_998*F[i][j]%mod_998*C[n*m-i*m-j*n+i*j][b]%mod_998)%mod_998+mod_998)%mod_998;
                }
            }
        }
        cout << res << endl;
        
        return 0;
    }



```


- - -

### [G - Range Pairing Query](https://vjudge.csgrandeur.cn/contest/484224#problem/G)

#### 题意

> 给定$N$个人,每个人穿的颜色是$A_i$,然后给定$Q$次询问,询问区间内$[l,r]$最多可组成颜色相同对数(即一人只能与一人进行匹配相同)


#### 思路

> **莫队+分块**,分块并排序(优先根据块的位置其次根据右端点),然后进行左右区间变化离线操作,当当前颜色为偶数并且区间减小一个当前颜色是$对数-1$,当当前颜色为奇数并且区间增加一个当前颜色是$对数+1$.


#### 代码

```cpp

    /**
     * @file    : G_-_Range_Pairing_Query.cpp
     * @author  : SDTBU_LY
     * @version : V1.0.0
     * @date    : 2022-03-11 周五 16:52:07
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
    #define MAXM 1000010
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

    int n,m,a[MAXN],block,bl[MAXN];
    ll sum=0,cnt[MAXN];
    
    ll res[MAXM];

    struct node{
        int l;
        int r;
        int id;
    }s[MAXM];

    int cmp(struct node x,struct node y)
    {
        if(bl[x.l]==bl[y.l])
            return x.r<y.r;
        else return bl[x.l]<bl[y.l];
    }

    void add(int num)
    {
        if(cnt[num]%2==1)
            sum++;
        cnt[num]++;
    }

    void del(int num)
    {
        if(cnt[num]%2==0)
            sum--;
        cnt[num]--;
    }



    int main()
    {
        //main函数由此开始
        scanf("%d",&n);
        block=sqrt(n);
        for(int i=1;i<=n;i++)
        {
            scanf("%d",&a[i]);
            bl[i]=(i-1)/block;
        }
        
        scanf("%d",&m);
        for(int i=1;i<=m;i++)
        {
            scanf("%d %d",&s[i].l,&s[i].r);
            s[i].id=i;
        }
        sort(s+1,s+m+1,cmp);

        int lef=1,rig=0;
        
        for(int i=1;i<=m;i++)
        {
            while(lef>s[i].l) 
                add(a[--lef]);
            while(rig<s[i].r)
                add(a[++rig]);
            while(lef<s[i].l)
                del(a[lef++]);
            while(rig>s[i].r)
                del(a[rig--]);
            res[s[i].id]=sum;
        }


        for(int i=1;i<=m;i++)
            printf("%lld\n",res[i]);
        return 0;
    }

```
