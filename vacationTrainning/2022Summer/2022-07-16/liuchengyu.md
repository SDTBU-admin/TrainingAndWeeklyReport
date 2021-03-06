# 补题报告

## 基本信息

队员一：刘骋羽

队员二：廖银

队员三：崔文贺

比赛链接：https://codeforces.com/gym/103448

## 做出来的题

### A 莫卡与 MCPC

题意：签到。

思路：根本不用跑素数筛，质数中只有2是唯一的偶数，所以2的时候输出OK，其它情况输出An invalid response was received from the upstream server。

### B bb 去食堂

题意：给一个矩形，再给一些点，判断哪个点距离这个矩形最近，如果有多个点满足的话输出下标小的那一个。

思路：分两种情况，一是点在矩形的正上方、正下方、正左方、正右方，这种情况直接计算即可；二是点在除上述情况外的地方，计算其到矩形的哪个角最近即可。答案是所有点中离矩形最近的那一个（如有相同取下标小的那一个），注意这题用double会有误差，结果是WA，直接用int存距离的平方就不会产生精度上的问题了。

### C 抵御阿草

题意：给你一个数n，然后你有一个0 ~ 2 ^ n - 1这样一个初始数组，给你一个n步的操作序列，0代表数组相邻两个数按位与，1代表数组相邻两个数按位或，2代表数组相邻两个数按位异或，问你进行完这n步操作后，结果是多少，答案用二进制表示。

思路：首先会先发现，每进行完一步操作之后，数组长度都会变为原来的一半，n步之后就只会剩下唯一一个数，也就是答案。再来看题目的数据，1e5级别的，根本没法暴力，像这种位运算的题一般都有规律，开启找规律之旅。经过不屑努力，会发现每次的按位与或者按位或都会确定最终答案的某一位，且按位与答案那一位就是0，按位或答案那一位就是1，例如第一次按位与，答案的倒数第一位就是0，第二次按位或，答案的倒数第二位就是1。这是其中一个规律，还有就是按位异或，只要出现异或，数组中的所有数都会变成一样的，且是1000000…………这种形式，例如，在第四次进行异或操作，答案就是1000。知道了上面的两个规律之后，直接模拟就行，这里还有两点要注意，一是如果操作中有两次及以上的异或操作时，答案一定是0；二是注意前导0，但是答案全部都是0的话还是要输出一个0的，只要注意到了这些细节一发AC没问题。

## 后面补的题

### K 皮卡丘与 Minimum Spanning Tree-I

题意：太麻烦了自己看题吧，反正都是中文题面。

思路：思路很直接，直接用kruskal构造最小生成树即可。将边存下来，将点也存下来看成拓展边，注意一旦某条边题目中给了，那么就不存在相应的拓展边了。将边以及点由小到大排序，边的话没啥说的，直接按kruskal跑最小生成树就行，点的话要关注一下，当我们需要用的某个点的拓展边时，不能一个点一个点地判断连通，这样的做法时间复杂度是O(n ^ 2)，会爆掉，正确的做法是此题的连通性除了要用并查集维护之外还要用set维护，set中存各个连通块中的点，当我们需要用到某个点的拓展边时，只需要跑与它不连通的点即可，发现一个连通一个，并及时更新并查集以及set，之后break即可，在这里可以用启发式合并。之后别忘了记录边权以及边连接的点，时间复杂度O(nlogn)。
