## 题解

### 题目：Coprime 2 (四月份训练题)

### 思路
1. 给出 n 个数，然后给出一个 m，求1 - m 之间有多少和给出的 n 个数都互质，然后输出符合题目的数。
2. 将给出的 n 个数的质因子求出来，都存在一个数组里，然后根据筛质数的方法将符合题意的数打表出来，然后逐个判断符合题意的数

### 代码

```
    #include <cstdio>
    #include <vector>
    #include <cstring>
    #include <iostream>
    #include <algorithm>

    using namespace std;

    const int N = 1e5 + 20;

    int n, m, cnt;
    int a[N], b[N], c[N], d[N];

    int main()
    {
        scanf("%d%d",&n, &m);

        for(int i = 0; i < n; i++ )
        {
            scanf("%d", &a[i]);

            for(int j = 2;j <= a[i] / j; j ++)
            {
                if(a[i] % j == 0)
                {
                    b[j] = 1;
                    while(a[i] % j == 0)
                        a[i] /= j;
                }
            }

            if(a[i] > 1)
                b[a[i]] = 1;
        }

        for(int i = 2;i <= m;i ++)
        {
            if(b[i])
                c[++ cnt] = i;
        }

        for(int i = 1;i <= cnt;i ++)
        {
            for(int j = c[i] + c[i];j <= m;j += c[i])
                b[j] = 1;
        }

        vector <int> ans;

        for(int i = 1;i <= m;i ++)
        {
            if(!b[i])
                ans.push_back(i);
        }

        printf("%d\n",ans.size());

        for(int i = 0; i < ans.size(); i ++)
            printf("%d\n", ans[i]);

        return 0;
    }

```
## 题目：Cylinder （上周个人赛的题目）

### 思路：将球的个数和价值用pair存起来，然后取的时候直接按照该球的个数进行操作，所得的答案根据取出球的个数和价值来进行乘法原理相乘。

### 原因：当时做的时候，少了一个等于号，就是取出的球正好是所有的个数的时候的情况少算了，所以没有叫上 0.0

### 代码
```
    #include <set>
    #include <map>
    #include <queue>
    #include <cstdio>
    #include <cstring>
    #include <iostream>
    #include <algorithm>

    using namespace std;

    const int N = 2e5 + 10,INF = 0x3f3f3f3f;

    typedef long long ll;
    typedef pair <ll,ll> PII;

    ll n;
    PII d[N];

    int main()
    {
        ll tt = 0,hh = 0;
        cin >> n;
        for(ll i = 1;i <= n;i ++)
        {
            ll cmd,w,sum;
            scanf("%lld",&cmd);
            if(cmd == 1)
            {
                scanf("%lld%lld",&w,&sum);
                d[tt ++] = {w,sum};
            }
            else
            {
                scanf("%lld",&sum);
                
                ll res = 0;
                
                for(ll j = hh;j < tt;j ++,hh ++)
                {
                    if(sum <= d[j].second)
                    {
                        d[j].second -= sum;
                        //cout << d[j].second << endl;
                        res += sum * d[j].first;
                        
                        printf("%lld\n",res);
                        //hh = j;
                        break;
                    }
                    else
                    {
                        res += d[j].first * d[j].second;
                        sum -= d[j].second;
                    }
                }
                
            }
        }
        

        return 0;
    }
    // 2 2 2 3 3 3 3
```
