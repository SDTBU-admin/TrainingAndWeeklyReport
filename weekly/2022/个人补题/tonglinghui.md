## 补题

### 递增三元组 

思路：题目的意思是给你三个整数A B C数组，统计有多少三元组（i，j，k），其中Ai < Bj <
Ck,一开始想的就是纯暴力，但是不出意外的是TLE了，然后就开始想其他的方法，发现，可以枚举 B 数组中的数，求出比 Bj 小的数在 A 数组中有多少，然后在找比其大的数的数目，然后进行相乘，枚举完B中所有的元素，最后得出的就是答案。

```

	#include <iostream>
	#include <algorithm>

	using namespace std;

	typedef long long ll;
	const int N = 1e5+10;

	int n,a[N],b[N],c[N];
	int main()
	{
	    cin>>n;
	    for(int i=0;i<n;i++)cin>>a[i];
	    for(int i=0;i<n;i++)cin>>b[i];
	    for(int i=0;i<n;i++)cin>>c[i];
	    
	    sort(a,a+n);
	    sort(b,b+n);
	    sort(c,c+n);

	    ll sum=0;
	    for(int i=0;i<n;i++)
	    {
	        ll x=(lower_bound(a,a+n,b[i])-a);//在数组a中找比b[i]小的数
	        //cout<<"i="<<i<<"  :"<<x<<"  ";
	        ll y=n-(upper_bound(c,c+n,b[i])-c);//在数组c中找比b[i]大的数
	        //cout<<y<<endl;
	        sum+=x*y;
	    }

	    cout<<sum;

	    return 0;
}

``` 
### 全球变暖

思路：题目的大意是有很多的小岛，然后小岛组成了几个岛屿，其中的每个小岛只要是沿海，就会被淹没，问淹没了多少岛屿，是一个BFS的问题，这个问题和以前学 BFS 的时候的一个题目很像，其中就是要定义一个数组，来标记那些岛屿被探索了，然后如果当前的小岛没有沿着海，就可以让答案 + 1，然后枚举所有有效的点。

```

	#include <iostream>
	#include <queue>
	
	using namespace std;
	
	typedef pair <int ,int> PII;
	
	const int N = 1010;
	
	int n;
	string g[N];
	bool tag[N][N];
	
	int dx[] = {-1,0,1,0};
	int dy[] = {0,1,0,-1};
	
	bool bfs(int a,int b)
	{
	  queue <PII> q;
	  q.push({a,b});
	
	  tag[a][b] = true;
	
	  bool res = true;// 表示岛屿是否被淹没
	
	  while(q.size())
	  {
	    auto t = q.front();
	    q.pop();
	
	    bool flag = false;
	    for(int i = 0; i < 4;i ++)
	    {
	      int x = t.first + dx[i];
	      int y = t.second + dy[i];
	
	      if(x >= 0 && x < n && y >= 0 && y < n && !tag[x][y])
	      {
	        if(g[x][y] == '.')
	        {
	          flag = true;//表示当前陆地已经被淹没
	          continue;
	        }
	        q.push({x,y});
	        tag[x][y] = true;
	      }
	
	    }
	      if(!flag) res = false;
	  }
	
	  return res;
	
	}
	
	int main()
	{
	  cin >> n;
	
	  for(int i = 0;i < n;i ++)
	    cin >> g[i];
	
	  int ans = 0;
	  for(int i = 0;i < n;i ++)
	    for(int j = 0;j < n;j ++)
	      if(g[i][j] == '#' && !tag[i][j])
	        ans += bfs(i,j);
	
	  cout  << ans << endl;
	  return 0;
	}
```
