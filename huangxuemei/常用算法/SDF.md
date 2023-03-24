## SDF（sign distance function）：

符号距离函数，在空间中的一个有限区域上确定一个点到区域边界的距离并同时对距离的符号进行定义：点在区域边界内部为正，外部为负，位于边界上时为0。

## 基础2D图形得SDF：

### 圆形：

```c++
/*
圆形：圆心在原点，半径为r
*/
float sdCircle(vec2 p float r){
    //返回点与圆形的关系，在圆上为0，在圆外为负，内部为正
    return length(p)-r;
    //p到圆心的距离
}
```

### 线段：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200518172325975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMzY4MjQ3,size_1,color_FFFFFF,t_0#pic_center)

坐标点p离线段的距离，根据垂直线段顶点的直线划分为三个区域

- 区域1：过A点垂直与AB的点划分
  - 区域1的点p到线段最短的距离其实就是向量AP的距离|AP|或者|AP-AC|，其中AC为n*AB当n为0，即为0
- 区域2：在两条直线之间的区域
  - 区域2的点p到线段最短的距离其实是向量AP在向量AB上的投影，又叫|AP-AC|，其中AC为n*AB，n为【0-1】之间的值
- 区域3：过B点垂直与AB的点划分
  - 区域3的点p到线段最短的距离其实就是向量BP的距离|BP|或者是|AP-AC|，其中AC为n*AB当n为1，即为AB

```c++
/*
线段：a、b为线段的两个端点
*/
float sdSegment(in vec2 p,in vec2 a,in vec2 b){
    vec2 pa=p-a,ba=b-a;
    //clamp保证在区域1和区域2的计算当中，当AP在AB上的投影为负数时候，限制为0，当AP在AB上的投影大于AB时候，限制为1
    float h=clamp(dot(pa,ba)/dot(ba,ba),0.0,1.0);
    return length(pa-ba*h);
}
```

### 矩形：

![img](https://img-blog.csdnimg.cn/20200518181122627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMzY4MjQ3,size_1,color_FFFFFF,t_70#pic_center)

矩形有四个角，其实都是一致的所以就将四个角都转化到第一象限，并且用右上角的顶点来进行计算。

将第一象限分为四个区域：

- 区域1：点p到矩形的最短距离便是p到右上角顶点b的距离|pb|
- 区域2：点p到矩形的最短距离为p的y值，因为p的x值为负数
- 区域4：点p到矩形的最短距离为p的x值，因为p的y值为负数
- 区域3：点p到矩形的最短距离为p的x与y值中小的值

区域124：length(max(d,0.0))-->如果x或者y中有负值限制到0.0，也就实现了2和4区域的距离的计算

区域3：min(max(d.x,d.y),0.0)-->取x与y值中较大的负值也就是绝对值偏下的值，用min是为了使得距离为负数

```c++
/*
长方形：p为坐标点，b为矩形右上角的顶点
*/
float sdBox(in vec2 p,in vec2 b){
    vec2 d=abs(p)-b;
    return length(max(d,0.0))+min(max(d.x,d.y),0.0);
}
```

### 菱形：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051820122637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMzY4MjQ3,size_1,color_FFFFFF,t_70#pic_center)

和线段一致也是判断距离，但是需要增加一步就是需要判断点p的位置是在矩形内还是矩形外，利用叉乘来判断，并且用sign来获取方向，如果为负则为-1，如果为正则为1，如果在菱形上则为0；

```c++
/*
菱形：p为坐标点，b.x为菱形与x轴的交点，b.y为菱形与y轴的交点
*/
float sdRhombus( in vec2 p, in vec2 b)
{
    vec2 q=abs(p);
    vec2 a = vec2(0, b.y); 
    vec2 c = vec2(b.x, 0);
    vec2 pa = q-a, ba = c-a;
    float h = clamp( dot(pa,ba)/dot(ba,ba), 0.0, 1.0 );
    float d = length( pa - ba*h );
    //判断p与菱形的关系
    return d * sign( q.x*b.y + q.y*b.x - b.x*b.y );
}
```

