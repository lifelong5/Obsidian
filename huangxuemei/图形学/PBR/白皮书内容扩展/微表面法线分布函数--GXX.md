**用于渲染表面粗糙的半透明物体，同时也适用于表面粗糙的不透明物体。**

#### 效果图：

![img](https://pic1.zhimg.com/80/v2-839904cc92e89ce04fbc688b8c22f94c_720w.webp)

在渲染既有反射又有透射的情况时，我们通常使用BSDF来模拟。

#### 使用的BSDF：

BSDF=BRDF+BTDF

![img](https://pic3.zhimg.com/80/v2-fdf9fee8defe691ed3a6221b111f6f52_720w.webp)

- i为入射光方向
- o为出射光方向
- n为法线

##### BRDF：

采用了Cook-Torrance 模型的形式：

![img](https://pic3.zhimg.com/80/v2-ec749fc17e81be03e2c7e8879670ba46_720w.webp)

- hr为中间向量，入射光线与出射光线。

##### BTDF：

从BRDF推广得到，定义了折射情况下的hr，给出两边介质的相对密度ηi 和 ηo，hr为：

![img](https://pic4.zhimg.com/80/v2-7a951a665767a0ade2ac9817b98bc443_720w.webp)

推导出的公式为：

![img](https://pic2.zhimg.com/80/v2-a79986ca3d2bb4f60dd3315b23efad9d_720w.webp)

#### BTDF公式项介绍：

#### F菲涅尔项：

采用**Cook-Torrance 模型中类似的菲涅尔项表达式**，c =|i ∙ n|，g被定义为与介质密度相关的函数。

![img](https://pic1.zhimg.com/80/v2-5041e975de4c7fc390becacac4e083dc_720w.webp)

![img](https://pic3.zhimg.com/80/v2-369dbf5114c3167d286a9df2636c9c66_720w.webp)

当内部全反射发生时候，F=1；

#### D微表面法线方向函数项：

**m为微表面的法线，n为宏观法线**

![img](https://pic4.zhimg.com/80/v2-c9fad7abc5c2aaeaf37f007252b5c403_720w.webp)

GGX分布函数：

-  ν 表示任意方向，视角方向
- θm 表示 m 与n 之间的夹角
- θv 表示 ν 与 n 之间夹角
- αρ 表示控制变量
- Χ+ （α）表示阶跃函数，当 α > 0 时取值 1，当 α ≤ 0 取值 0。

![img](https://pic3.zhimg.com/80/v2-b0f37ce39cd2a75ca0bb22fa40d29cae_720w.webp)

**Beckmann、Phong以及 GGX 分布函数以及阴影遮挡函数在不同控制变量取值时的曲线**，可以看出，GGX的拖尾更长更窄。

![img](https://pic2.zhimg.com/80/v2-259ee9f0a427393a62cfa2fb7c060ab5_720w.webp)

#### G阴影遮挡项：

采用**Smith shadowing-masking**模型：

![img](https://pic3.zhimg.com/80/v2-5474ca1d2699321514e8ce145457c0da_720w.webp)

G1是根据微表面法线分布函数推导出来。