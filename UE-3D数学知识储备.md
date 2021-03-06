## 惯性坐标系

> 惯性坐标系的原点和物体坐标系原点重合，但惯性坐标系的轴平行于世界坐标系的轴。

- 引入意义

**从物体坐标系转换到惯性坐标系只需旋转，从惯性坐标系转换到世界坐标系只需平移。**

## 概念认知

零向量：各个维度均为零的向量。零向量没有方向。

负向量：一个向量各个维度取反得到的向量。几何上体现为负向量与原向量长度相等，方向相反。

标量乘向量，各个维度值乘上标量值。几何上体现为乘正值标量是沿正向“伸缩”，乘负值标量是方向反转并“伸缩”。

## 向量加减

向量加法的几何解释：使a的头连接b的尾，从a的尾到b的头的向量即是向量a+b。

向量减法的几何解释：使a和b的尾连接，从a的头到b的头就是向量b-a。

## 距离公式

各元相减的平方和再开根号。例如3D距离公式：

根号 (b~x~-a~x~)^2^ + (b~y~-a~y~)^2^ + (b~z~-a~z~)^2^

## 向量点乘，也常称为内积

向量点乘就是对应分量乘积的和，结果为一个标量。

几何解释，点乘结果描述了两向量的相似程度，点乘结果越大两向量越相近。

点乘等于向量大小与向量夹角的cos值的积。

点乘结果>0，说明两向量夹角在0~90°(左闭右开)之间

=0说明正交

<0说明夹角在90~180°(左开右闭)之间

点乘对零向量的解释是，零向量和任意其他向量都垂直。

## 向量投影

向量v在向量n上的投影v~||~ 

![image-20200118124124105](illustration/UE-3D%E6%95%B0%E5%AD%A6%E7%9F%A5%E8%AF%86%E5%82%A8%E5%A4%87/image-20200118124124105.png)

## 向量叉乘

点乘和叉乘在一起时，叉乘先计算。叉乘得到的向量垂直于原来的两个向量。叉乘公式为：

![image-20200118124354008](illustration/UE-3D%E6%95%B0%E5%AD%A6%E7%9F%A5%E8%AF%86%E5%82%A8%E5%A4%87/image-20200118124354008.png)

叉乘向量的模=两向量的模乘两向量夹角的正弦值。数值上等于以a和b为两边的平行四边形的面积。

**a和b叉乘后得到的向量的方法的判定方法：在左手坐标系中，依据左手螺旋定律，用手的四指从张开到卷起来的方向比对从a到b的方向，大拇指的指向即为叉乘向量的方向。**

## 矩阵变换的组合





镜像并不被认为是刚体变换。刚体变换也称为正规变换。

3D中，行列式等于以变换后的基向量为三边的平行六面体的有符号体积。



方位和方向不一样，一个向量自转，不会有任何变化，表示不出来“厚度”和“宽度”。一个物体自转后方位会产生变化。

描述物体方位时，不能使用绝对量。与位置只是相对已知点的位移一样，方位是通过于相对已知方位（通常称为“单位”方位或“源”方位）的旋转来描述的。旋转的量称作**角位移**。

用矩阵和四元数来表示角位移，用欧拉角来表示“方位”。

- [ ] 140