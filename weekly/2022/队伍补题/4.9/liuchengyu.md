# 补题报告

## 神奇天平(https://ac.nowcoder.com/acm/contest/11181/A)

## 思路

在我们的常识中天平就两端，这道题假设我们有一个m端的天平，给我们n件物品，只有1件比其它的重，问我们至少称多少次可以把重的那件物品找出来。我们可以类比普通的天平，普通的天平我们都知道，有两端，分三堆称最快，类推到m端的天平，可以想到分成m+1堆称是最快的。对于这道题来说，我们每称一次都要使问题的规模尽量减小，因此每次都分成最多堆的话所需要称的次数最少，而m端最多分m+1堆，所以只需要比较n与m+1的幂的大小关系即可。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int _;
    cin >> _;
    while(_--)
    {
        long long n,m;
        cin >> n >> m;
        long long number=1;
        for(int i=1;;i++)
        {
            number*=(m+1);
            if(n<=number)
            {
                cout << i << endl;
                break;
            }
        }
    }
    return 0;
}
```

## 魔法学院(easy version)(https://ac.nowcoder.com/acm/contest/11181/B)

## 思路

这道题是简单版本，数据比较水，直接暴力模拟就能过，没啥说的。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 100005
char s[N];
int main()
{
    int n,m;
    cin >> n >> m;
    cin >> s+1;
    while(m--)
    {
        int l,r;
        char c;
        cin >> l >> r >> c;
        for(int i=l;i<=r;i++) s[i]=max(s[i],c);
    }
    int ans=0;
    for(int i=1;i<=n;i++) ans+=s[i];
    cout << ans << endl;
    return 0;
}
```

## 魔法学院(hard version)(https://ac.nowcoder.com/acm/contest/11181/C)

## 思路

当上一道题把数据改大没法暴力跑之后，这道题就变得棘手起来了，我们队的想法是先把数据由大到小sort，再按区间去跑，但是按区间跑的话操作有点复杂，难以实现，就不知道该怎么处理了，最后还是不会做，看的题解。题解的做法也是先把数据由大到小sort，然后用并查集的思想来处理区间。当一个区间已经被处理完之后，它的值就是最大的了，之后对这个区间的处理都是无意义的，所以我们用并查集来记录哪个区间用哪种操作得到的值最大，这样一来，相当于所有的位置都只跑了一次，时间上完全允许。

## 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;
#define N 10000005
char s[N];
int fa[N];
vector <pair <int,int> > v[150];
int find(int x)
{
    return fa[x]=fa[x]==x?x:find(fa[x]);
}
int main()
{
    int n,m;
    scanf("%d%d",&n,&m);
    scanf("%s",s+1);
    while(m--)
    {
        int l,r;
        char c;
        scanf("%d%d %c",&l,&r,&c);
        v[c].push_back(make_pair(l,r));
    }
    for(int i=1;i<=n;i++) fa[i]=i;
    for(int i=126;i>=33;i--)
    {
        for(int j=0;j<v[i].size();j++)
        {
            int l=v[i][j].first,r=v[i][j].second;
            for(int k=find(r);k>=l;k=find(k))
            {
                s[k]=max(s[k],(char)i);
                fa[k]=find(k-1);
            }
        }
    }
    int ans=0;
    for(int i=1;i<=n;i++) ans+=s[i];
    printf("%d\n",ans);
    return 0;
}
```
