# Image Dehazing, the end.

<!-- style settings -->
<style>
.center 
{
  width: auto;
  display: table;
  margin-left: auto;
  margin-right: auto;
}
</style>
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

读论文、分析论文之类的就不说了，这些都是研究者的一些基本的必备素质。这里主要说的就是以何种心态和想法去做去雾研究。众所周知，去雾这个课题，目前来说还没有成对的数据集，对于数据驱动的深度学习方法来说，这是灾难性的。在测试当前网络的性能指标的时候，一共有两种选择：1.尽力的提高在人造数据集上的指标(当然，这是没有任何意义的，人造数据集和真实数据集的差异实在太大，卷人造数据集上的指标有虚空打靶，自我陶醉之意。当然，一切为了生活/毕业嘛) 2.只关注真实数据集上的去雾效果(这是办实事的研究，但是，对于ill posed+缺少数据集的去雾问题，使用深度学习的方法，必须做好毫无进展的准备)

<details>
<summary>[1.卷人造数据集指标]</summary>
显然，这样的选择应该是多数的(为了生活/毕业)。TODO
</details>

<details>
<summary>[2.义愤填膺的想搞出真东西]</summary>
这是一条非常艰难的路子(实际上，我是持劝退态度的，或许换一个有好的数据集，setting还算可以的课题更容易出扎实的成果)。TODO
</details>

## 相关的工作

### 数据集

<div class="center">

|数据集 | 描述  | 人造/真实 |URL |
|:----:|:----:|:----:    |:----: |
|RESIDE[^1]|训练集包含13,990个合成图像，每个清晰图像合成10个模糊图像。 提供了13,000个用于训练和990个用于验证。设置每个通道大气光A在[0.7，1.0]之间，均匀地随机选择beta在[0.6,1.8]。 |人造| [link](https://sites.google.com/view/reside-dehaze-datasets/reside-standard?authuser=3D0)|    

</div>
[^1]: Li, Boyi, Wenqi Ren, Dengpan Fu, Dacheng Tao, Dan Feng, Wenjun Zeng and Zhangyang Wang. “RESIDE: A Benchmark for Single Image Dehazing.” ArXiv abs/1712.04143 (2017): n. pag.

### 算法

## 目前dehazing的主要问题

### 数据集

### 先验-网络-end2end

### 过拟合-还是-DomainGap

## 一些好玩的idea

### 数据集改进

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
<summary>[check more]</summary>
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

