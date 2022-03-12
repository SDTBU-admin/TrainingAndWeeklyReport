## 补题
### C - Switches 

思路：DFS ，因为 N 是小于 10 的，可以进行暴搜，将所有的状态都列举出来，每列举一种情况，就要判断所有的灯是不是都能打开，如果能打开，就 ans ++，最后将 ans 输出。

```

	#include <map>
	#include <queue>
	#include <stack>
	#include <math.h>
	#include <stdio.h>
	#include <stdlib.h>
	#include <string.h>
	#include <iostream>
	#include <algorithm>
	
	using namespace std;
	
	const int N = 20,INF=0x3f3f3f3f;
	typedef long long ll;
	typedef pair <int,int> PII;
	
	// 升序
	//priority_queue <int,vector<int>,greater<int> > q;
	// 降序
	//priority_queue <int,vector<int>,less<int> >q;
	
	struct Node
	{
	    int n;
	    int on[N];
	    int p;
	}node[N];
	
	int n,m,ans;
	int path[N];
	//每一个开关的状态
	void dfs(int u)
	{
	    if(n + 1 == u)
	    {
	        bool flag = true;
	        for(int i = 1;i <= m;i ++)
	        {
	            int cnt = 0;
	            for(int j = 1;j <= n;j ++)
	            {
	                if(path[j] && node[i].on[j])
	                    cnt ++;
	            }
	
	            if(cnt % 2 != node[i].p)
	                return ;
	        }
	        ans ++;
	        return ;
	    }
	
	    for(int i = 0;i <= 1;i ++)
	    {
	        path[u] = i;
	        dfs(u + 1);
	    }
	}
	
	int main()
	{
	    cin >> n >> m;
	
	    for(int i = 1;i <= m;i ++)
	    {
	        cin >> node[i].n;
	        for(int k = 1;k <= node[i].n;k ++)
	        {
	            int x;
	            cin >> x;
	            node[i].on[x] = 1;
	        }
	    }
	
	    for(int i = 1;i <= m;i ++)
	    {
	        cin >> node[i].p;
	    }
	
	    dfs(1);
	
	    cout << ans << endl;
	    return 0;
	}
	/*
	2 2
	2 1 2
	1 2
	0 1
	
	*/


``` 
