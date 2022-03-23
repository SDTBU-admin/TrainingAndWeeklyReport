## 补题报告

#### [C - 1111gal password]

**链接：**

https://vjudge.csgrandeur.cn/contest/484224#problem/C

**题意：**

给定一个整数N，求满足以下所有条件的整数X的个数，模为998244353。

X是N位正整数。

设X_1，X_2，...... ，X_N是X从上到下的数字。它们满足以下所有条件：

对于1≤i≤N的所有整数：1≤X_i≤9 .

对于1≤i≤N−1的所有整数：|X_i-X_{i+1}|≤1 .

**思路：**

因为所有相邻的整数相差不超过1，所以一个数的相邻数为这个数+1或-1；

又因为X_i的取值范围为1-9，所以1和9只有一个相邻数：

当i=1时，dp[i-1] [j]+dp[i-1] [j+1];
当i=9时，dp[i-1] [j-1]+dp[i-1] [j];
除以上两者之外都有三个情况：dp[i-1] [j-1]+dp[i-1] [j+1]+dp[i-1] [j]

**代码：**

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <algorithm>

using namespace std;

const int MAXN=1e6+5;
const int MOD=998244353;

long long dp[MAXN][10];

int main()
{
    long long n,i,j;
    long long sum=0;
    cin >> n;
    for(i=1;i<=9;i++)
        dp[1][i]=1;
    for(i=2;i<=n;i++)
    {
        for(j=1;j<=9;j++)
        {
            if(j==1)
                dp[i][j]=(dp[i-1][j]+dp[i-1][j+1])%MOD;
            else
            {
                if(j==9)
                    dp[i][j]=(dp[i-1][j-1]+dp[i-1][j])%MOD;
                else
                    dp[i][j]=(dp[i-1][j-1]+dp[i-1][j]+dp[i-1][j+1])%MOD;
            }
        }
    }
    for(i=1;i<=9;i++)
        sum=(sum+dp[n][i])%MOD;
    cout << sum << endl;
    return 0;
}
```

#### [E - ABC Transform]

**链接：**

https://vjudge.csgrandeur.cn/contest/484224#problem/E

**题意：**

对T个测试用例解决以下问题。

给定一个整数N和一个字符串S，求出满足以下所有条件的字符串X的数量，模为998244353。

X是长度为N的字符串，由大写英文字母组成。

X是回文。

X<=S按字典顺序排列，也就是说，X=S或X在字典上比S小。

**思路：**

通过X的前[N/2] 个字符就可以确定唯一的X。

以ABCDE为例：

- ABCDE的前[N/2]个字符为ABC
- 字典序小于ABC的字符串有28个
- 判断ABCBA是否可行，与ABCDE比较
- 可行，答案增加1得到29，因此输出29，其他类似.

**代码：**

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <algorithm>

using namespace std;

#define MOD 998244353

int main()
{
    string s;
    long long t,n,i,j,k;
    cin >> t;
    while(t--)
    {
        cin >> n;
        cin >> s;
        k=0;
        j=(n-1)/2;
        for(i=0;i<=j;i++)
        {
            k=k*26+s[i]-'A';
            k=k%MOD;
        }
        long long ok=1;
        while(j>=0)
        {
            if(s[j]<s[n-1-j])
                break;
            if(s[j]>s[n-1-j])
            {
                ok=0;
                break;
            }
            j--;
        }
        if(ok!=0&&++k==MOD)
            k-=MOD;
        cout << k << endl;
    }
    return 0;
}
```

