# 补提报告

## 队员
队员一:刘统誉\
队员二:王成魁\
队员三:刘敏\
比赛链接:https://vjudge.csgrandeur.cn/contest/503918
## 完成的题
  J - JavaScript \
  题意:如果字符串里有字母就输出"NaN",如果两个字符串都是数字,输出两字符串的相减的结果\
  思路:见题意\
  A - Olympic Ranking\ 
  题意:给各个参赛队的金牌,银牌,铜牌数,以及名称,问谁老大;\
  思路:sort排序,先看金再看银,最后看铜.需要注意的是名称里有空格,可以用gets取,而且名称还很大,第一次开了100,没想到最后超时了,\
  后来试了试,开大开小都可能会使codeblock停工,结果是开小了....\
  B - Aliquot Sum\
  题意:求一个数的除自己以外的因子和与自己的比较结果\
  思路:给了8s,直接暴力解,第二个循环到根号n就行.\
  D - Drunk Passenger\
  题意:有n位人,原来大家都是对号入座的,有一个人喝醉了,他随机选取一个lucky dog坐他的位置(幸运儿当然不是他自己),\
  于是大家都随便做了,求你丢掉你的位置的概率.\
  思路:三人时概率为1/2(1+1/2),四人时概率为1/3(1+1/2+1/2),五人时为1/4(1+3/2),综上n人时为1/(n-1)*(1+(n-1)*(1/2));
##补题
  C - A Sorting Problem\
  题意:给一个整数数组p[],可以让相差为一的两个数交换位置,问最少几步使p[i]=i;
  思路:王成魁同学发现这是逆序对,但是我们一开始不会缩减时间,用遍历n^2就在text6超了.\
  后来学哥讲了一个set容器它的find()找一个数的迭代位置只要logn,我以为可以做了,结果发现set的迭代只能用"++"和"--",所以只能重头一个一个加,可想而知,\
  又超时了,离谱的是这次到text5就T了!!!
  哎,还是乖乖用归并排序吧~.~
