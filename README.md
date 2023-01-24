# Image Dehazing, the end.

<!-- style settings -->
<!-- <style>
.center 
{
  width: auto;
  display: table;
  margin-left: auto;
  margin-right: auto;
}
</style> -->
<!-- style settings -->

```
chenghua Wang, Jan 23 2023

chenghua.wang.edu@gmail.com
```

**Keywords**: *dehaze*, *defog*, *deep learning*, *computer vision*

## Table of Contents

- [介绍](#介绍)
- [上手dehazing的建议](#上手dehazing的建议)
- [相关的工作](#相关的工作)
  - [数据集](#数据集)
  - [算法](#算法)
- [目前dehazing的主要问题](#目前dehazing的主要问题)
  - [数据集](#数据集)
  - [先验-网络-end2end](#先验-网络-end2end)
  - [过拟合-还是-DomainGap](#过拟合-还是-DomainGap)
- [一些好玩的idea](#一些好玩的idea)
  - [数据集改进](#数据集改进)
  - [只当作数值问题来考虑](#只当作数值问题来考虑)
- [一些实验结果](一些实验结果)
- [总结](#总结)
- [citing](#citing)

## 介绍

这里的the end并不是说终结了去雾课题，而是为期一年的去雾研究落幕了(Jan 1 2022 -> Dec 31 2022)，开始准备考研去了。之后大概率也不会再研究去雾这个课题了，在这篇文章中记录了这段时间去雾的一些体会和思考内容。虽然我只是半只脚入门计算机视觉的小白，此文全是一家之谈，但是也希望这篇文章能为近期研究去雾的同志们带来一点启发。

下文将首先给出上手去雾研究的建议；然后阐述该领域的一些工作(当然，这里不会事无巨细的写完所有的算法，成为综述一类的，只是提一下大家的普遍的做法)；之后分析目前工作的问题和去雾这个问题本身的难点；其次提出我目前的一些Idea；最后，给出我目前的实验结果和总结。

## 上手dehazing的建议

读论文、分析论文之类的就不说了，这些都是研究者的一些基本的必备素质。这里主要说的就是以何种心态和想法去做去雾研究。众所周知，去雾这个课题，目前来说还没有成对的数据集，对于数据驱动的深度学习方法来说，这是灾难性的。在测试当前网络的性能指标的时候，一共有两种选择：1. 尽力的提高在人造数据集上的指标(当然，这是没有任何意义的，人造数据集和真实数据集的差异实在太大，卷人造数据集上的指标有虚空打靶，自我陶醉之意。当然，一切为了生活/毕业嘛) 2. 只关注真实数据集上的去雾效果(这是办实事的研究，但是，对于ill posed+缺少数据集的去雾问题，使用深度学习的方法，必须做好毫无进展的准备)

<details>
<summary>[1.卷人造数据集指标(click to expand)]</summary>
显然，这样的选择应该是多数的(为了生活/毕业)。TODO
</details>

<details>
<summary>[2.义愤填膺的想搞出真东西(click to expand)]</summary>
这是一条非常艰难的路子(实际上，我是持劝退态度的，或许换一个有好的数据集，setting还算可以的课题更容易出扎实的成果)。TODO
</details>

## 相关的工作

### 数据集

<div class="center">

|数据集 | 描述  | 人造/真实 |URL |
|:----:|:----:|:----:    |:----: |
|RESIDE<sup><a href="#ref1">[1]</a></sup>|训练集包含13,990个合成图像，每个清晰图像合成10个模糊图像。 提供了13,000个用于训练和990个用于验证。设置每个通道大气光A在[0.7，1.0]之间，均匀地随机选择beta在[0.6,1.8]。 |人造| [link](https://sites.google.com/view/reside-dehaze-datasets/reside-standard?authuser=3D0)|    

</div>

### 算法

## 目前dehazing的主要问题

我始终认为，解决数据集的问题(如何模拟出现实场景中的雾)是去雾里面最重要的点。好的先验是其次的，好的网络设计是最后的，然后才是调参数。最后，对于如何评价网络性能的优劣，也缺少一个合适的方法(在 low-level 的大部分任务中都缺少一个合适的评价指标)

### 数据集

在去雾中，最常使用的物理模型是:

$$
I = J \times t + A \times (1-t)
$$

其中 $I$ 表示退化后的图片；$J$ 表示清晰的图片；$t=e^{-\beta d(x)}$，其中$d(x)$是深度；$A$ 是大气光值。实际上，如果我们直接用网络估计 $t$ 和 $A$，或者估计与$t$ 和 $A$相关的值，可以把这个公式退化成一个简单的线性拉伸/加权平均(除非强监督$t$, $A$，否则无法说明网络到底学了个什么值)。

1. **去雾没有大量的成对数据集**

<!-- 直接产生 t，A。或者直接产生无雾图是一种无中生有的行为，依赖于很大的数据集 -->

2. **现实中的雾不是均质的**

3. **现有的数据集多样性不够**

### 先验-网络-end2end

### 过拟合-还是-DomainGap

现在许多的网络方法在指标上非常的高，但是在实际的数据集上，效果并不好。很多时候人们说：“先提高指标，再考虑如何提高泛化性能。”，**但是，前提是数据集和真实数据本身是一个 Domain 内的，二者偏差不大**。在去雾中，尚没有人探讨过使用大气光照模型产生的雾和真实的雾的差别，但我想，差别还是非常大的，甚至如卡通图像和写实图像一样大的gap。

## 一些好玩的idea

### 数据集改进

1. **使用超像素的方法来改进(并不是非常好)**

2. **线性的雾生成**

### 只当作数值问题来考虑

## 一些实验结果

<table>
    <tr>
        <td ><center><img src="./asset/real-world/3.bmp" > pic 1(a) real-world id=3</center></td>
        <td ><center><img src="./asset/real-world/3.bmp" > pic 1(b) dehazed id=3</center></td>
    </tr>
    <tr>
        <td><center><img src="./asset/real-world/5.bmp"> pic 2(a) real-world id=5</center></td>
        <td ><center><img src="./asset/real-world/5.bmp"> pic 2(b) dehazed id=5</center> </td>
    </tr>
    <tr>
        <td><center><img src="./asset/real-world/104.bmp" > pic 3(a) real-world id=104</center></td>
        <td ><center><img src="./asset/real-world/104.bmp" > pic 3(b) dehazed id=104</center> </td>
    </tr>
</table>

<details>
<summary>[click to check more]</summary>
<table>
    <tr>
        <td ><center><img src="./asset/real-world/3.bmp" > pic 1(a) real-world id=3</center></td>
        <td ><center><img src="./asset/real-world/3.bmp" > pic 1(b) dehazed id=3</center></td>
    </tr>
    <tr>
        <td><center><img src="./asset/real-world/5.bmp" > pic 2(a) real-world id=5</center></td>
        <td ><center><img src="./asset/real-world/5.bmp"> pic 2(b) dehazed id=5</center> </td>
    </tr>
</table>
</details>

<!-- <figure class="half">
    <img src="http://xxx.jpg">
    <img src="http://yyy.jpg">
</figure>

<figure class="half">
    <img src="http://xxx.jpg">
    <img src="http://yyy.jpg">
</figure>

<figure class="half">
    <img src="http://xxx.jpg">
    <img src="http://yyy.jpg">
</figure> -->

## 总结

## citing

```txt
@misc{imagedehzetheend,
  author = {chenghua Wang},
  title = {Image Dehazing, the end},
  url = {TODO},
  year = {2023},
  note = {Version 1.0}
}
```

---

*Reference*

1. <p name="ref1">Li, Boyi, Wenqi Ren, Dengpan Fu, Dacheng Tao, Dan Feng, Wenjun Zeng and Zhangyang Wang. “RESIDE: A Benchmark for Single Image Dehazing.” ArXiv abs/1712.04143 (2017): n. pag.</p>
