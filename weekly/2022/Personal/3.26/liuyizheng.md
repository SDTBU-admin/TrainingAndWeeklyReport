## 补题报告

#### [C - Collision 2]

**链接：**

https://vjudge.csgrandeur.cn/contest/485267#problem/C

**题意：**

xy平面上有N个人，第i个人在（X_i，Y_i）。所有人的位置都不同。

我们有一个长度为N的字符串S，由L和R组成。

如果S_i=R，则表示第i个人面向右；如果S_i=L，则表示第i个人面向左。所有人同时开始朝着他们所面对的方向行走。这里，右边和左边分别对应于正x方向和负x方向。

我们说当两个方向相反的人走到同一个位置时会发生碰撞。如果所有人继续无限期地行走，会不会发生碰撞？

如果发生碰撞，请打印“是”；否则，请打印“否”。

**思路：**

利用排序将纵坐标相同的点按横坐标大小升序排列，纵坐标不同的点按纵坐标大小降序排列，分成一个个纵坐标相同的模块

对每一个模块都找方向不同的两点，找到则Yes，都找不到则No

**代码：**

```c++
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <algorithm>

using namespace std;

struct Node
{
    int x;
    int y;
    int z;//表方向
}node[200005];

//纵坐标相同按横坐标升序表示，否则按纵坐标降序表示
bool cmp(Node a,Node b)
{
    if(a.y==b.y)
        return a.x<b.x;
    return a.y>b.y;
}

int main()
{
    int n,i,j;
    string s;
    cin >> n;
    for(i=0;i<n;i++)
        cin >> node[i].x >> node[i].y;
    cin >> s;
    for(i=0;i<n;i++)
    {   
        //向右走将z赋为1，向左赋为-1
        if(s[i]=='R')
            node[i].z=1;
        else
            node[i].z=-1;
    }
    sort(node,node+n,cmp);
    int f=0;
    for(i=0;i<n;i++)
    {
        //如果出现向右平移的点，将f赋为1
        if(node[i].z==1)
            f=1;
        //若向右向左的点都发现了，则纵坐标相同方向相反的两点必然相遇，输出Yes
        if(f!=0&&node[i].z==-1)
        {
            cout << "Yes" << endl;
            return 0;
        }
        //若两点纵坐标不同，则重新将f赋为0，在下一组纵坐标相同的点中找方向不同的两点
        if(node[i+1].y!=node[i].y)
            f=0;
    }
    //任一组都没找到方向不同的两点（即纵坐标相同的点没有方向不同的）
    cout << "No" << endl;
    return 0;
}
```

#### [D - Moves on Binary Tree]

**链接：**

https://vjudge.csgrandeur.cn/contest/485267#problem/D

**题意：**

有一个完美的二叉树，有2^(10^100)-1个顶点，编号为1,2，。。。，2^{(10^100)-1.

顶点1是根。区间1≤ i<2^(10^100)-1的每个顶点i都有两个子节点：左边的顶点 2i 和右边的顶点 2i+1。

高桥从顶点X开始，执行N次移动，用字符串S表示。第 i 次移动如下：

如果S的第i个字符是U，则转到他现在所在顶点的父级。

如果S的第i个字符是L，则转到他现在所在顶点的左子级。

如果S的第i个字符是R，则转到他现在所在顶点的右子级。

查找顶点的索引，高桥将在N次移动后打开。在给定的情况下，可以保证答案最多为10^{18}。

**思路：**

注意转节点的时候节点是否超过限制，根据二叉树序号特征，转到左就是x乘2，转到右是x乘2+1，转到父节点就是除以2；

另，注意转节点的途中有没有超限

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
    long long n,x,i,j;
    string s;
    cin >> n >> x;
    cin >> s;
    for(i=0;s[i]!=0;i++)
    {
        //转到父节点
        if(s[i]=='U')
        {
            if(x==1)
                continue;
            else
                x=x/2;
            continue;
        }
        //转到当前节点的左子树
        if(s[i]=='L'&&(x*2-1)<=1e18)
        {
            x*=2;
            continue;
        }
        //转到当前节点的右子树
        if(s[i]=='R'&&(x*2+1)<=1e18)
        {
            x=x*2+1;
            continue;
        }
        int cnt=0;
        for(j=i;j<n;j++)
        {
            if(s[j]=='U')
                cnt++;
            else
                cnt--;
            if(cnt==0||j==n-1)
            {
                i=j;
                break;
            }
        }
    }
    cout << x << endl;
    return 0;
}
```

