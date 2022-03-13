# 补题

### 牛客练习赛 36

E、 二分答案题

如果最后一次向右，障碍只能添加在左边，向左同理，假如在x的位置放置障碍，那么0-x的位置都可以（0除外）

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e6+6;
const int inf=0x3f3f3f3f;
int a[N];int n;
int minn=inf;int maxn;
string s;
int check(int x)//判断最后能不能到达新点
{
    int p=0,pos=0;
    for(int i=0;i<n;i++)
    {
        p=max(p,pos);
        if(s[i]==s[n-1])pos++;
        else pos--;
        if(pos==-x)pos++;
    }
    return pos>p;
}
signed main()
{
    cin.tie(0);
    ios::sync_with_stdio(0);
    cin>>n;
    cin>>s;
    int l=1,r=n;
    int sum=0,ans=-1;
    while(l<=r)
    {
      int mid=(l+r)/2; 
      if(check(mid)){l=mid+1;ans=mid;}
      else{
        r=mid-1;
      }
    }
    if(ans==n)cout<<"0 1";
    else if(ans==-1)cout<<"-1";
    else cout<<"1"<<" "<<abs(ans);
}

```

F、求集合面积

根据叉乘公式，一一个顶点，求出前缀和，然后根据给出的两个点，面积就是前缀和的差减去一这两个点构成的三角形的面积，然后比较出最大值。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<double,double> pdd;
const int N=1e6+6;
const int inf=0x3f3f3f3f;
pair<double,double> a[N];
double sum[N],s,ans;
double sub(pdd a,pdd b,pdd c)
{
  return (c.first*(a.second-b.second)-c.second*(a.first-b.first)+a.first*b.second-a.second*b.first)/2.0;//叉乘公式
}
signed main()
{
    cin.tie(0);
    ios::sync_with_stdio(0);
    int n,m;cin>>n>>m;
    for(int i=1;i<=n;i++){
      double x,y;cin>>x>>y;
      a[i]=make_pair(x,y);
    }
    for(int i=3;i<=n;i++){
      sum[i]=(sum[i-1]+sub(a[i-1],a[i],a[1]));
    }
    while(m--){
      int x,y;cin>>x>>y;
      if(x>y)swap(x,y);
      double l=sum[y]-sum[x]-sub(a[x],a[y],a[1]);
      ans=max(ans,min(l,sum[n]-l));
    }
    printf("%.2lf",ans);
}

```

### 3月训练赛Ⅰ

## [D - equeue](https://vjudge.net/problem/AtCoder-abc128_d)

 两重循环模拟两边就可以了（当时竟然没想明白，菜！）

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<double,double> pdd;
const int N=1e6+6;
const int inf=0x3f3f3f3f;
int a[N],ans;
priority_queue<int,vector<int>,greater<int>> q;
signed main()
{
    cin.tie(0);
    ios::sync_with_stdio(0);
    int n,k;cin>>n>>k;
    for(int i=1;i<=n;i++)cin>>a[i];
    for(int i=0;i<=k&&i<=n;i++){
       for(int j=0;j+i<=k&&j+i<=n;j++)
       {
         int sum=0;
          for(int l=1;l<=i;l++){q.push(a[l]),sum+=a[l];}
          for(int l=1;l<=j;l++){q.push(a[n-l+1]),sum+=a[n-l+1];}
          ans=max(ans,sum);
          int r=k-i-j;
          while(r--&&!q.empty()){
            sum-=q.top();
            ans=max(ans,sum);
            q.pop();
          }
          while(!q.empty()){
            q.pop();
          }
          ans=max(ans,sum);
       }
    }
    cout<<ans;
}

```

