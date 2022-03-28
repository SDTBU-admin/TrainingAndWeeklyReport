# 补题报告

## 题目[A-重新排列_牛客练习赛70（重现赛）@HHZP (nowcoder.com)](https://ac.nowcoder.com/acm/contest/30866/A)

## 思路

- 一直维护一个尾部符合条件的区间，不断对头部剪枝。

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
string k = "puleyaknoi";
map<char, int >p;
bool cherk()
{
	for (int i = 0; i < k.size(); ++i)
	{
	//如果map中没有这个元素，则关键字的值为0，但有时候关键字的值为0不能说明是否存在该元素时时，
	// 则需要用到如下语句:
	// auto t=p.find(k[i]);
	// if(t==p.end())
	//     cout<<"不存在该元素"<<endl;
	//
    //或者我们可以使用map的成员函数
    //例如我们定义map<int ,int >k;
    //若k[i].count为零，则说明map中不存在关键值i    
		if (p[k[i]] == 0)
			return false;
	}
	return true;
}
int main()
{
	int a;
	cin >> a;
	while (a--)
	{	
		string s;
		cin >> s;
		p.clear();//map类型需要用clear函数清零
		int l = 0, ans = 0x3f3f3f3f;
		for (int i = 0; i < s.size(); ++i)
		{
			p[s[i]]++;
			while (cherk())
			{
				ans = min(ans, i - l + 1);
				p[s[l]]--;
				l++;
			}
		}
		if (ans <= s.size()) cout << ans<<endl;
		else cout << -1 << endl;
	}
	return 0;
}
```

## 题目[B-拼凑_牛客练习赛70（重现赛）@HHZP (nowcoder.com)](https://ac.nowcoder.com/acm/contest/30866/B)

## 思路

- 针对每一个'p'逐步dp更新模式串,其后所有出现模式串字符都会随前面的'p'而更新,所更新时记录的是最近一个以'p'起始模式串的位置.
- 所有dp[i] [j]都可以由dp[i-1] [j-1]得到:如果不属于模式串的字符,直接继承;如果属于,则看是否属于‘p’或者'i'如果是p就更新,如果是i则看是否是一直继承过来的.

```c++
#include<map>
#include<queue>
#include<string>
#include<vector>
#include<cstring>//使用to_string必须加该头文件
#include<iostream>
#include<algorithm>
using namespace std;
#define ll long long 
const int array_size = (int)1e5 + 50;
string s, mould_s = "puleyaknoi";
int dp[array_size][11];//dp[i][j]表示在第i个字符处与模式串j字符匹配时的位置
map<char, int>p;
int slove()
{
	int ans = 0x3f3f3f3f;
	for (int i = 1; i < s.size(); ++i)
	{
		for (int j = 0; j < 10; ++j) dp[i][j] = dp[i - 1][j];//首先继承前一次dp的结果
		if (!p.count(s[i])) continue;//如果不是模式串字符之子，跳过
		int k = p[s[i]];
		if (!k) dp[i][0] = i;//所匹配的字符是'p'则重置dp[i][j]的位置
		else dp[i][k] = dp[i - 1][k - 1];//重点在于j-1上，继承模式串中该字符位置上一个字符在s串中出现的位置
		if (k == 9 && dp[i][k]) ans = min(ans, i - dp[i][k] + 1);//假如匹配到的字符是'k'并且模式串所有字符"依次"出现过
		//更新ans时要用dp[i][k]是因为dp[i][0]中存的是最近一次'p'出现的位置,而dp[i][9]中存的是最近一次完整模式串出现的位置
	}
	return ans < 0x3f3f3f3f ? ans : -1;
}
int main()
{
	for (int i = 0; i < 10; ++i)
		p[mould_s[i]] = i;
	int n;
	cin >> n;
	while (n--)
	{
		s.clear();
		memset(dp, 0, sizeof(dp));
		cin >> s;
		s = " " + s;
		cout << slove() << endl;;
	}
	return 0;
}
```
