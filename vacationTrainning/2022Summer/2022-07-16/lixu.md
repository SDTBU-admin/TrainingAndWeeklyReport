 补题报告
## 基本信息

 - 队员一：李旭
 
 - 队员二：张乐康
 
 - 队员三：孙祥恒
 
 - 比赛链接：https://vjudge.csgrandeur.cn/contest/505066
## 做出来的题
A - 莫卡与 MCPC
 - 题意：判断一个数：即使偶数又是素数
 
 - 思路：即使偶数又是素数的就2一个，判断输出即ac
 
B - bb 去食堂
 - 题意：给出一个矩形坐标范围，然后给出一些坐标点，判断那个点距离这个矩形最近，然后输出标记

 - 思路：分出几个情况，给出的坐标x轴（或y轴）坐标在矩形内，只算y轴（或x轴）距离就行，其余情况算出离距离坐标最近的那个点距离，输出最小距离的下标
## 后面补的题
C - 抵御阿草
 - 题意：一开时有2^n个数，从0到2^n-1排好，每进行一次操作，就将两个数合成，进行n次后，输出最后结果的二进制

 - 思路： 如果给的字符串中有大于等于2个n的话输出0，0个2时其将字符串倒置输出不包含前导0就可，1个2时将第一个
2变成1，然后继续倒置输出即可
