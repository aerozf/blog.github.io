---
layout:     post
title:      "计算传热学第一次大作业记录"
subtitle:   " \" numerical heat transfer\""
date:       2020-4-29 23:01:00
author:     "张凡"
header-img: "https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/post.jpg"
catalog: true
tags:
    - 计算传热学
    - 作业
---

# 计算传热学第一次大作业

## 问题描述

- **L=0.1m，k=10W/(m·K)，四周绝热**

  - 1、左侧面T0=400K。

  - 2、右侧面x=L，h=100W/(m^2·K)，Tf=300K。
  - 3、内热源S=250(300-Tx)。

- **求解温度场**

- **要求：**
  - 1、网格数大于7，至少计算三套网格数不同的网格。
  - 2、使用TDMA和G-S分别求解，比较计算结果和时间。
  - 3、提交源程序和报告，报告中包含离散方程推导过程，边界条件处理过程，以及不同数目的网格计算结果对比，TDMA和G-S计算结果对比。

 

## 一、离散方程推导过程

### 1.1、数学模型

​	该问题属于含源项的一维稳态导热问题，常物性。

​	其导热微分方程为：![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps1.png)

​	单值性条件：x=0，T0=400K；x=L，Tf=300K，h=物理四周绝热。

### 1.2、计算区域离散化

**前提假设：**在本计算中计算区域采用网格均匀划分，设有n个网格，则网格间距均为：![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps2.png)。

由课堂知识可知，要求解该问题，需要把计算区域离散化，化为代数方程组，求解方程就可以算得温度场分布。

 	![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps37.png)

​		如上图所示，把导热问题分为n个内部节点，两个边界节点，对于每个节点，使用统一的计算格式来计算节点温度，即对于每个节点构成如下方程：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps3.png)

**右边界节点：**

​		对于右边界节点，给定了边界条件T0=400K，带入如下边界条件统一格式中：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps4.png)

​		在以上统一格式中，A=1，B=0，C=T0。
​	
​		右边界的通用计算格式为：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps5.png)

​		在本问题中，![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps6.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps7.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps8.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps9.png)
​	
​		将所有已知条件带入右边界条件通用计算式中，得到：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps10.png)

**内部节点：**

​		对于内部节点，直接按照以下公式计算就行了。即：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps11.png)

​		其中：![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps12.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps13.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps14.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps15.png)
​	
​		把其化为 ![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps16.png) 这种格式，带入上面已知的数据，即为：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps17.png)

**左边界节点：**

​		对于左边界节点，给定了边界条件Tf=300K，h=100W/(m^2·K)，带入如下边界条件统一格式中：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps18.png)

​		在以上统一格式中，A=h，B=k，C=![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps19.png)。
​	
​		右边界的通用计算格式为：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps20.png)

​		在本问题中，![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps21.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps22.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps23.png)、![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps24.png)
​	
​		将所有已知条件带入右边界条件通用计算式中，得到：

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps25.png)

​		最后将边界节点、内部节点中对应于方程![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps26.png)中的a,b,c,d这四个参数提取出来，组成如下图所示的计算矩阵，最后求解出各节点的温度，从而求得物体当中温度场的分布。
​	
![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps27.jpg) 

## **二、**计算结果及分析

### 2.1 使用TDMA算法求解节点方程

#### 2.1.1、10个节点

**温度场计算结果：**396.80175741571884, 390.64727664069574, 384.7194140572742, 379.00335000899577, 373.48479433573976, 368.14995064832306, 362.9854818375272, 357.97847673132503, 353.1164178169512, 348.38714994711967

**耗时：**0.0秒（由于计算时间特别短，显示为0秒）

**温度分布图：**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps28.jpg) 

#### 2.1.2、50个节点

**温度场计算结果：**399.3602392560424, 398.0906537920529, 396.8308773934427, 395.58078408257177, 394.3402488501091, 393.10914764253147, 391.88735734971823, 390.6747557926399, 389.471221711141, 388.2766347518132, 387.0908754559606, 385.9138252476537, 384.74536642187155, 383.58538213273164, 382.43375638180504, 381.2903740065167, 380.1551206686291, 379.02788284280837, 377.90854780527195, 376.7970036225161, 375.6931391401225, 374.59684397164295, 373.50800848756063, 372.4265238043271, 371.352281773474, 370.2851749707983, 369.22509668561975, 368.17194091010975, 367.1256023286908, 366.0859763075048, 365.0529588839496, 364.0264467562828, 363.0063372732917, 361.992528424028, 360.98491882760675, 359.9834077230683, 358.9878949593022, 357.9982809850322, 357.01446683886064, 356.0363541393731, 355.0638450752994, 354.09684239573335, 353.1352494004069, 352.17896993002057, 351.2279083566273, 350.2819695740698, 349.3410589884697, 348.4050825087685, 347.4739465373182, 346.5475579605217

**耗时：**0.0秒（由于计算时间特别短，显示为0秒）

**温度分布图：**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps29.jpg) 

#### 2.1.3、100个节点

**温度场计算结果：**399.6801178736887, 399.042845624013, 398.40804944547796, 397.7757134681791, 397.14582188371685, 396.51835894480166, 395.8933089648602, 395.27065631764276, 394.6503854368333, 394.03248081565977, 393.4169270065067, 392.8037086205288, 392.19281032726667, 391.5842168542627, 390.97791298668017, 390.3738835669223, 389.7721134942537, 389.1725877244225, 388.57529126928455, 387.9802091964284, 387.3873266288022, 386.7966287443417, 386.2081007756, 385.6217280093777, 385.0374957863557, 384.45538950072836, 383.8753945998386, 383.2974965838139, 382.7216810052037, 382.14793346861876, 381.57623963037065, 381.00658519811327, 380.438955930486, 379.8733376367569, 379.3097161764689, 378.7480774590853, 378.18840744363825, 377.6306921383773, 377.07491760041984, 376.5210699354024, 375.9691352971334, 375.41909988724683, 374.87094995485745, 374.32467179621693, 373.7802517543715, 373.2376762188199, 372.6969316251738, 372.1580044548185, 371.62088123457465, 371.0855485363617, 370.5519929768623, 370.0202012171874, 369.4901599625428, 368.96185596189736, 368.435276007651, 367.9104069353049, 367.3872356231322, 366.8657489918502, 366.34593400429304, 365.8277776650861, 365.3112670203208, 364.7963891572311, 364.28313120387037, 363.7714803287898, 363.2614237407174, 362.7529486882387, 362.24604245947717, 361.7406923817772, 361.23688582138675, 360.73461018314185, 360.23385291015154, 359.73460148348414, 359.23684342185385, 358.74056628130916, 358.24575765492153, 357.75240517247533, 357.26049650015847, 356.7700193402541, 356.2809614308334, 355.7933105454484, 355.3070544928271, 354.8221811165682, 354.33867829483717, 353.8565339400636, 353.3757359986386, 352.89627245061365, 352.4181313094, 351.94130062146917, 351.4657684660539, 350.99152295485027, 350.5185522317206, 350.0468444723968, 349.57638788418484, 349.10717070567006, 348.63918120642296, 348.17240768670604, 347.70683847718135, 347.24246193861865, 346.77926646160444, 346.31724046625186

**耗时：**0.0秒（由于计算时间特别短，显示为0秒）

**温度分布图：**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps30.jpg) 

### 2.2、使用G-S算法求解节点方程

#### 2.2.1、10个节点

**温度场计算结果：**396.8017574154691, 390.6472766399803, 384.7194140561456, 379.00335000752193, 373.4847943340005, 368.1499506464061, 362.9854818355239, 357.97847672932636, 353.11641781504363, 348.38714994538196

**耗时：**2.444189秒

**计算精度：**1.0e-10

**迭代次数：**483

**温度分布图：**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps31.jpg) 

#### 2.2.2、50个节点

**温度场计算结果：**399.3601871714353, 398.0904978094103, 396.8306179568643, 395.5804218080316, 394.33978452412333, 393.108582220544, 391.88669195420727, 390.6739917109494, 389.47036039304027, 388.2756778067902, 387.0898246502533, 385.9126825010249, 384.74413380413347, 383.58406186002605, 382.4323508126457, 381.2888856376015, 380.1535521304284, 379.0262368949388, 377.906827331662, 376.7952116263729, 375.69127873870923, 374.5949183908739, 373.5060210564254, 372.42447794915245, 371.3501810120332, 370.2830229062784, 369.22289700045684, 368.1696973597033, 367.1233187350067, 366.08365655257893, 365.05060690330293, 364.02406653225836, 363.0039328283255, 361.9901038138649, 360.98247813447256, 359.9809550488101, 358.98543441850774, 357.9958166981401, 357.0120029252737, 356.0338947105848, 355.06139422804597, 354.09440420518257, 353.1328279133951, 352.1765691583487, 351.22553227042744, 350.2796220952528, 349.33874398426525, 348.4028037853674, 347.47170783362805, 346.54536294204576

**耗时：**11.382859秒

**计算精度：**1.0e-10

**迭代次数：**483

**温度分布图：**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps32.jpg) 

#### 2.2.3、100个节点

**温度场计算结果：**399.6597220860568, 398.98168465661774, 398.3061665886421, 397.63316888159017, 396.96269256396437, 396.29473868592527, 395.62930831192847, 394.9664025133863, 394.3060223613566, 393.6481689192614, 392.99284323563865, 392.3400463369301, 391.68977922030746, 391.0420428465405, 390.39683813290924, 389.7541659461635, 389.11402709553215, 388.47642232578545, 387.84135231035225, 387.20881764449587, 386.5788188385504, 385.95135631122054, 385.3264303829474, 384.7040412693432, 384.0841890746966, 383.4668737855524, 382.85209526436694, 382.23985324324207, 381.63014731774075, 381.0229769407854, 380.41834141664214, 379.81623989499366, 379.2166713651016, 378.6196346500617, 378.02512840115435, 377.4331510922906, 376.8437010145585, 376.25677627086964, 375.67237477070864, 375.0904942249881, 374.51113214100974, 373.9342858175348, 373.3599523399647, 372.78812857563366, 372.21881116921605, 371.65199653824897, 371.0876808687719, 370.52586011108554, 369.9665299756311, 369.40968592899094, 368.8553231900127, 368.3034367260584, 367.7540212493787, 367.20707121361465, 366.662580810428, 366.1205439662604, 365.5809543392238, 365.0438053161221, 364.5090900096052, 363.97680125545696, 363.44693161001726, 362.9194733477392, 362.3944184588823, 361.8717586473421, 361.35148532861746, 360.83358962791516, 360.31806237839334, 359.80489411954323, 359.29407509571087, 358.7855952547575, 358.27944424686063, 357.77561142345485, 357.2740858363132, 356.77485623676887, 356.2779110750774, 355.7832384999193, 355.29082635804394, 354.8006621940527, 354.3127332503232, 353.82702646707367, 353.3435284825665, 352.8622256334518, 352.38310395525, 351.9061491829728, 351.4313467518827, 350.9586817983904, 350.48813916108855, 350.0197033819226, 349.5533587074974, 349.08908909051814, 348.62687819136625, 348.16670937980837, 347.70856573683767, 347.2524300566468, 346.7982848487311, 346.3461123401214, 345.89589447774546, 345.4476129309158, 345.0012490939441, 344.5567840888803

**耗时：**41.42833秒

**计算精度：**1.0e-10

**迭代次数：**483

**温度分布图：**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps33.jpg) 

### 2.3、TDMA算法和G-S算法的对比

​		由上面的TDMA算法和G-S算法的计算数据可知，使用G-S算法计算节点温度的耗时要比TDMA算法的耗时大的多，而且随着节点个数的增加，G-S算法的耗时也在增加，且这种增加是非线性的。TDMA算法计算耗时很短。下面我们来对比相同节点使用不同算法时的计算结果比较。

**10个节点**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps34.jpg) 

**50个节点**

![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps35.jpg) 

**100个节点**
	
![img](https://zfblog.oss-cn-hangzhou.aliyuncs.com/course/%E8%AE%A1%E7%AE%97%E4%BC%A0%E7%83%AD%E5%AD%A6/%E7%AC%AC%E4%B8%80%E6%AC%A1%E5%A4%A7%E4%BD%9C%E4%B8%9A/wps36.jpg) 

​		由上面三个对比图可知，在节点数目较少时，TDMA算法计算的结果和G-S算法计算的结果很相近。随着计算节点数目的增加，G-S算法计算的结果和TDMA算法计算结果的差距开始加大，G-S算法计算的结果在节点n较大时误差较大，而且G-S算法对初始值的选取也比较挑剔，初值选择不合理，计算耗时和误差会更大。综上所述，适合使用TDMA算法来计算节点温度，相比于G-S算法，主要有以下优势：
​	
​		1、耗时短。
​	
​		2、计算精度较高。
​	
​		3、时间复杂度和空间复杂度都由于G-S算法。

## 三、计算程序源代码

​		该大作业的计算程序由python语言编写，并导入numba模块使用了jit加速运算，大大提高了计算的效率。

**程序介绍：**

- 该程序一共包含6个函数，分别为：

  - **GaussSeidel函数**：高斯赛德尔迭代计算函数

  - **JacobiMain函数**：雅可比迭代计算函数（测试用，并未用到）

  - **TDMA函数**：TDMA算法求解函数

  - **CreatHeatMatrix函数**：根据节点个数生成计算矩阵的函数

  - **plot函数**：绘制单个算法计算出的温度分布的函数

  - **ploty函数**：绘制TDMA算法和G-S算法求得的温度分布对比函数

**计算流程：**

​		先由CreatHeatMatrix函数根据给定的节点个数生成的计算矩阵（有两种模式），然后再由GaussSeidel函数或者TDMA函数计算温度分布，最后由plot函数和ploty函数绘制出温度分布图和对比图。

``` python
# -*- coding: utf-8 -*-
"""
Created on Sun Apr 15 15:29:35 2020

@author: zhangfan
"""

import time
import numpy
from numba import jit  #引入jit装饰，提高运算速度
import pylab

##############################
"""常量定义"""

L=0.1        #计算物体长度（单位：m）
k=10         #导热系数（单位：w/(m·k)）
h=100        #对流换热系数（单位：w/(m^2·k)）
T0=400       #左边界温度（单位：K）
Tf=300       #右边界温度（单位：K）

##############################


"""Gauss-Seidel迭代法求解方程组"""
@jit
def GaussSeidel(A,B,eps=1.0e-10,N=5000):
    start=time.time()
    A=numpy.array(A).astype(float)
    B=numpy.array(B).astype(float)
    num=0
    m=0
    mm=0
    epsX=0
    n=len(B)
    X0=numpy.zeros(n)+350
    X=numpy.zeros(n)+350
    while num<N:
        for i in range(n):
            if i==0:
                m=0
                for j in range(1,n):
                    m+=A[i][j]*X0[j]
                X[0]=(B[i]-m)/A[i][i]
            if i>0 and i<n-1:
                m=0
                mm=0
                for j in range(i):
                    m+=A[i][j]*X[j]
                for j in range(i+1,n):
                    mm+=A[i][j]*X0[j]
                X[i]=(B[i]-mm-m)/A[i][i]
            if i==n-1:
                m=0
                for j in range(n-1):
                    m+=A[i][j]*X[j]
                X[n-1]=(B[i]-m)/A[i][i]
        epsX = max(abs(X - X0))
        num += 1
        if epsX < eps:
            break
        else:
            X0=X.copy()
    TimeCost=time.time()-start
    return X,num,TimeCost

"""Jacobi迭代法求解方程组"""
@jit
def JacobiMain(A,B,eps=1.0e-10,N=5000):
    start=time.time()
    A=numpy.array(A).astype(float)
    B=numpy.array(B).astype(float)
    num=0
    m=0
    epsX=0
    n=len(B)
    X0=numpy.zeros(n)+350
    X=numpy.zeros(n)+350
    while num<N:
        for i in range(n):
            m=0
            for j in range(n):
                if i!=j:
                    m+=A[i][j]*X0[j]
            X[i]=(B[i]-m)/A[i][i]
        epsX=max(abs(X - X0))
        num += 1
        if epsX < eps:
            break
        else:
            X0=X.copy()
    TimeCost=time.time()-start
    return X,num,TimeCost
"""TDMA算法求解方程组"""
@jit
def TDMA(a,b,c,d):
    start=time.time()
    a=numpy.array(a).astype(float)
    b=numpy.array(b).astype(float)
    c=numpy.array(c).astype(float)
    d=numpy.array(d).astype(float)
    Lx=len(b)
    #X=d.copy()
    m=0
    b[0]=b[0]/a[0]
    d[0]=d[0]/a[0]
    for i in range(1,Lx-1):
        m = 1.0 / (a[i] - c[i] * b[i - 1])
        b[i] = b[i] * m
    for i in range(1,Lx):
        m = 1.0 / (a[i] - c[i] * b[i - 1])
        d[i] = (d[i] - c[i] * d[i - 1]) * m
    
    for i in range(Lx-2,-1,-1):
        d[i]=d[i] - b[i] * d[i + 1]
    TimeCost=time.time()-start
    return d,TimeCost


"""节点方程计算矩阵生成函数"""
#参数n为节点的个数
@jit
def CreatHeatMatrix(n,pattern="TDMA"):
    
    '''初始化热节点矩阵'''
    A=numpy.zeros((n,n))
    B=numpy.zeros(n)
    a=numpy.zeros(n)
    b=numpy.zeros(n)
    c=numpy.zeros(n)
    DeltaX=L/n
    
    '''边界节点参数赋值'''
    a[0]=250*DeltaX**2+3*k
    a[n-1]=3*k+250*DeltaX**2-4*k*k/(h*DeltaX+2*k)
    b[0]=-k
    c[n-1]=-k
    B[0]=2*k*T0+250*300*DeltaX**2
    B[n-1]=75000*DeltaX**2+Tf*2*k*h*DeltaX/(h*DeltaX+2*k)
    
    '''内部节点参数赋值'''
    for i in range(1,n-1):
        a[i]=250*DeltaX**2+2*k
        b[i]=-k
        c[i]=-k
        B[i]=75000*DeltaX**2
    
    '''
    返回值'''
    if pattern=="TDMA":
        return a,b,c,B
    elif pattern=="GS":
        '''构造矩阵'''
        for i in range(n):
            A[i][i]=a[i]
            if i>0:
                A[i][i-1]=c[i]
            if i<n-1:
                A[i][i+1]=b[i]
        return A,B

'''画出计算的数据'''
def plot(num,data,title,xlabel,ylabel,path,t,line='r.'):
    pylab.figure(num)
    n=len(data)
    pylab.plot(data,line)
    pylab.title(title)
    pylab.xlabel(xlabel)
    pylab.ylabel(ylabel)
    pylab.grid("on")
    pylab.text(n-n*0.5, 390, 'time:'+str(round(t,6))+"s")
    pylab.savefig("E://计算传热学1/"+path+".jpg")

'''画TDMA和GS的对比图'''
def ploty(num,x,y,l1,l2,title,xlabel,ylabel):
    pylab.figure(num)
    s=numpy.array([i+1 for i in range(len(x))])
    pylab.plot(s,x,'r.-',label=l1)
    pylab.plot(s,y,'b.-',label=l2)
    pylab.title(title)
    pylab.xlabel(xlabel)
    pylab.ylabel(ylabel)
    pylab.legend()
    pylab.grid("on")
    pylab.savefig("E://计算传热学1/"+title+".jpg")
    
    
'''TDMA算法求解的三种不同的节点数量（10、50、100）'''
a10,b10,c10,d10=CreatHeatMatrix(10)
a50,b50,c50,d50=CreatHeatMatrix(50)
a100,b100,c100,d100=CreatHeatMatrix(100)

'''Gauss-Seidel迭代法求解的三种不同的节点数量（10、50、100）'''
A10,B10=CreatHeatMatrix(10,pattern="GS")
A50,B50=CreatHeatMatrix(50,pattern="GS")
A100,B100=CreatHeatMatrix(100,pattern="GS")

'''TDMA算法求解计算'''
T_td10,TCost_td10=TDMA(a10,b10,c10,d10) #多计算一次来减小耗时误差
T_td10,TCost_td10=TDMA(a10,b10,c10,d10)
T_td50,TCost_td50=TDMA(a50,b50,c50,d50)
T_td100,TCost_td100=TDMA(a100,b100,c100,d100)

'''Gauss-Seidel迭代法求解'''
T_gs10,N10,TCost_gs10=GaussSeidel(A10,B10)
T_gs50,N50,TCost_gs50=GaussSeidel(A50,B50)
T_gs100,N100,TCost_gs100=GaussSeidel(A100,B100)

print("使用TDMA算法计算10个节点计算结果：",list(T_td10),'\t耗时：',round(TCost_td10,5),' s')
print("使用TDMA算法计算50个节点计算结果：",list(T_td50),'\t耗时：',round(TCost_td50,5),' s')
print("使用TDMA算法计算100个节点计算结果：",list(T_td100),'\t耗时：',round(TCost_td100,5),' s')
print("使用GS算法计算10个节点计算结果：",list(T_gs10),'\t耗时：',round(TCost_gs10,5),' s','\t迭代次数：',N10)
print("使用GS算法计算50个节点计算结果：",list(T_gs50),'\t耗时：',round(TCost_gs50,5),' s','\t迭代次数：',N10)
print("使用GS算法计算100个节点计算结果：",list(T_gs100),'\t耗时：',round(TCost_gs100,5),' s','\t迭代次数：',N10)

plot(1,T_td10,"TDMA(10 node)","Node","Temp(K)","TDMA10",TCost_td10)
plot(2,T_td50,"TDMA(50 node)","Node","Temp(K)","TDMA50",TCost_td50)
plot(3,T_td100,"TDMA(100 node)","Node","Temp(K)","TDMA100",TCost_td100)
plot(4,T_gs10,"Gauss-Seidel(10 node)","Node","Temp(K)","GS10",TCost_gs10)
plot(5,T_gs50,"Gauss-Seidel(50 node)","Node","Temp(K)","GS50",TCost_gs50)
plot(6,T_gs100,"Gauss-Seidel(100 node)","Node","Temp(K)","GS100",TCost_gs100)


ploty(7,T_td10,T_gs10,"TDMA","GS","TDMA vs GS(10 node)","Node","Temp(K)")
ploty(8,T_td50,T_gs50,"TDMA","GS","TDMA vs GS(50 node)","Node","Temp(K)")
ploty(9,T_td100,T_gs100,"TDMA","GS","TDMA vs GS(100 node)","Node","Temp(K)")
```



***2020年4月29日***

***天气：晴***

 
