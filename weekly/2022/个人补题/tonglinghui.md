## 补题
### C - Jumping Takahashi

思路：利用动态规划来做，其实上就是暴力打表，把每一种情况都表示出来，如果最后`dp[n][m] == 1`,说明可以到达。

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
	
	const int N = 2010,INF=0x3f3f3f3f;
	typedef long long ll;
	
	// 升序
	//priority_queue <int,vector<int>,greater<int> > q;
	// 降序
	//priority_queue <int,vector<int>,less<int> >q;
	
	int n,m;
	int dp[110][10100];
	
	int main()
	{
	    cin >> n >> m;
	    
	    for(int i = 0;i < n ;i ++)
	    {
	        int a,b;
	        cin >> a >> b;
	
	        dp[0][0] = 1;
	
	        for(int j = 0;j <= m ;j ++)
	        {
	          if(dp[i][j] == 1)
	          {
	            if(j + a <= m) dp[i + 1][j + a] = 1;
	            if(j + b <= m) dp[i + 1][j + b] = 1;
	          }
	        }
	    }
	    if(dp[n][m] ==  1)
	      puts("Yes");
	    else
	      puts("No");
	    return 0;
	}

``` 
