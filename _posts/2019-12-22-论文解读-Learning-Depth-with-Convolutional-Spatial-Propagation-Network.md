---
title: '论文解读 Learning Depth with Convolutional Spatial Propagation Network'
author: DeH40
tags: ['论文解读']
date: '2019-12-22 21:53:27+0800'
layout: post
categories: Blog
---



# Learning Depth with Convolutional Spatial Propagation Network 

这篇论文里提出的网络是目前KITTI2015排行榜一，作者提出的CSPN既可以用于深度预测也可以用于深度图补全。

本文在Spatial Propagation Networks(SPN)的基础上提出了Convolutional Spatial Propagation Networks(CSPN)，相较于SPN，CSPN可以并行计算且效果更好。CSPN和SPN一样，都使用affinity matrix（相似度矩阵）来进行传播的网络，affinity matrix是用来确定空间中两个点相似性的矩阵。

为了将CSPN用于立体匹配（处理4D的CostVolume），作者将CSPN扩展到了3D提出了3D CSPN。受spatial pyramid pooling (SPP)的启发，作者把CSPN和SPP相结合，提出了convolutional spatial pyramid pooling(CSPP)。

## 1.CSPN将SPN的按线方向的传播过程改为了卷积操作：

![image.png](https://static.dingtalk.com/media/lALPDeC2uuix3g3M2M0CSw_587_216.png_995x10000.jpg)

写成向量化的形式如下：

![image.png](https://static.dingtalk.com/media/lALPDeC2uuiq7pvNAVfNAmg_616_343.png_995x10000.jpg)

同时将他扩展到了3D：

![image.png](https://static.dingtalk.com/media/lALPDeC2uuixtfzM880CGw_539_243.png_995x10000.jpg)

下面的这个图展示了SPN和CSPN以及3D CSPN的区别：

![image.png](https://static.dingtalk.com/media/lALPDeC2uuiifMrNAbjNBS4_1326_440.png_995x10000.jpg)

## 2.作者提出所谓的spatial pyramid pooling(SPP)其实就是一种CSPN的特例：

给定大小为的特征以及空间大小为的目标池化特征图后，空间池化计算每个分块格网的均值，这就相当于设置核大小为，步长为p和q且设置中的所有值为一致时的单步CSPN。因此可以将SPP使用不同卷积核尺寸和不同步长的CSPN代替：

![image.png](https://static.dingtalk.com/media/lALPDeC2uui9w_3NAezNBO0_1261_492.png_995x10000.jpg)

作者把这个叫做CSPP，为了强化其效果作者采用了类似于注意力机制的方法，将Affinity Matrix融合到其中提出了Convolutional spatial

pyramid fusion （CSPF）：

![image.png](https://static.dingtalk.com/media/lALPDeC2uujQzuHNAg7NBF8_1119_526.png_995x10000.jpg)

下图是CSPN用于深度预测的网络结构图，其基础来自于PSMNet，主要做了两项变动：将其其空间池化模块替换为CSPF，并在多尺度输出之后附加了3DCSPN。

![image.png](https://static.dingtalk.com/media/lALPDeC2uujb7LvNATDNBQg_1288_304.png_995x10000.jpg)