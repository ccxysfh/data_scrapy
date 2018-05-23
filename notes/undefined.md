# 基于深度卷积网络的端到端SPH流体仿真

## 摘要

基于N-S方程的高效仿真一直在数值计算上耗费巨大的计算资源，近来一些利用机器学习方法加速流体仿真的方法也陆续被提出来，但是我们发现，很多方法都是采用卷积网络对欧拉网格法进行驱动仿真，这些方法都无法针对SPH仿真中离散粒子数据的处理，因而我们提出一种端到端的自动提取离散粒子特征的深度学习框架，驱动仿真场景中的粒子加速度，我们称其为FluidPointConvNet。借鉴在3-D目标检测应用中对点云数据的处理，摒弃大多数粒子法使用的手动提取粒子特征的方式，提出一种将SPH粒子分组到固定体元，并使用全卷积网络进行特征提取，学习场景中粒子的平移，旋转及排列不变性。

## 介绍

基于N-S方程的高效仿真一直在数值计算上耗费巨大的计算资源，近来随着机器学习的流行，以及深度学习在不同领域都取得突破性进展，尤其是在欧拉网格法上，嗯，一大堆方法。

但是这些方法之所以会选择欧拉法进行探索，一方面是因为卷积网络在类图像数据的处理上，有着卓越的效果，另一方面，欧拉法的空间划分使得数据稳定，正好可以适用于卷积网络，二者相互结合，产生了一些结果比较优秀的驱动仿真方法，但是这些方法都回避了对于粒子法仿真数据的处理。

粒子法的数据具有这样几个特点：首先，流体被粒子化，每个粒子便成为一个单独的个体，因而数据离散，粒子运动后，数据更为离散；其次，不同仿真场景，即使整体仿真空间不变，但场景内的粒子数目变化不定，仿真过程中也会出现粒子的增加和减少。目前，处理SPH仿真方法的机器方法很少，唯一看到的面向粒子法的随机森林方法也是基于手动提取的基于上下文积分特征，并且需要在庞大的集群上进行训练，在一般的环境下并不容易实现，并且手工提取特征的方式在实践过程中，当特征维度很大时，针对每个仿真场景中的所有粒子都进行积分特征的提取，即便常量计算时间，如果不是并行化做的特别好，即便是一个只有几万个粒子的仿真场景，这样的时间消耗其实也是很难忽略不计的，所以我的观点作者在论文对比的时间效率其实应该是除去了这部分预处理的时间，这样结果其实会让机器学习方法的效率提升显得苍白无力。

## 引用

 Deep learning in fluid dynamics 

https://github.com/loliverhennigh/Computational-Physics-and-Machine-Learning-Reading-List

## 讨论

from reddit and stackoverflow

从 Computational Fluid Dynamics分类开始，交代欧拉方法和SPH方法在传统CFD领域所处的位置， proper  orthogonal  decomposition  or  dynamic  mode  decomposition（正交分解，动态模态分解，奇异值分解）