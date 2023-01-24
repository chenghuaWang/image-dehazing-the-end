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
  - [迭代级联，模型选择是正确的吗？](#迭代级联模型选择是正确的吗)
- [一些实验结果](一些实验结果)
- [总结](#总结)
- [citing](#citing)

## 介绍

这里的the end并不是说终结了去雾课题，而是为期一年的去雾研究落幕了(Jan 1 2022 -> Dec 31 2022)，开始准备考研去了。之后大概率也不会再研究去雾这个课题了，在这篇文章中记录了这段时间去雾的一些体会和思考内容。虽然我只是半只脚入门计算机视觉的小白，此文全是一家之谈，但是也希望这篇文章能为近期研究去雾的同志们带来一点启发。

古早的传统图像处理方法(Hazy line<sup><a href="#ref2">[2]</a></sup>，DCP<sup><a href="#ref3">[3]</a></sup>等)在去雾上还是别有一番风味的，但是重量级武器(深度学习)上马以后，去雾领域却是一言难尽。

下文将首先给出上手去雾研究的建议；然后阐述该领域的一些工作(当然，这里不会事无巨细的写完所有的算法，成为综述一类的，只是提一下大家的普遍的做法)；之后分析目前工作的问题和去雾这个问题本身的难点；其次提出我目前的一些Idea；最后，给出我目前的实验结果和总结。

## 上手dehazing的建议

读论文、分析论文之类的就不说了，这些都是研究者的一些基本的必备素质。这里主要说的就是以何种心态和想法去做去雾研究。众所周知，去雾这个课题，目前来说还没有成对的数据集，对于数据驱动的深度学习方法来说，这是灾难性的。在测试当前网络的性能指标的时候，一共有两种选择：1. 尽力的提高在人造数据集上的指标(当然，这是没有任何意义的，人造数据集和真实数据集的差异实在太大，卷人造数据集上的指标有虚空打靶，自我陶醉之意。当然，一切为了生活/毕业嘛) 2. 只关注真实数据集上的去雾效果(这是办实事的研究，但是，对于ill posed+缺少数据集的去雾问题，使用深度学习的方法，必须做好毫无进展的准备)

<details>
<summary>[1. 卷人造数据集指标(click to expand)]</summary>
<br>

- 显然，这样的选择应该是多数的(为了生活/毕业)。如果是这样的目的就非常好办了，广泛阅读文章，找到一些漏洞，缝合/寻找新设定/为网络编造新的故事/挑选几张好的图片(ps下)/进行不公平对比/不跟sota进行对比。<u>$\color{red}{❗❗❗学术不端是严重的罪行，我坚决抵制这些行为❗❗❗}$</u>

- 不那么虚一点的。我认为可以尝试在小地方进行改进(当然要发文章的话，期刊/会议等级不会太高)。
  - 技术报告类型的文章
  - 在某些微小操作上改进，普适性的。<!-- 比如旷世non-local那篇 -->

</details>

<details>
<summary>[2. 义愤填膺的想搞出真东西(click to expand)]</summary>

<br>

- 这是一条非常艰难的路子(实际上，我是持劝退态度的，或许换一个有好的数据集，setting还算可以的课题更容易出扎实的成果)。去雾领域虽然目前有很多的文章可以借鉴参考，但是我认为还是处于非常初级的阶段，并没有非常扎实可行的主要研究方向。**希望读者在选择去雾这个课题上慎重考虑❗❗❗**

- 如果读者执意要在去雾上做出成果，那么我向你表示感谢，并且由衷的希望你能做出很好的成果，甚至终结目前去雾领域的一些怪象。也希望我在下文中阐述的一些观点能够给你启发。再次祝你研究顺利。

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

其中 $I$ 表示退化后的图片； $J$ 表示清晰的图片； $t=e^{-\beta d(x)}$，其中 $d(x)$ 是深度； $A$ 是大气光值。实际上，如果我们直接用网络估计 $t$ 和 $A$，或者估计与 $t$ 和 $A$ 相关的值，可以把这个公式退化成一个简单的线性拉伸/加权平均(除非强监督 $t$, $A$，否则无法说明网络到底学了个什么值)。

1. 去雾没有大量的成对数据集

<!-- 直接产生 t，A。或者直接产生无雾图是一种无中生有的行为，依赖于很大的数据集 -->

2. 现实中的雾不是均质的

3. 现有的数据集多样性不够

### 先验-网络-end2end

### 过拟合-还是-DomainGap

现在许多的网络方法在指标上非常的高，但是在实际的数据集上，效果并不好。很多时候人们说：“先提高指标，再考虑如何提高泛化性能。”，**但是，前提是数据集和真实数据本身是一个 Domain 内的，二者偏差不大**。在去雾中，尚没有人探讨过使用大气光照模型产生的雾和真实的雾的差别，但我想，差别还是非常大的，甚至如卡通图像和写实图像一样大的gap。

## 一些好玩的idea

### 数据集改进

1. **使用超像素的方法来改进(并不是非常好)**

2. **线性的雾生成**

### 只当作数值问题来考虑

<!-- 去雾任务需要很多卷积网络难以表达的统计信息，比如 min，max，mean，std等 -->

<!-- 神经网络适合干的事情是信息的压缩，去雾这样的映射不大适合(主要是没数据集啊) -->

### 迭代级联，模型选择是正确的吗？

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

最后，祝各位读者学习、研究顺利，身体健康，生活愉快。

## citing

```txt
@misc{imagedehzetheend,
  author = {chenghua Wang},
  title = {Image Dehazing, the end.},
  url = {https://github.com/chenghuaWang/image-dehazing-the-end},
  year = {2023},
  note = {Version 1.0}
}
```

---

*Reference*

1. <p name="ref1">Li, Boyi, Wenqi Ren, Dengpan Fu, Dacheng Tao, Dan Feng, Wenjun Zeng and Zhangyang Wang. “RESIDE: A Benchmark for Single Image Dehazing.” ArXiv abs/1712.04143 (2017)</p>

2. <p name="ref2">Fattal, Raanan. “Dehazing Using Color-Lines.” ACM Transactions on Graphics (TOG) 34 (2014): 1 - 14.</p>

3. <p name="ref3">He, Kaiming, Jian Sun and Xiao Jie Tang. “Single Image Haze Removal Using Dark Channel Prior.” IEEE Transactions on Pattern Analysis and Machine Intelligence (2011)</p>
