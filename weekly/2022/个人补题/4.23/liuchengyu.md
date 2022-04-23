# 补题报告

## Move Right(https://atcoder.jp/contests/abc247/tasks/abc247_a)

## 思路

这题相当于位运算中的右移，最高位添0，其余的三位为移动之前的高三位。

## 代码

```cpp
#include <iostream>
#include <string>
using namespace std;
int main()
{
    string s;
    cin >> s;
    cout << '0' << s[0] << s[1] << s[2] << endl;
    return 0;
}
```

## Unique Nicknames(https://atcoder.jp/contests/abc247/tasks/abc247_b)

## 思路

个人认为这题比C题、D题都难，主要这题有坑……为了描述方便我们假设s为一个人的姓，t为一个人的名，开一个map来统计每一个人的姓以及名出现的次数，然后分两种情况讨论。为什么分两种情况讨论呢？因为坑就在这里！题目中没说一个人的姓不能和名相同，当一个人的姓和名不同时，如果姓和名都出现过不止一次，就无法获得一个唯一的昵称，输出"NO"；当一个人的姓和名相同时，如果姓和名都出现过不止两次，就无法获得一个唯一的昵称，输出"NO"。除此之外的情况均可以为每个人找到一个唯一的昵称，输出"YES"。

## 代码

```cpp
#include <iostream>
#include <string>
#include <map>
using namespace std;
#define N 105
string s[N],t[N];
map <string,int> mp;
int main()
{
    int n;
    cin >> n;
    for(int i=1;i<=n;i++)
    {
        cin >> s[i] >> t[i];
        mp[s[i]]++;
        mp[t[i]]++;
    }
    int flag=1;
    for(int i=1;i<=n;i++)
    {
        if(s[i]!=t[i]&&mp[s[i]]!=1&&mp[t[i]]!=1)
        {
            flag=0;
            break;
        }
        if(s[i]==t[i]&&mp[s[i]]!=2&&mp[t[i]]!=2)
        {
            flag=0;
            break;
        }
    }
    if(flag) cout << "Yes" << endl;
    else cout << "No" << endl;
    return 0;
}
```

## 1 2 1 3 1 2 1(https://atcoder.jp/contests/abc247/tasks/abc247_c)

## 思路

这题可太水了，感觉这题才应该是B题……没啥好说的，直接开几个vector数组暴力模拟题目的要求即可。

## 代码

```cpp
#include <iostream>
#include <vector>
using namespace std;
vector <int> v[20];
int main()
{
    int n;
    cin >> n;
    v[1].push_back(1);
    for(int i=2;i<=n;i++)
    {
        for(int j=0;j<v[i-1].size();j++) v[i].push_back(v[i-1][j]);
        v[i].push_back(i);
        for(int j=0;j<v[i-1].size();j++) v[i].push_back(v[i-1][j]);
    }
    for(int i=0;i<v[n].size();i++)
    {
        if(!i) cout << v[n][i];
        else cout << " " << v[n][i];
    }
    cout << endl;
    return 0;
}
```

## Cylinder(https://atcoder.jp/contests/abc247/tasks/abc247_d)

## 思路

这题也比较简单，先开一个结构体，结构体中有两个变量，一个记录球的价值，一个记录球的数量，之后再开一个结构体队列模拟题目的操作即可。放球进队列好说，直接push结构体即可，关键是出队列。在出队列时，如果当前待出队列的球的个数大于等于队首球的个数，那么待出队列的球的个数就要减去队首球的个数，同时答案应加上队首球的价值与队首球的个数的积，并将队首出队。如果当前待出队列的球的个数小于队首球的个数，那么直接给答案加上队首球的价值与待出队列的球的个数的积，之后退出循环输出答案即可。总的来说，这题可以拿队列来直接模拟，没什么难度。

## 代码

```cpp
#include <iostream>
#include <queue>
using namespace std;
struct Ball
{
    long long x;
    long long c;
};
queue <Ball> q;
int main()
{
    int t;
    cin >> t;
    while(t--)
    {
        int opt;
        cin >> opt;
        if(opt==1)
        {
            long long x,c;
            cin >> x >> c;
            q.push((Ball){x,c});
        }
        else
        {
            long long c;
            cin >> c;
            long long ans=0;
            while(!q.empty())
            {
                if(c>=q.front().c)
                {
                    c-=q.front().c;
                    ans+=q.front().c*q.front().x;
                    q.pop();
                }
                else
                {
                    ans+=c*q.front().x;
                    q.front().c-=c;
                    break;
                }
            }
            cout << ans << endl;
        }
    }
    return 0;
}
```

## Max Min(https://atcoder.jp/contests/abc247/tasks/abc247_e)

## 思路

这题有思路，但还是折腾了俩小时才交上……首先我们应该把原数组分段，可以发现当一段中存在大于X或小于Y的数，那么符合要求的段就不能包含这个数，也就是说我们可以把原数组分成若干个段，使得每一段中的数都大于等于Y小于等于X。但是分段不好分啊，我就是在分段上有bug，一直交不上，折腾了俩小时……在这里建议把每一段都放进一个vector数组中，计算完这一段之后再计算下一个段，如果先把原数组都分好段再一次性计算的话容易出错。在计算每一段时，采用滑动窗口法结合双指针法来计算，先开两个指针，初始时都指向0，再开两个cnt变量分别记录双指针包含的范围内X的个数以及Y的个数。开始时左指针不动，右指针右移，同时更新两个cnt变量的值，当两个cnt变量的值都不为0时，代表双指针这段范围内已经满足要求了，那么从右指针到这段的结尾的集合肯定也满足要求，一并加入到答案中。之后给左指针加一，注意左指针加一对两个cnt变量也有影响。之后重复执行以上过程直到左指针移动出段的最右端，这才是计算完了一段的答案，通过这样的计算把原数组跑完之后得到的答案才是最终的答案，时间复杂度O(2N)。

## 代码

```cpp#include <iostream>
#include <vector>
using namespace std;
#define N 200005
int n,x,y,ans,number[N];
vector <int> v;
long long cul()
{
    long long res=0;
    int pos1=0,pos2=0,cnt1=0,cnt2=0;
    while(pos1<v.size())
    {
        while(pos2<v.size()&&(!cnt1||!cnt2))
        {
            if(v[pos2]==x) cnt1++;
            if(v[pos2]==y) cnt2++;
            pos2++;
        }
        if(cnt1&&cnt2) res+=v.size()-pos2+1;
        if(v[pos1]==x) cnt1--;
        if(v[pos1]==y) cnt2--;
        pos1++;
    }
    return res;
}
int main()
{
    cin >> n >> x >> y;
    for(int i=1;i<=n;i++) cin >> number[i];
    int pos=1;
    while(pos<=n)
    {
        v.clear();
        while(pos<=n&&number[pos]>=y&&number[pos]<=x)
        {
            v.push_back(number[pos]);
            pos++;
        }
        if(!v.size()) pos++;
        else ans+=cul();
    }
    cout << ans << endl;
    return 0;
}
```

## Cards(https://atcoder.jp/contests/abc247/tasks/abc247_f)

## 思路

这题是一道图论与数学结合的题，整不明白，附上官方题解：https://atcoder.jp/contests/abc247/editorial/3773。

## 代码

```cpp
#include <iostream>
using namespace std;
#define N 200005
#define MOD 998244353
int f[N],u[N],v[N],vis[N];
int cul(int x)
{
    int cnt=0,rt=x;
    do
    {
        cnt++;
        vis[x]=1;
        x=v[x];
    }while(x!=rt);
    return cnt;
}
int main()
{
    int n;
    cin >> n;
    f[1]=1,f[2]=3;
    for(int i=3;i<=n;i++) f[i]=(f[i-2]+f[i-1])%MOD;
    for(int i=1;i<=n;i++)
    {
        int number;
        cin >> number;
        u[number]=i;
    }
    for(int i=1;i<=n;i++)
    {
        int number;
        cin >> number;
        v[i]=u[number];
    }
    long long ans=1;
    for(int i=1;i<=n;i++)
        if(!vis[i])
            ans=(ans*f[cul(i)])%MOD;
    cout << ans << endl;
    return 0;
}
```
