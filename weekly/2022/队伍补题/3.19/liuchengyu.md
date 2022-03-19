# 补题报告

## 活着的证据(https://ac.nowcoder.com/acm/contest/11178/A)

## 思路

明显的贪心题，但是如果考虑错了的话可能会WA很多遍。首先可以想到的是，当num1+num2<=n时，先输出num1个5，再输出num2个1，这肯定是没问题的。当num1+num2>n时，先算出能腾出来多少个1，计算公式为num=min(num2,num1+num2-n)，然后再计算有多少个有效的5，计算公式为num1=min(n,num1)，再算出有多少个有效的1，计算公式为num2-=num。之后就好贪心了，把多余的1按照贪心的思路分配给有效的5和1即可。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int _;
    scanf("%d",&_);
    while(_--)
    {
        int num1,num2,n;
        scanf("%d%d%d",&num1,&num2,&n);
        if(num1+num2<=n)
        {
            for(int i=1;i<=num1;i++) printf("5");
            for(int i=1;i<=num2;i++) printf("1");
            printf("\n");
        }
        else
        {
            int num=min(num2,num1+num2-n);
            num1=min(n,num1);
            num2-=num;
            while(num1--)
            {
                if(num>=3) {printf("8");num-=3;}
                else if(num>=2) {printf("7");num-=2;}
                else if(num>=1) {printf("6");num-=1;}
                else printf("5");
            }
            while(num2--)
            {
                if(num>=2) {printf("3");num-=2;}
                else if(num>=1) {printf("2");num-=1;}
                else printf("1");
            }
            printf("\n");
        }
    }
    return 0;
}
```

## 寻寻觅觅寻不到(https://ac.nowcoder.com/acm/contest/11178/B)

## 思路

这题的解法居然是暴力模拟判断是否满足要求，刚开始都没敢往这方面想，有一个优化的小细节很关键。首先当两个字符串长度不相等时肯定是"NO"，当两个字符串相等时肯定是"YES"，这是没毛病的。我们再来讨论除此之外的普通情况，我们从前往后遍历原串，每到一个字符就判断从这个字符开始切n个字符的到的字符串是否和待判断串的最后n个字符组成的字符串相同，并且原串除了切下来的这一部分剩下的两部分拼接在一起组成的新串是否和待判断串的除最后n个字符剩下的字符组成的字符串相同，如果两者都满足，那么满足条件，标记满足条件，退出循环。如果从当前字符开始切字符串不满足条件并且原字符串与待判断字符串在这个位置上的字符不相等，标记不满足条件，退出，这个优化的小细节很关键，如果不加上会WA，而且时间开销很大，这个优化的思想是要么从当前位切字符串满足条件，要么两位相等继续循环，否则就是不满足条件。

## 代码

```cpp
#include <iostream>
#include <string>
using namespace std;
int main()
{
    int _;
    cin >> _;
    while(_--)
    {
        string s1,s2;
        int n;
        cin >> s1 >> s2 >> n;
        if(s1.length()!=s2.length())
            cout << "NO" << endl;
        else
        {
            if(s1==s2)
                cout << "YES" << endl;
            else
            {
                int flag=0;
                for(int i=0;i<s1.length();i++)
                {
                    if(s1.substr(i,n)==s2.substr(s2.length()-n,n)&&s1.substr(0,i)+s1.substr(i+n,s1.length()-(i+n))==s2.substr(0,s2.length()-n))
                    {
                        flag=1;
                        break;
                    }
                    if(s1[i]!=s2[i]) break;
                }
                if(flag) cout << "YES" << endl;
                else cout << "NO" << endl;
            }
        }
    }
    return 0;
}
```

## 踩不出足迹(https://ac.nowcoder.com/acm/contest/11178/C)

## 思路

这题又是位运算，考察的是位运算的性质，以下双引号的内容出自官方题解：“考虑异或的性质，首先异或和同或都是具有交换律的，所以我们可以任意调换顺序。首先同或运算符就等价于和另外一个数取反异或起来。a⊗¬b=¬a⊗b,¬a⊗¬b=a⊗b。所以我们如果需要将若干个数取反后异或起来，可以把它们排在一起然后同时消掉这两个取反符号。”，也就是说，我们可以将所有的数都异或起来，然后输出这个值与这个值按位取反后的大者。注意按位取反后高位的0会变1，会影响最终的答案，所以我们要对这个按位取反后得到的数再进行位运算处理。注意最高位的符号位会影响答案，所以要用unsigned long long。

## 代码

```cpp
#include <iostream>
using namespace std;
int main()
{
    int n,k;
    cin >> n >> k;
    unsigned long long ans=0;
    while(n--)
    {
        unsigned long long number;
        cin >> number;
        ans^=number;
    }
    cout << max(ans,~ans<<64-k>>64-k) << endl;
    return 0;
}
```
