
## 补题报告 ACM集训队 20 级寒假第 9 次训练[比赛链接](https://vjudge.csgrandeur.cn/contest/481846)


### [E-Ranges on Tree ](https://vjudge.csgrandeur.cn/contest/481846#problem/E)

#### 题意

> 有 一棵以1为根且有$n$个结点的树，每个树上的节点都有一个以当前结点为根的子树上所有点的集合$S_i$，为树上每个结点，分配一个区间[$L_i$,$R_i$]，使得结点之间的关系与$S_i$集合个数相同

> 其中[$L_i$,$R_i$]表示从$L_i$到$R_i$之间的所有数 

> 输出保证每个区间右端点$R_i$最小的情况

#### 思路

> `dfs序`，对于每一个叶节点，让总数尽可能小，那么叶节点的$l$=$r$自然是最优的,再dfs从根往下递归，每一个根节点的$l$是使它的所有叶节点的$l$的最小值，相应的，根节点的$r$是所有叶节点的$r$的最大值




#### 代码

	/**
	  * @file    : E_-_Ranges_on_Tree.cpp
	  * @author  : SDTBU_LY
	  * @version : V1.0.0
	  * @date    : 2022-03-08 周二 20:18:02
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
	
	int n,leaf_cnt;
	int l[MAXN],r[MAXN],leaf[MAXN];
	vector<int> v[MAXN];
	
	void dfs_1(int root,int fa)
	{
	    if(root!=1&&v[root].size()==1)
	    {
	        leaf[root]=++leaf_cnt;
	        return ;
	    }
	    for(int i=0;i<v[root].size();i++)
	        if(v[root][i]!=fa)
	            dfs_1(v[root][i],root);
	}
	
	void dfs_2(int root,int fa)
	{
	    if(leaf[root]!=0)
	    {
	        l[root]=r[root]=leaf[root];
	        return ;
	    }
	    for(int i=0;i<v[root].size();i++)
	    {
	        if(v[root][i]==fa)
	            continue;
	        dfs_2(v[root][i],root);
	        l[root]=min(l[root],l[v[root][i]]);
	        r[root]=max(r[root],r[v[root][i]]);
	    }
	    
	}
	
	
	int main()
	{
	    //main函数由此开始
	    cin >> n;
	    for(int i=1;i<=n;i++)
	    {
	        l[i]=inf;
	        r[i]=-inf;
	    }
	    for(int i=1;i<=n-1;i++)
	    {
	        int a,b;
	        cin >> a >> b;
	        v[a].push_back(b);
	        v[b].push_back(a);
	    }
	
	    dfs_1(1,-1);
	    dfs_2(1,-1);
	    
	    for(int i=1;i<=n;i++)
	        cout << l[i] << ' ' << r[i] << endl;
	    return 0;
	}


### [F - Sum Sum Max ](https://vjudge.csgrandeur.cn/contest/481846#problem/F)

#### 题意

> 给定长度为$M$的数组$A$,$B$,$C$，其中$C$数组可分为$n$组$x_i$和$y_i$,意思是每一组$i$有$y_i$个$x_i$,总共构成$m$个数

> $B_i$数组表示$\sum_{k=1}^{i}C_k$,即$C$数组的前缀和

> 同理$A_i$数组表示为$\sum_{k=1}^{i}B_k$，即$B$数组的前缀和

> 求最大的$A_i$的结果 

#### 思路

> 假设$z_i=\sum_{k=1}^{i}y_k$,且$z_0=0$，其中$C_K=x_i$,求$A_{z_{i-1}+1},...,A_{z_{i}}$的最大值.
> 对于每一段$z_{i-1} + 1 \leqslant  k \leqslant  z_i$，对应的$C_k=x_i$
> 其中$A_0=B_0=0$,令$1 \leqslant j \leqslant z_i- z_{i-1} $

> $B_{z_{i-1}+j}=B_{z_{i-1}}+\sum_{k=1}^{j}C_{z_{{i-1}+k}}$
$= B_{z_{i-1}}+\sum_{k=1}^{j}x_i=B_{z_{i-1}}+x_i*j$

> $A_{z_{i-1}+j} =A_{z_{i-1}}+\sum_{k=1}^{j}B_{z_{{i-1}+k}}$
$=A_{z_{i-1}}+\sum_{k=1}^{j}(B_{z_{i-1}}+x_ik) $
$=A_{z_{i-1}}+B_{z_{i-1}}n+x_i\times \frac{n\times (n+1)}{2}$

最终转化为二次函数(**看题解有点看不太懂，尽力在写题解**)

> 令$f(n)=A_{z_{i-1}+j}=an^2+bn+c$,求$f(1),...,f(z_i-z_{i-1})$的最大值

> 讨论$a$的情况
> 其中$a\geqslant0 $时,$f(n+1)-f(n)=a(n+1)^2+b(n+1)+c-(an^2+bn+c)=2an+a+b$,最大值只会在左右端点,而$a<0$时，最大值是中间的某个点


#### 代码

	/**
	* @file    : F_-_Sum_Sum_Max.cpp
	* @author  : SDTBU_LY
	* @version : V1.0.0
	* @date    : 2022-03-08 周二 21:20:37
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

	int t,n,m;
	ll x[MAXN],y[MAXN];
	ll b[MAXN],e[MAXN],sum[MAXN];


	int main()
	{
		//main函数由此开始
		cin >> t;
		while(t--)
		{
			scanf("%d %d",&n,&m);
			for(int i=0;i<=n-1;i++)
				scanf("%lld %lld",&x[i],&y[i]);
			ll res=0;
			for(int i=0;i<=n-1;i++)
			{
				b[i]=res+x[i];
				e[i]=res+x[i]*y[i];
				res=e[i];
				sum[i]=(i>0?sum[i-1]:0)+(b[i]+e[i])*y[i]/2;
			}
			ll ans=max(b[0],sum[0]);
			for(int i=1;i<=n-1;i++)
			{
				ans=max(ans,sum[i]);
				if(b[i]>e[i]&&b[i]>0)
				{
					ll l=0,r=y[i]-1;
					while(l<=r)
					{
						ll mid=(l+r)/2;
						if(b[i]+x[i]*mid>0)
						{
							ans=max(ans,sum[i-1]+(b[i]+(b[i]+x[i]*mid))*(mid+1)/2);
							l=mid+1;
						}
						else r=mid-1;
					}
				}
			}

			cout << ans << endl;
		}
		
		
		return 0;
	}




