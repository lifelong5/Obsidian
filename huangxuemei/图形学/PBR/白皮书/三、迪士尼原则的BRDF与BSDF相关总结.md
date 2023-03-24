# 迪士尼对MERL材质数据库的观察结论

## Specular F项的观察结论：

- 菲涅尔反射系数表示了当入射光与试图矢量分开时镜面反射的增加
- 光滑表面在切线入射时有将近百分之百的镜面反射；粗糙表面无法达到百分之百但是反射率将会越来越高
- 每种材质在掠射角附近都显示出一些反射率的增加
- 掠射角入射附近的许多曲线的陡度已经大于菲涅尔效应的预测值

## Specular G项的观察结论：

- 几何项的影响可以间接地看作其对方向反射率的影响
- 大多数材质的方向反射率对于前70是相对平坦的，并且切线入射处的反射率与表面粗糙度密切相关
- 几何项的选择会对反射率产生影响，反过来又会对表面外观产生影响
- 如果忽略G项的话会存在掠射角处过暗的相应

![img](https://pic2.zhimg.com/80/v2-d3247efb9835b14ca59852f6b53655a5_720w.webp)

*光滑表面和粗糙表面*

## 布料材质的观察结论：

- 许多布料在掠射角处呈现出镜面反射的色调，并且还具有比非常粗糙的材质更强的菲涅尔波峰
- 布料具有有色的掠射反射，可以理解为是其轮廓附近获取到材质颜色的透射纤维造成的
- 布料在掠射角处的额外光泽增加，超出了普通微平面模型的预测范围

![img](https://pic3.zhimg.com/80/v2-8d816e1d26796c7ab5e05799ffbc1cc2_720w.webp)

## 彩虹色的观察结论：

- 变色涂料在(θh,θd)空间上显示出连续的色块，切对φd的依赖性最小。
- 彩虹色远离镜面峰值的反射率非常小，所以可以将彩虹色理解为一种镜面反射现象。
- 可以将镜面色调调制为θh和θd的函数，配合小尺寸纹理贴图对彩虹色进行建模。

![img](https://pic1.zhimg.com/80/v2-77f553b894e310e81bebb71c3cbeed74_720w.webp)

*上图：原始数据; 下图：通过每像素缩放1/ max(r, g, b)生成的相应色度图像。*

# 迪士尼原则的BRDF：

## 本质：

金属BRDF与非金属BRDF的线性混合

![img](https://pic3.zhimg.com/80/v2-879917eeb9c582dcd293b78cf39b8966_720w.webp)

*正因如此，在PBR的金属/粗糙度工作流中，固有色贴图才会同时包含金属和非金属的材质数据：1、金属的反射率值；2、非金属的漫反射颜色*

## BRDF参数：

- baseColor（固有色）：表面颜色，通常由纹理贴图提供。
- subsurface（次表面）：使用次表面近似控制漫反射形状。
- metallic（金属度）：金属（0 = 电介质，1 =金属）。这是两种不同模型之间的线性混合。金属模型没有漫反射成分，并且还具有等于基础色的着色入射镜面反射。
- specular（镜面反射强度）：入射镜面反射量。用于取代折射率。
- specularTint（镜面反射颜色）：对美术控制的让步，用于对基础色（basecolor）的入射镜面反射进行颜色控制。掠射镜面反射仍然是非彩色的。
- roughness（粗糙度）：表面粗糙度，控制漫反射和镜面反射。
- anisotropic（各向异性强度）：各向异性程度。用于控制镜面反射高光的纵横比。（0 =各向同性，1 =最大各向异性。）
- sheen（光泽度）：一种额外的掠射分量（grazing component），主要用于布料。
- sheenTint（光泽颜色）：对sheen（光泽度）的颜色控制。
- clearcoat（清漆强度）：有特殊用途的第二个镜面波瓣（specular lobe）。
- clearcoatGloss（清漆光泽度）：控制透明涂层光泽度，0 = “缎面（satin）”外观，1 = “光泽（gloss）”外观。

![img](https://pic2.zhimg.com/80/v2-8c18a8760406f32ff5a66be9aed14e91_720w.webp)

## BRDF着色模型：

### 核心BRDF模型：

通用的microfacet Cook-Torrance BRDF着色模型：

![image-20221103144000370](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221103144000370.png)

- diffuse为漫反射项
- 第二项为镜面反射项
  - D为微平面分布函数，主要负责镜面反射波峰的形状
  - F是菲涅尔反射系数
  - G微几何衰减/阴影项

#### 漫反射项diffuse：

##### Disney Diffuse：

使用了Schlick Fresnel近似，并修改掠射逆反射以达到其特定值由粗糙度确定，而不是简单为0。

![image-20221031150656314](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221031150656314.png)

其中：![image-20221031150914768](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221031150914768.png)

从公式中可以看出漫反射项受到粗糙度影响。

###### 代码：

```c++
float3 Diffuse_Burley_Disney(float3 DiffuseColor,float Roughness,float v,float l,float h){
    float FD90 = 0.5 +2*h*h*Roughness;
    float FDV=1+(FD90-1)*pow((1-v),5);//视角矢量
    float FDL=1+(FD90-1)*pow((1-l),5);//光线矢量
    return DiffuseColor*((1/PI)*FDV*FDL);
}
```

#### 镜面反射项：

##### 法线分布项：

###### [TR（Trowbridge-Reitz）/GGX](微表面法线分布函数--GXX)：![image-20221103144309733](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221103144309733.png)

- c为缩放常数
- 阿尔法为粗糙度参数，其值在【0，1】之间，0为完全光滑的分布，1为完全粗糙或均匀的分布

###### Berry：![image-20221103144817517](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221103144817517.png)

###### GTR：

对比TR与Berry对比得知，当r的系数越小产生的尾部越长，所以disney对r进行n次幂的推广，这就是GTR，Generalized-Trowbridge-Reitz

![image-20221103145521206](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221103145521206.png)

r为1即为TR，r为2的时候为Berry，不同的r有不同的效果，如下图

![img](https://pic1.zhimg.com/80/v2-97153e46022402eae28c35cff2115560_720w.webp)

###### 两个固定的镜面反射波瓣：

- 主波瓣：
  - 使用γ= 2的GTR（即TR）
  - 代表基础底层材质（Base Material）的反射
  - 可为各向同性或各项异性的金属或非金属
- 次级波瓣：
  - 使用γ= 1的GTR（即Berry）
  - 代表基础材质上的清漆层（ClearCoat Layer）的反射
  - 一般为各向同性的非金属材质，即清漆层

###### 代码：

- γ= 1

```c++
float D_GTR1(float alpha,float dotNH)
{
    float a2 = alpha * alpha;
    float cos2th = dotNH * dotNH;
    float den = (1.0 + (a2 - 1.0) * cos2th);

    return (a2 - 1.0) / (PI * log(a2) * den);
}
```

- γ= 2

```c++
float D_GTR2(float alpha, float dotNH)
{
    float a2 = alpha * alpha;
    float cos2th = dotNH * dotNH;
    float den = (1.0 + (a2 - 1.0) * cos2th);

    return a2 / (PI * den * den);
}
```

- 各项异性的版本

```c++
float D_GTR2_aniso(float dotHX, float dotHY, float dotNH, float ax, float ay)
    {
            float deno = dotHX * dotHX / (ax * ax) + dotHY * dotHY / (ay * ay) + dotNH * dotNH;
            return 1.0 / (PI * ax * ay * deno * deno);
    }
```

##### 菲涅尔项：

###### Schlick Fresnel：

![image-20221103170117025](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221103170117025.png)

- F0表示垂直入射时的镜面反射率
- θd为半矢量h和视线矢量v之间的夹角

###### 代码：

```c++
float3 F_Schlick(float HdotV, float3 F0)
{
    return F0 + (1 - F0) * pow(1 - HdotV , 5.0));
}
```

*在介质间相对IOR接近1的时候，近似的误差较大，建议直接使用准确的菲涅尔方程*

##### 几何项：

- 主波瓣

  使用GGX导出的G项，并将粗糙度进行重映射以减少光泽表面的极端，即α的值为(0.5 + roughness/2)^2。

  ![image-20221103170720813](C:\Users\huangxuemei\AppData\Roaming\Typora\typora-user-images\image-20221103170720813.png)

- 次级波瓣

​		直接采用粗糙度为0.25的GGX的G项，为一个定值，既可以达到比较好的效果。

###### 代码：

```
//主波瓣
//各项同性
float smithG_GGX(float NdotV, float alphaG)
{
    float a = alphaG * alphaG;
    float b = NdotV * NdotV;
    return 1 / (NdotV + sqrt(a + b - a * b));
}
//各项异性
float smithG_GGX_aniso(float dotVN, float dotVX, float dotVY, float ax, float ay)
{
	return 1.0 / (dotVN + sqrt(pow(dotVX * ax, 2.0) + pow(dotVY * ay, 2.0) + pow(dotVN, 2.0)));
}
//次级波瓣
float G_GGX(float dotVN, float alphag)
{
		float a = alphag * alphag;
		float b = dotVN * dotVN;
		return 1.0 / (dotVN + sqrt(a + b - a * b));
}
```

# 迪士尼原则的分层材质：

设计原则：所有参数都允许线性的插值，可以在不同材质之间进行线性插值混合得到复杂的外观。

![img](https://pic3.zhimg.com/80/v2-434dd8c9b5d550c588f62c3c4d48c8c2_720w.webp)

# 迪士尼的BRDF代码：

```c++
//参数
::begin parameters
color baseColor .82 .67 .16
float metallic 0 1 0
float subsurface 0 1 0
float specular 0 1 .5
float roughness 0 1 .5
float specularTint 0 1 0
float anisotropic 0 1 0
float sheen 0 1 0
float sheenTint 0 1 .5
float clearcoat 0 1 0
float clearcoatGloss 0 1 1
::end parameters

//shader代码
::begin shader

const float PI = 3.14159265358979323846;

float sqr(float x) { return x*x; }

float SchlickFresnel(float u)
{
    float m = clamp(1-u, 0, 1);
    float m2 = m*m;
    return m2*m2*m; // pow(m,5)
}

float GTR1(float NdotH, float a)
{
    if (a >= 1) return 1/PI;
    float a2 = a*a;
    float t = 1 + (a2-1)*NdotH*NdotH;
    return (a2-1) / (PI*log(a2)*t);
}

float GTR2(float NdotH, float a)
{
    float a2 = a*a;
    float t = 1 + (a2-1)*NdotH*NdotH;
    return a2 / (PI * t*t);
}

float GTR2_aniso(float NdotH, float HdotX, float HdotY, float ax, float ay)
{
    return 1 / (PI * ax*ay * sqr( sqr(HdotX/ax) + sqr(HdotY/ay) + NdotH*NdotH ));
}

float smithG_GGX(float NdotV, float alphaG)
{
    float a = alphaG*alphaG;
    float b = NdotV*NdotV;
    return 1 / (NdotV + sqrt(a + b - a*b));
}

float smithG_GGX_aniso(float NdotV, float VdotX, float VdotY, float ax, float ay)
{
    return 1 / (NdotV + sqrt( sqr(VdotX*ax) + sqr(VdotY*ay) + sqr(NdotV) ));
}

vec3 mon2lin(vec3 x)
{
    return vec3(pow(x[0], 2.2), pow(x[1], 2.2), pow(x[2], 2.2));
}


vec3 BRDF( vec3 L, vec3 V, vec3 N, vec3 X, vec3 Y )
{
    float NdotL = dot(N,L);
    float NdotV = dot(N,V);
    if (NdotL < 0 || NdotV < 0) return vec3(0);

    vec3 H = normalize(L+V);
    float NdotH = dot(N,H);
    float LdotH = dot(L,H);

    vec3 Cdlin = mon2lin(baseColor);
    float Cdlum = .3*Cdlin[0] + .6*Cdlin[1]  + .1*Cdlin[2]; // luminance approx.

    vec3 Ctint = Cdlum > 0 ? Cdlin/Cdlum : vec3(1); // normalize lum. to isolate hue+sat
    vec3 Cspec0 = mix(specular*.08*mix(vec3(1), Ctint, specularTint), Cdlin, metallic);
    vec3 Csheen = mix(vec3(1), Ctint, sheenTint);

    // Diffuse fresnel - go from 1 at normal incidence to .5 at grazing
    // and mix in diffuse retro-reflection based on roughness
    float FL = SchlickFresnel(NdotL), FV = SchlickFresnel(NdotV);
    float Fd90 = 0.5 + 2 * LdotH*LdotH * roughness;
    float Fd = mix(1.0, Fd90, FL) * mix(1.0, Fd90, FV);

    // Based on Hanrahan-Krueger brdf approximation of isotropic bssrdf
    // 1.25 scale is used to (roughly) preserve albedo
    // Fss90 used to "flatten" retroreflection based on roughness
    float Fss90 = LdotH*LdotH*roughness;
    float Fss = mix(1.0, Fss90, FL) * mix(1.0, Fss90, FV);
    float ss = 1.25 * (Fss * (1 / (NdotL + NdotV) - .5) + .5);

    // specular
    float aspect = sqrt(1-anisotropic*.9);
    float ax = max(.001, sqr(roughness)/aspect);
    float ay = max(.001, sqr(roughness)*aspect);
    float Ds = GTR2_aniso(NdotH, dot(H, X), dot(H, Y), ax, ay);
    float FH = SchlickFresnel(LdotH);
    vec3 Fs = mix(Cspec0, vec3(1), FH);
    float Gs;
    Gs  = smithG_GGX_aniso(NdotL, dot(L, X), dot(L, Y), ax, ay);
    Gs *= smithG_GGX_aniso(NdotV, dot(V, X), dot(V, Y), ax, ay);

    // sheen
    vec3 Fsheen = FH * sheen * Csheen;

    // clearcoat (ior = 1.5 -> F0 = 0.04)
    float Dr = GTR1(NdotH, mix(.1,.001,clearcoatGloss));
    float Fr = mix(.04, 1.0, FH);
    float Gr = smithG_GGX(NdotL, .25) * smithG_GGX(NdotV, .25);

    return ((1/PI) * mix(Fd, ss, subsurface)*Cdlin + Fsheen)
        * (1-metallic)
        + Gs*Fs*Ds + .25*clearcoat*Gr*Fr*Dr;
}

::end shader
```

# 迪士尼的BSDF：

## 设计思路

增加了一个新的参数specTrans（镜面反射透明度）用于混合BSDF和BRDF。

![img](https://pic2.zhimg.com/80/v2-16148202bf65d3c90f71452dc56d3449_720w.webp)

1. 镜面反射的BSDF与非金属的BRDF根据specTrans进行混合
2. 金属的BRDF与第一步得到的材质进行混合

## 参数：

### 普通表面：

新增specTrans（镜面反射透明度）、scatterDistance（散射距离）

### 薄表面：

新增specTrans（镜面反射透明度）、scatterDistance（散射距离）、flatness（平坦度）

## 新的次表面散射模型：

在BRDF中加入次表面散射模型：

1. 将漫反射波瓣重构为两部分：①方向性的微表面效应，主要为逆反射 ②非方向性的次表面效应Lambertian
2. 用散射模型或体积散射模型替换漫反射波瓣中的Lambert部分

这样，便能保留微表面效应（microsurface effect），让散射模型在散射距离较小时收敛到与漫反射BRDF相同的结果。

## 基于两个指数项总和的次表面漫射模拟模型

次表⾯漫射（Subsurface diffusion）。Disney通过蒙特卡洛模拟（Monte Carlo simulation），观察到对于典型的散射参数，包括单次散射的扩散剖面（diffusion profile），使用两个指数项的总和（a sum of two exponentials）便可以很好地进行模拟，且得到了比偶极子剖面（dipole diffusion）更好的渲染结果。如下图所示。

![img](https://pic1.zhimg.com/80/v2-848e3e3b70c769afe83233aec3fb3b8c_720w.webp)

## 薄表面BSDF：

对于薄的半透明表⾯，Disney选择在单个着色点处模拟入射和出射散射事件，作为镜面反射和漫反射传输的组合，由specTrans和diffTrans参数控制，并用各向同性的波瓣近似薄表面漫反射传输。