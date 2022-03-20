# 补题报告

## 题目[B-野比大雄的作业_牛客练习赛97（重现赛）@HHZP (nowcoder.com)](https://ac.nowcoder.com/acm/contest/30541/B)

## 思路

- 题目要求对区间[l,r]中所有数字与运算,加所有数字或运算后的和.并求出最大值.
- 两个数与运算后只可能有两种情况,要么不变,要么变小.而两个数或运算会有三种情况:不变,变小,变大.其中当两个数或完变大时,一定是只有一个1的情况,这种情况下增加的部分在与运算后会清零,其和一定不会变大.
- 题目没有要求区间的长度,也即区间可以为一,所以问题就转化为找数列中最大的数*2.

## 代码

```c++
#include<map>
#include<vector>
#include<iostream>
#include<algorithm>
using namespace std;
#define ll long long 
const int array_size = (int)2e5 + 50;
int a[array_size], b[array_size];
int main()
{
	int n,maxnum=0,k;
	cin >> n;
	for (int i = 0; i < n; i++)
		cin >> k, maxnum = max(maxnum, k);
	cout << maxnum * 2;
	return 0;
}
```

## 题目[C-哦~唔西迪西小姐~_牛客练习赛97（重现赛）@HHZP (nowcoder.com)](https://ac.nowcoder.com/acm/contest/30541/C)

## 思路

- 题目要求在两种共n块地板上跳动,在此之前可以有m次操作.
- 跳动只能在一种地板上,所以题目可以分为两种情况:在冰上跳动或在火上跳动,最后取最大值即可.
- 而跳动时有两种状态,不需要改状态和需要改状态的.
- 对于不需要改状态的:如果ai的值大于0,显然我们就加上.但是由于pi可以小于0,就会造成-pi+a[i]反而比更改某些需要改状态的地板分更高.所以我们就将其放入队列.
- 对于需要改状态的地板,我们直接将其放入地板即可.
- 最后我们只需要加上优先队列中前m个且大于零的数并返回即可.

## 代码

```c++
#include<map>
#include<queue>
#include<vector>
#include<cstring>
#include<iostream>
#include<algorithm>
using namespace std;
#define ll long long 
const int array_size = (int)1e5 + 50;
int n,m,a[array_size], b[array_size],c[array_size];
//有的格子损失分为负，更改类型后反而可能加分，这不仅仅局限于不同类型的格子，即便同类型的格子也可以通过更改
//类型来获得更高的分数
ll bing()//跳到冰上
{
	ll ans = 0;
	priority_queue<int>p;
	for (int i = 0; i < n; ++i)
	{
		if (!c[i])
		{
			p.push(-b[i] - max(0, a[i]));
			if (a[i] > 0)
				ans += a[i];
		}
		else p.push(max(-b[i], ( - b[i] + a[i])));
	}
	int j = 0;
	while (p.size() > 0)
	{
		if (j == m||p.top()<=0) break;
		int k = p.top();
		p.pop();
		ans += k;
		j++;
	}
	return ans;
}
ll huo()
{
	ll ans = 0;
	priority_queue<int>p;
	for (int i = 0; i < n; ++i)
	{
		if (c[i])
		{
			p.push(-b[i] - max(0,a[i]));
			if (a[i] > 0)
				ans += a[i];
		}
		else p.push(max(-b[i],(-b[i] + a[i])));
	}
	int j = 0;
	while (p.size() > 0)
	{
		if (j == m || p.top() <= 0) break;
		int k = p.top();
		p.pop();
		ans += k;
		j++;
	}
	return ans;
}
int main()
{
	cin >> n >> m;
	for (int i = 0; i < n; ++i)
		cin >> a[i];
	for (int i = 0; i < n; i++)
		cin >> b[i];
	for (int i = 0; i < n; i++)
		cin >> c[i];
	cout << max(bing(), huo());
	return 0;
}
```

