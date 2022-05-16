## 补题报告

##### [B - Switches]

**网址：**

https://vjudge.csgrandeur.cn/contest/483122#problem/B

**题意：**

我们有N个开关处于“开”和“关”状态，还有M个灯泡。开关编号为1至N，灯泡编号为1至M。

灯泡i连接了k_i个开关：开关s_{i1}，s_{i2}......，s_{ik_i}。

当这些开关中“开启”的开关数量与p_i**mod**2相等时，该灯亮。

**开关的“开”和“关”状态**有多少组合点亮了所有灯泡？

输出点亮所有灯泡的**开关的“开”和“关”状态组合**的数量。

- 1≤*N*,*M*≤10
- 1≤*K_i*≤*N*
- 1≤*S_ij*≤*N*
- *S_ia*\!=*S_ib*(*a*\!=*b*)
- *p_i* is 0 or 1.

**思路：**

简单的说就是暴力解题，通过递归遍历所有开关所有状态的组合；

对于每个组合都要判断其中打开的开关数是奇数还是偶数，若奇偶性满足所有灯泡的要求，则这个组合加到总数中，最后输出总数就行。

**代码：**

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <algorithm>
#include <string>

using namespace std;

int n,m,sum,f;
int a[12],k[12],p[12],all_bulb_s[12][12];

void dfs(int x)
{
    int i,j;
    if(x==n)
    {
        for(i=1;i<=m;i++)
        {
            f=0;
            for(j=1;j<=k[i];j++)
                if(a[all_bulb_s[i][j]]==1)
                    f++;
            //若有一个灯泡连接的开关打开的数目的奇偶性和要求不符
            if((f%2)!=p[i])
                return ;
        }
        //若所有灯泡连接的开关打开的数目的奇偶性和要求都相符
        sum++;
        return ;
    }
    x++;
    a[x]=0;
    dfs(x);
    //经过递归先将所有开关置为0，即所有开关处于关状态的情况
    a[x]=1;
    dfs(x);
    //全关状态到达x==n遍历完毕后，利用递归将各个开关逐渐置为1
    //这样将所有开关开合情况全部遍历一遍
}

int main()
{
    int i,j;
    cin >> n >> m;
    for(i=1;i<=m;i++)
    {
        cin >> k[i];
        for(j=1;j<=k[i];j++)
            cin >> all_bulb_s[i][j];
    }
    for(i=1;i<=m;i++)
        cin >> p[i];
    dfs(0);
    cout << sum << endl;
    return 0;
}
```



##### [D - equeue]

**网址：**

https://vjudge.csgrandeur.cn/contest/483122#problem/D

**题意：**

你朋友送给你D作为生日礼物。

D是一个水平圆柱体，包含一排N颗宝石。

珠宝的价值从左到右依次是V_1，V_2,......，V_N。可能会有价值为负值的珠宝。

一开始，你手里没有珠宝。

您最多可以对D执行K次操作，从以下选项中选择，最多执行K次（可能为零）：

操作A：取出D中最左边的宝石，放在手中。当D为空时，不能执行此操作。

操作B：取出D中最右边的宝石，放在手中。当D为空时，不能执行此操作。

操作C：选择手中的珠宝并将其插入D的左端。如果手中没有珠宝，则无法执行此操作。

操作D：选择手中的珠宝并将其插入D的右端。如果手中没有珠宝，则无法执行此操作。

在所有操作结束后，找出手中珠宝价值的最大可能总和。

**思路：**

你可以连续取出多颗宝石，但在k次操作后，你手中只能留两颗宝石；

主要限制是操作只有k次，不是无限，要在k次内取出能取出的最大价值的两颗宝石；

n和k都很小，直接暴力，枚举去1-k个的最大值

**代码：**

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <algorithm>

using namespace std;

int main()
{
    int i,j,p,q;
    int n,k,a[100],b[100];
    cin >> n >> k;
    for(i=1;i<=n;i++)
        cin >> a[i];
    int ans=-1e9;
    int s=min(n,k);
    for(i=0;i<=s;i++)
    {
        for(j=0;j+i<=s;j++)
        {
            int m=i+j;
            int tot=0,sum=0;
            for(p=1;p<=n;p++)
            {
                if(p>i&&p<n-j+1)
                    continue;
                b[tot++]=a[p];
                sum+=a[p];
            }
            sort(b,b+tot);
            q=k-m;
            while(q>=0)
            {
                if(q<=tot)
                {
                    int sums=sum;
                    for(p=0;p<q;p++)
                        sums-=b[p];
                    ans=max(ans,sums);
                }
                q--;
            }
        }
    }
    cout << ans << endl;
    return 0;
}
```

 
