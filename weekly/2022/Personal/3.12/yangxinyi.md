# 补题报告

## 题目[C - Jumping Takahashi](https://vjudge.csgrandeur.cn/problem/AtCoder-abc240_c)

## 思路

- 双重循环，外循环表示层数，内层循环则表示高度。
- 最后只需要看f【n】【m】是否达到过即可。

## 代码

```
#include<stdio.h>
#include<string.h>
#define scnaf scanf
const int N=1e4+50;
int f[150][N],a[150],b[150];
int main()
{
    int i,j,n,m;
    scnaf("%d%d",&n,&m);
    for(i=1;i<=n;i++)
        scnaf("%d%d",&a[i],&b[i]);
    f[0][0]=1;
    for(i=0;i<n;i++)
    {
        for(j=0;j<=m;j++)
        {
            if(f[i][j])
                f[i+1][j+a[i+1]]=1;
            if(f[i][j])
                f[i+1][j+b[i+1]]=1;
        }
    }
    if(f[n][m])
        printf("Yes\n");
    else printf("No\n");
    return 0;
}
```

## 题目[D - Strange Balls](https://vjudge.csgrandeur.cn/problem/AtCoder-abc240_d)

## 思路

- 栈＋前缀和
- 通过构造结构体表示每个球的数字和相邻球数字相同数，当前一个球的个数+1等于当前球标号时，出栈。
- 其它情况入栈。每次检验之后输出当前栈中个数。

## 代码

```
#include<stack>
#include<stdio.h>
#include<string.h>
#include<algorithm>
using namespace std;
#define scnaf scanf
const int N=2e5+50;
int a[N];
struct ss
{
    int a,b;
};
int main()
{
    int i,j,k,n;
    stack<struct ss>p;
    struct ss s;
    scnaf("%d",&n);
    for(i=1;i<=n;i++)
        scnaf("%d",&a[i]);
    for(i=1;i<=n;i++)
    {
        if(p.empty()||p.top().a!=a[i])
        {
            s.a=a[i],s.b=1;
            p.push(s);
        }
        else
        {
            if(p.top().b+1==a[i])
            {
                while(!p.empty()&&p.top().a==a[i])
                        p.pop();
            }
            else
            {
                s.b=p.top().b+1;
                s.a=a[i];
                p.push(s);
            }

        }
        printf("%d\n",p.size());
    }
    return 0;

}
```

