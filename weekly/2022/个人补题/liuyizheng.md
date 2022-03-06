## 补题报告

https://vjudge.csgrandeur.cn/contest/481846#problem/C

#### **[C - Jumping Takahashi]**

 **大体题意：**

Takahashi站在坐标轴的0坐标处，他一共要进行N次跳跃，每次跳跃的长度为ai或bi，如果在N次跳跃结束后，Takahashi能刚好站在坐标X处则输出Yes，否则输出No

**思路：**

在主函数输入输出，写一个函数判断能否达到要求

首先设参数，分别为还**剩几次跳跃次数N**和还有多长到达X即**和X的距离**

其次考虑函数的**结束条件**，有刚好到达坐标X、无法到达X和超过X**三种可能**；比X小和是否刚好到达X只有在N次结束后才看到出来，而比X大是有可能在N次结束之前就达到的条件（这是三种可能的区别，以此来写**判断条件**）；

然后我们设一个跳跃次数和对应坐标二维数组，每跳一次对应一个坐标，我们可以每跳一次分别用x减ai和bi的坐标（**判断是否到达或超越X**）及对应的跳跃次数（**每跳一次，N减1，判断是否跳完**）进行一次递归调用，每调用一次则进行一次上面所说的判断，若所有判断条件都没达到，则先将本节点标记，返回其标记值，表明此节点已经遍历到（ai和bi可能会便利到同一节点），直到达成以上三种结束条件之一

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>

using namespace std;

int n,x;
int a[105],b[105];
int m[105][10005];

int f(int n,int x)
{
    //x<0,说明a[i]或b[i]大了，超过了x的长度，即无法正好落在坐标x处
    if(x<0)
        return 0;
    //如果跳跃次数耗尽（递归结束），若x==0则能刚好到达坐标x处，否则不能
    if(n==0)
    {
        if(x==0)
            return 1;
        else
            return 0;
    }
    //如果这个点已经被遍历过赋值了，直接返回此点的值
    if(m[n][x]!=-1)
        return m[n][x];
    //跳跃次数减1，当x减去此次a或b的值后，看x是否刚好为0，是则返回1，否则返回0
    if(f(n-1,x-a[n])||f(n-1,x-b[n]))
        m[n][x]=1;
    else
        m[n][x]=0;
    return m[n][x];
}

int main()
{
    cin >> n >> x;
    for(int i=1;i<=n;i++)
        cin >> a[i] >> b[i];
    memset(m,-1,sizeof(m));
    int k=f(n,x);
    if(k==0)
        cout << "No" << endl;
    else
        cout << "Yes" << endl;
    return 0;
}
```

https://vjudge.csgrandeur.cn/contest/481846#problem/D

#### **[D - Strange Balls]**

 **题意：**

Takahashi有N个球，每个球上有一个大于等于2的整数，他将它们一个接一个的插入一个圆筒之中，若x个写有数字x的球相邻，这几个相邻的球会消掉，问每插入一个球，筒中还剩几个球

**思路：**

设两个数组，一个存每一次插入的球上的数，一个存相邻的相同数字的球的个数；

每插入一个球，在数组a中存入球上数字并判断球的数字和前一个是否相同，不是则计数1（即第一个无相同值相邻的值为x的球），是则为前一个球的数目加一（即计量这种球的数目），若球的数目和球的值相同了，则在数组a中去除这几个球，然后输出现在筒中球的数目。

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <algorithm>

using namespace std;

int a[200005],b[200005];

int main()
{
    int n,i,f,t=0;
    cin >> n;
    for(i=0;i<n;i++)
    {
        cin >> f;
        a[++t]=f;
        //如果是第一个数或者刚刚输入的数不等于上一个数
        //那么这个数是这种数的第一个
        if(t==1||a[t]!=a[t-1])
            b[t]=1;
        //若这个数和前一个数相等则它们为连续的相同的数
        //在b数组统计连续相同的数的数目
        else
            b[t]=b[t-1]+1;
        //若统计数目和数本身的值相同，消掉这几个连续相同的数
        if(b[t]==f)
            t=t-f;
        cout << t << endl;
    }
    return 0;
}
```

