---
layout:     post
title:      Ashikhmin-Shirley 光照模型
subtitle:   
date:       2018-06-30
author:     JPeng
header-img: img/Ashikhmin-Shirley01.png
catalog: true
tags:
    - LightModel
---


# Ashikhmin-Shirley
  各向异性模型。用来表现一些特殊的金属材质。相比其他的模型，可以更好的控制高光的形状。虽然不是基于物理的模型，但是使用了Fresnel函数来获得更真实的反射系数。同时Diffuse项也脱离了Labertian模型，保证了能量守恒，效果还是很不错的，不过计算代价确实很大。如果要用到游戏里面，肯定要做一些简化。
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley02)
下面是原始论文里面的不同的nu,nv值的效果：
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley03)
当nu=nv时，A.S模型就退化成了各向同性的模型，下面是原始论文里面的效果比较：

![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley04)

![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley05)
注意Diffuse项现在是和视点相关的，Fresnel的加入使得在不同的角度，Diffuse和Specular的比例有所区别，Fresnel采用的是Schlick的简化函数：

![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley06)
加入Fresnel的效果，看下面这张原始论文的图就很直观了：
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley07)
下面是我的代码实现与论文中的现象相同 
Nu= 10；Nv= 10；            Nu = 10 ； Nv= 100        Nu = 10 ； Nv= 1000          Nu = 10 ； Nv= 10000

![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley08)
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley09)
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley10)
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley11)
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley12)
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley13)
![images](https://github.com/liujiapeng550/liujiapeng550.github.io/tree/master/img/Ashikhmin-Shirley14)
