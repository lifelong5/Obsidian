### 基于物理的渲染：

#### BRDF（双向反射分布函数）：

可以用如下来表示，表示入射方向I到达表面某点时候，有多少的能量被反射到了v上。

<img src="C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20220628191809454.png" alt="image-20220628191809454" style="zoom:200%;" />

##### 组成：

###### 高光反射项：

- **F(i,h)是菲涅尔项，表示有多少的光线反射，**

- **D(h)**中D是法线的分布情况，D(h)表示的就是在法线的分布中，寻找有多少光线的法线是与中间法线差不多的（**因为只有法线等于中间向量的光线，才能沿着反射方向射出**）
- **G(i,v,h)**，有些光线沿着比较平行与表面的方向入射，那么光线会被微表面所遮挡而无法到达后面的一些微表面，而产生自遮挡的情况，微表面自己会产生部分阴影，也就是某些微平面失去了它的作用，表示没有被遮挡的部分

![image-20220628193330657](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20220628193330657.png)

###### 漫反射项：

兰伯特BRDF：

![image-20220628192934082](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20220628192934082.png)

入射方向I的光源在表面某点的漫反射率为

![image-20220628193057591](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20220628193057591.png)

#### 反射等式：

入射光的能量乘以BRDF表示从某个方向入射的光线，从v方向反射了多少能量出来。

而用积分则表示，所有入射光线在v方向上反射的能量的总和。

![image-20220628191948205](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20220628191948205.png)