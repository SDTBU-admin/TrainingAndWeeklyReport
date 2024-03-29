# 极角排序详解<br>

----
## 名词释意：

-------
在平面内取一个定点O，叫极点，引一条射线Ox，叫做极轴，再选定一个长度单位和角度的正方向（通常取逆时针方向）。对于平面内任何一点M，
用ρ表示线段OM的长度（有时也用r表示），θ表示从Ox到OM的角度，ρ叫做点M的极径，θ叫做点M的极角，有序数对 (ρ,θ)就叫点M的极坐标，
## 四种极角排序代码详解：

------
```cpp
struct point
{
    double x,y;
};

double cross(double x1,double y1,double x2,double y2)
{
    return (x1*y2-x2*y1);
}

double compare(point a,point b,point c)
{
    return cross((b.x-a.x),(b.y-a.y),(c.x-a.x),(c.y-a.y));
}
```
## １、 利用complex类按极角从小到大排序

------
```cpp
bool cmp0(const point& a, const point& b) // 利用complex类按极角从小到大排序
{
    complex<double> c1(a.x,a.y);                   //头文件 #include<complex>
    complex<double> c2(b.x,b.y);
    if( arg(c1) == arg(c2))
        return a.x<b.x;
    return arg(c1) < arg(c2);
}
```
## 2、　利用atan2（）函数按极角从小到大排序

-----
```cpp
bool cmp1(point a,point b)//利用atan2（）函数按极角从小到大排序
{
    if(atan2(a.y,a.x)!=atan2(b.y,b.x))
        return atan2(a.y,a.x)<atan2(b.y,b.x);
    else return a.x<b.x;
}
```
## 3、　利用叉积按极角从小到大排序

----
```cpp
bool cmp2(point a,point b) //利用叉积按极角从小到大排序
{
    point c;//原点
    c.x = 0;
    c.y = 0;
    if(compare(c,a,b)==0)
        return a.x<b.x;
    else return compare(c,a,b)>0;
}
```
## 4、　先按象限从小到大排序 再按极角从小到大排序

------
```cpp
int Quadrant(point a)
{
    if(a.x>0&&a.y>=0)  return 1;
    if(a.x<=0&&a.y>0)  return 2;
    if(a.x<0&&a.y<=0)  return 3;
    if(a.x>=0&&a.y<0)  return 4;
}
bool cmp3(point a,point b)  //先按象限从小到大排序 再按极角从小到大排序
{
    if(Quadrant(a)==Quadrant(b))
        return cmp1(a,b);
    else Quadrant(a)<Quadrant(b);
}

```

本文为个人随笔,如有不当之处,望各位大佬多多指教. 若能为各位提供小小帮助,不胜荣幸.
