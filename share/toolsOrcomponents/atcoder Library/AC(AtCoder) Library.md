# AC(AtCoder) Library

[英文文档](https://atcoder.github.io/ac-library/production/document_en/index.html) 

## List

`#include <atcoder/all>` 可以使用库中的所有功能

### Data Structures

#### 1. segtree 线段树

##### 简单介绍

这是一种 `幺半群` 的数据结构, 可以简单理解为需要满足区间合并性质的一类数据结构

- 比如 一个区间的和可以划分为 左区间和以及右区间的和(乘积同理)
- 但是不满足如上性质的例子: 一个区间众数不等于 max(左区间众数数量, 右区间众数数量)

##### 构造函数 (创建一个线段树对象的方式) 

1. `segtree<S, op, e> seg(int n)`

   n 为线段树要维护区间的范围 

2. `segtree<S, op, e> seg(vector<S> v)`

   传入一个 `vector` 将 `vector` 中的元素作为线段树维护的区间

**通过 (1), (2) 两种方式之前需要提供的数据类型的定义:**

- `S` 的类型
- 提供合并规则的操作函数 `S op(S a, S b)`
- 每一个元素 `S` 的默认值

在提供完所需条件后, 一个元素类型为 `S` , 合并类型为 `op` 的线段树就建立好了 (如果是空树的话会从提前定义好的获取元素默认值的 `e()` 获取线段树的默认值)

**例子**: 建立一个查询区间长度为 10 的可以获取区间区间最小值的线段树

```cpp
//定义比较方法
int op(int a, int b) {
    return min(a, b);
}

//每一个元素都初始化为 INF (1e9)
int e() {
    return (int)(1e9);
}
//要维护 1 - 10 这个区间的最小值
segtree<int, op, e> seg(10);
```

- 对应上面第一种创建方式, 将会创建一个维护长度为 `n` 的数组. 所有元素的值都为初始化函数 `e()` 提供的值
- 对应上面第二种创建方式, 将会创建一个维护长度为 `vector.size()` 长度的数组, 并按照 `vector` 中的值来初始化整棵树

**线段树最多可维护序列长度为 ** `1e8`, **且维护的下标为** ` 0 -  n - 1`

**创建一颗线段树的时间复杂度为 O(n)**



##### set 方法 线段树单点修改

```cpp
void seg.set(int p, S x)
```

将  `a[p]`  设置为 X 

- 修改位置 `p` 应满足 $0$ $\le$ $p$  $\le$ $n$
- 时间复杂度为 O ($\log_{}n$)

##### get 方法 单点查询

```cpp
S seg.get(int p)
```

返回 `a[p]` 的值

-  `p` 应满足  `0 ≤ p  ≤ n`
-  时间复杂度为 O (${1}$)

##### prod 区间查询

```cpp
S seg.prod(int l, int r)
```

按照之前定义好的 `op` 来返回 `区间 [l, r - 1]` 的值 如果 `l == r` 的话会返回 `e()`

- `[l, r]` 应满足  `0 ≤ l ≤ r ≤ n`

- 时间复杂度为 O(log n) 

##### all_prod 最大区间查询

```cpp
S seg.all_prod()
```

 返回 `op(a[0], ..., a[n - 1])` 如果 树为空的话 则返回 `e()`

- 时间复杂度 O( n )

##### max_right

```cpp
(1) int seg.max_right<f>(int l)
(2) int seg.max_right<F>(int l, F f)
```

在线段树上使用二分搜索以获得满足以 `l` 为左区间且满足 `函数 f()` 的最长区间结束位置

- (1) 中需要满足函数 `f` 的返回值为 `bool` 且 `f` 是以及被定义的
- (2) 中需要满足 `F`  是必须要以 线段树的基本单位类型 `S` 作为参数 且返回的是 `bool` 值 的**函数对象**

**返回的右边界将满足**

-  `r == l 或 f(op(a[l], a[l + 1], ...,  a[r - 1])) =  true`

-  `r == n  或  f(op(a[l], a[l + 1], ..., a[r])) = false`

如果 `f` 是单调的(符合二分的前置条件)  则这是最大的 `r` 满足 `f(op(a[l], a[l + 1], ..., a[r - 1])) = true`.

**条件**

- 如果使用相同的参数调用 `f`，则返回相同的值，即 `f` 没有副作用.
- `f(e()) == true`  
- `0 ≤ r ≤ n`



##### min_left

```cpp
(1) int seg.min_left<f>(int r)
(2) int seg.min_left<F>(int r, F f)
```

同上



##### 练手题目  AC code of https://atcoder.jp/contests/practice2/tasks/practice2_j

```cpp
#include <atcoder/segtree>
#include <cstdio>
#include <vector>

using namespace std;
using namespace atcoder;

int op(int a, int b) { return max(a, b); }

int e() { return -1; }

int target;

bool f(int v) { return v < target; }

int main() {
    int n, q;
    scanf("%d %d", &n, &q);
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &(a[i]));
    }

    segtree<int, op, e> seg(a);

    for (int i = 0; i < q; i++) {
        int t;
        scanf("%d", &t);
        if (t == 1) {
            int x, v;
            scanf("%d %d", &x, &v);
            x--;
            seg.set(x, v);
        } else if (t == 2) {
            int l, r;
            scanf("%d %d", &l, &r);
            l--;
            printf("%d\n", seg.prod(l, r));
        } else if (t == 3) {
            int p;
            scanf("%d %d", &p, &target);
            p--;
            printf("%d\n", seg.max_right<f>(p) + 1);
        }
    }
}
```

