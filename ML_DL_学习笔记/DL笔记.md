# 网络模型

## LeNet

它是最早发布的卷积神经网络之一，因其在计算机视觉任务中的高效性能而受到广泛关注。 这个模型是由AT&T贝尔实验室的研究员Yann LeCun在1989年提出的（并以其命名），目的是识别图像 `LeCun.Bottou.Bengio.ea.1998`中的手写数字。 当时，Yann LeCun发表了第一篇通过反向传播成功训练卷积神经网络的研究，这项工作代表了十多年来神经网络研究开发的成果。

总体来看，(**LeNet（LeNet-5）由两个部分组成：**)(卷积编码器和全连接层密集块)

- 卷积编码器：由两个卷积层组成;
- 全连接层密集块：由三个全连接层组成。

`架构图`

<img src="https://zh-v2.d2l.ai/_images/lenet.svg" alt="../_images/lenet.svg" style="zoom: 50%;" />

```python
# 网络结构
net = nn.Sequential(
    	nn.Conv2D(channels=6, kernel_size=5, padding=2, activation='sigmoid'),
        nn.AvgPool2D(pool_size=2, strides=2),
        nn.Conv2D(channels=16, kernel_size=5, activation='sigmoid'),
        nn.AvgPool2D(pool_size=2, strides=2),
        nn.Linear(16 * 5 * 5, 120), nn.Sigmoid(),
    	nn.Linear(120, 84), nn.Sigmoid(),
    	nn.Linear(84, 10))
```

`小结`

- 卷积神经网络（CNN）是一类使用卷积层的网络。
- 在卷积神经网络中，我们组合使用卷积层、非线性激活函数和汇聚层。
- 为了构造高性能的卷积神经网络，我们通常对卷积层进行排列，逐渐降低其表示的空间分辨率，同时增加通道数。
- 在传统的卷积神经网络中，卷积块编码得到的表征在输出之前需由一个或多个全连接层进行处理。
- LeNet是最早发布的卷积神经网络之一。



## AlexNet

`对比及改进`

- 更深更大的LeNet
- 主要改进：
  - 丢弃法（dropout）
  - ReLU
  - MaxPooling
  - 数据增强
- 计算机视觉方法论的改变

2012年，AlexNet横空出世。它首次证明了学习到的特征可以超越手工设计的特征。它一举打破了计算机视觉研究的现状。 AlexNet使用了8层卷积神经网络，并以很大的优势赢得了2012年ImageNet图像识别挑战赛。

AlexNet和LeNet的架构非常相似，注意，这里我们提供了一个稍微精简版本的AlexNet，去除了当年需要两个小型GPU同时运算的设计特点。

`架构图`

<img src="https://zh-v2.d2l.ai/_images/alexnet.svg" alt="../_images/alexnet.svg" style="zoom:67%;" />

AlexNet和LeNet的设计理念非常相似，但也存在显著差异。 首先，AlexNet比相对较小的LeNet5要深得多。 AlexNet由八层组成：五个卷积层、两个全连接隐藏层和一个全连接输出层。 其次，AlexNet使用ReLU而不是sigmoid作为其激活函数。

```python
# 网络结构
net = nn.Sequential(
    # 这里，我们使用一个11*11的更大窗口来捕捉对象。
    # 同时，步幅为4，以减少输出的高度和宽度。
    # 另外，输出通道的数目远大于LeNet
    nn.Conv2d(1, 96, kernel_size=11, stride=4, padding=1), nn.ReLU(),
    nn.MaxPool2d(kernel_size=3, stride=2),
    # 减小卷积窗口，使用填充为2来使得输入与输出的高和宽一致，且增大输出通道数
    nn.Conv2d(96, 256, kernel_size=5, padding=2), nn.ReLU(),
    nn.MaxPool2d(kernel_size=3, stride=2),
    # 使用三个连续的卷积层和较小的卷积窗口。
    # 除了最后的卷积层，输出通道的数量进一步增加。
    # 在前两个卷积层之后，汇聚层不用于减少输入的高度和宽度
    nn.Conv2d(256, 384, kernel_size=3, padding=1), nn.ReLU(),
    nn.Conv2d(384, 384, kernel_size=3, padding=1), nn.ReLU(),
    nn.Conv2d(384, 256, kernel_size=3, padding=1), nn.ReLU(),
    nn.MaxPool2d(kernel_size=3, stride=2),
    nn.Flatten(),
    # 这里，全连接层的输出数量是LeNet中的好几倍。使用dropout层来减轻过拟合
    nn.Linear(6400, 4096), nn.ReLU(),
    nn.Dropout(p=0.5),
    nn.Linear(4096, 4096), nn.ReLU(),
    nn.Dropout(p=0.5),
    # 最后是输出层。由于这里使用Fashion-MNIST，所以用类别数为10，而非论文中的1000
    nn.Linear(4096, 10))
```

`小结`

- AlexNet的架构与LeNet相似，但使用了更多的卷积层和更多的参数来拟合大规模的ImageNet数据集。
- 今天，AlexNet已经被更有效的架构所超越，但它是从浅层网络到深层网络的关键一步。
- 尽管AlexNet的代码只比LeNet多出几行，但学术界花了很多年才接受深度学习这一概念，并应用其出色的实验结果。这也是由于缺乏有效的计算工具。
- Dropout、ReLU和预处理是提升计算机视觉任务性能的其他关键步骤。



## VGGNet

`对比及改进`

- 更大更深的AlexNet（重复的VGG块）

与AlexNet、LeNet一样，VGG网络可以分为两部分：第一部分主要由卷积层和汇聚层组成，第二部分由全连接层组成。

`架构图`

<img src="https://zh-v2.d2l.ai/_images/vgg.svg" alt="../_images/vgg.svg" style="zoom:67%;" />

- 多个VGG块后接全连接层
- 不同次数的重复块得到不同的架构 VGG-16，VGG-19

```python
# vgg块
def vgg_block(num_convs, in_channels, out_channels):
    layers = []
    for _ in range(num_convs):
        layers.append(nn.Conv2d(in_channels, out_channels,
                                kernel_size=3, padding=1))
        layers.append(nn.ReLU())
        in_channels = out_channels
    layers.append(nn.MaxPool2d(kernel_size=2,stride=2))
    return nn.Sequential(*layers)
# 模型
def vgg(conv_arch):
    conv_blks = []
    in_channels = 1
    # 卷积层部分
    for (num_convs, out_channels) in conv_arch:
        conv_blks.append(vgg_block(num_convs, in_channels, out_channels))
        in_channels = out_channels

    return nn.Sequential(
        *conv_blks, nn.Flatten(),
        # 全连接层部分
        nn.Linear(out_channels * 7 * 7, 4096), nn.ReLU(), nn.Dropout(0.5),
        nn.Linear(4096, 4096), nn.ReLU(), nn.Dropout(0.5),
        nn.Linear(4096, 10))

net = vgg(conv_arch)
```

`小结`

- VGG-11使用可复用的卷积块构造网络。不同的VGG模型可通过每个块中卷积层数量和输出通道数量的差异来定义。
- 块的使用导致网络定义的非常简洁。使用块可以有效地设计复杂的网络。
- 在VGG论文中，Simonyan和Ziserman尝试了各种架构。特别是他们发现深层且窄的卷积（即3×3）比较浅层且宽的卷积更有效。



## NiN

==网络中的网络==

LeNet、AlexNet和VGG都有一个共同的设计模式：

- 通过一系列的卷积层与汇聚层来提取空间结构特征；
- 然后通过全连接层对特征的表征进行处理。 

AlexNet和VGG对LeNet的改进主要在于如何扩大和加深这两个模块。 或者，可以想象在这个过程的早期使用全连接层。然而，如果使用了全连接层，可能会完全放弃表征的空间结构。 

*网络中的网络*（*NiN*）提供了一个非常简单的解决方案：**在每个像素的通道上分别使用多层感知机**

`架构图`

<img src="https://zh-v2.d2l.ai/_images/nin.svg" alt="../_images/nin.svg" style="zoom:67%;" />

==NiN块==

- 一个卷积层后跟两个FC
  - 步幅1，无填充，输出形状和卷积层输出一样
  - 充当全连接层

`架构`

- 无全连接层
- 交替使用NiN块和步幅为2的MaxPooling
  - 逐步减小高宽和增大通道数
- 最后使用 全局平均池化得到输出
  - 其输入通道数是类别数

`对比及改进`

- NiN和AlexNet之间的一个显著区别是NiN完全取消了全连接层。 
- 相反，NiN使用一个NiN块，其输出通道数等于标签类别的数量。最后放一个*全局平均汇聚层*（global average pooling layer），生成一个对数几率 （logits）。
- NiN设计的一个优点是，它显著减少了模型所需参数的数量。然而，在实践中，这种设计有时会增加训练模型的时间。

```python
# 网络结构
# NiN块
def nin_block(in_channels, out_channels, kernel_size, strides, padding):
    return nn.Sequential(
        nn.Conv2d(in_channels, out_channels, kernel_size, strides, padding),
        nn.ReLU(),
        nn.Conv2d(out_channels, out_channels, kernel_size=1), nn.ReLU(),
        nn.Conv2d(out_channels, out_channels, kernel_size=1), nn.ReLU())
# 模型
net = nn.Sequential(
    nin_block(1, 96, kernel_size=11, strides=4, padding=0),
    nn.MaxPool2d(3, stride=2),
    nin_block(96, 256, kernel_size=5, strides=1, padding=2),
    nn.MaxPool2d(3, stride=2),
    nin_block(256, 384, kernel_size=3, strides=1, padding=1),
    nn.MaxPool2d(3, stride=2),
    nn.Dropout(0.5),
    # 标签类别数是10
    nin_block(384, 10, kernel_size=3, strides=1, padding=1),
    nn.AdaptiveAvgPool2d((1, 1)),
    # 将四维的输出转成二维的输出，其形状为(批量大小,10)
    nn.Flatten())
```

`总结`

- NiN使用由一个卷积层和多个1×1卷积层组成的块。该块可以在卷积神经网络中使用，以允许更多的每像素非线性。
- NiN去除了容易造成过拟合的全连接层，将它们替换为全局平均汇聚层（即在所有位置上进行求和）。该汇聚层通道数量为所需的输出数量（例如，Fashion-MNIST的输出为10）。
- 移除全连接层可减少过拟合，同时显著减少NiN的参数。
- NiN的设计影响了许多后续卷积神经网络的设计。



## GoogLeNet

`对比及改进`

- 这篇论文的一个重点是**解决了什么样大小的卷积核最合适的问题**

`Inception块`

在GoogLeNet中，基本的卷积块被称为*Inception块*

下图中，蓝色1×1卷积 为抽取信息；白色1×1卷积为 改变通道数，减少通道数，降维，降低模型复杂性

<img src="https://zh-v2.d2l.ai/_images/inception.svg" alt="../_images/inception.svg" style="zoom: 80%;" />

`架构`

- GoogLeNet一共使用9个Inception块和全局平均汇聚层的堆叠来生成其估计值。
- Inception块之间的最大汇聚层可降低维度。 第一个模块类似于AlexNet和LeNet，Inception块的组合从VGG继承，全局平均汇聚层避免了在最后使用全连接层。

<img src="https://zh-v2.d2l.ai/_images/inception-full.svg" alt="../_images/inception-full.svg" style="zoom:80%;" />

`小结`

- Inception块相当于一个有4条路径的子网络。它通过不同窗口形状的卷积层和最大汇聚层来并行抽取信息
  - 并使用1×1卷积层**减少**每像素级别上的**通道维数**从而**降低模型复杂度**，**模型参数小**。
- GoogLeNet将多个设计精细的Inception块与其他层（卷积层、全连接层）串联起来。其中Inception块的通道数分配之比是在ImageNet数据集上通过大量的实验得来的。
- GoogLeNet和它的后继者们一度是ImageNet上最有效的模型之一：它以较低的计算复杂度提供了类似的测试精度。

## ResNet

> 残差网络的核心思想是：**每个附加层都应该更容易地包含原始函数作为其元素之一**。 于是，*残差块*（residual blocks）便诞生了，这个设计对如何建立深层神经网络产生了深远的影响。 凭借它，ResNet赢得了2015年ImageNet大规模视觉识别挑战赛。

`残差块`

- 串联一个层改变函数类，我们希望扩大函数类
- 残差块加入快速通道（右边）来得到 f(x) = x + g(x)的结构

右图是ResNet的基础架构–*残差块*（residual block）。 在残差块中，输入可通过跨层数据线路更快地向前传播。

<img src="https://zh-v2.d2l.ai/_images/residual-block.svg" alt="../_images/residual-block.svg" style="zoom:67%;" />

```python
class Residual(nn.Module):  #@save
    def __init__(self, input_channels, num_channels,
                 use_1x1conv=False, strides=1):
        super().__init__()
        self.conv1 = nn.Conv2d(input_channels, num_channels,
                               kernel_size=3, padding=1, stride=strides)
        self.conv2 = nn.Conv2d(num_channels, num_channels,
                               kernel_size=3, padding=1)
        # 1 × 1卷积，改变通道数
        if use_1x1conv:
            self.conv3 = nn.Conv2d(input_channels, num_channels,
                                   kernel_size=1, stride=strides)
        else:
            self.conv3 = None
        self.bn1 = nn.BatchNorm2d(num_channels)
        self.bn2 = nn.BatchNorm2d(num_channels)

    def forward(self, X):
        Y = F.relu(self.bn1(self.conv1(X)))
        Y = self.bn2(self.conv2(Y))
        if self.conv3:
            X = self.conv3(X)
        Y += X
        return F.relu(Y)
```

`块细节`

- ResNet沿用了VGG完整的3×3卷积层设计。
- 每个卷积层后接一个批量规范化层和ReLU激活函数。

如图，此代码生成两种类型的网络： 一种是当`use_1x1conv=False`时，应用ReLU非线性函数之前，将输入添加到输出。 另一种是当`use_1x1conv=True`时，添加通过1×1卷积调整通道和分辨率。

<img src="https://zh-v2.d2l.ai/_images/resnet-block.svg" alt="../_images/resnet-block.svg" style="zoom:67%;" />

```python
def resnet_block(input_channels, num_channels, num_residuals,
                 first_block=False):
    blk = []
    for i in range(num_residuals):
        if i == 0 and not first_block:
            blk.append(Residual(input_channels, num_channels,
                                use_1x1conv=True, strides=2))
        else:
            blk.append(Residual(num_channels, num_channels))
    return blk
```

`架构`

- 类似VGG和GoogLeNet的总体架构
- Inception块替换成了ResNet块

下图为ResNet-18 架构

<img src="https://zh-v2.d2l.ai/_images/resnet18.svg" alt="../_images/resnet18.svg" style="zoom:67%;" />

`小结`

- 残差映射可以更容易地学习同一函数，例如将权重层中的参数近似为零。
- 利用残差块（residual blocks）可以训练出一个有效的深层神经网络
  - 甚至可以训练一千层的网络
  - 输入可以通过层间的残余连接更快地向前传播。

# 计算机视觉（CV）

## 数据增强

大多数图像增广方法都具有一定的随机性。为了便于观察图像增广的效果，我们下面定义辅助函数`apply`。 此函数在输入图像`img`上多次运行图像增广方法`aug`并显示所有结果。

```python
def apply(img, aug, num_rows=2, num_cols=4, scale=1.5):
    Y = [aug(img) for _ in range(num_rows * num_cols)]
    d2l.show_images(Y, num_rows, num_cols, scale=scale)
    
# 水平方向随机反转
apply(img, torchvision.transforms.RandomHorizontalFlip())

# 垂直翻转
apply(img, torchvision.transforms.RandomVerticalFlip())

# 随机取部分并调整至规定大小
shape_aug = torchvision.transforms.RandomResizedCrop(
    (200, 200), scale=(0.1, 1), ratio=(0.5, 2))
apply(img, shape_aug)

# 亮度
apply(img, torchvision.transforms.ColorJitter(
    brightness=0.5, contrast=0, saturation=0, hue=0))

# 色相
apply(img, torchvision.transforms.ColorJitter(
    brightness=0, contrast=0, saturation=0, hue=0.5))

# 多个参数一起调
color_aug = torchvision.transforms.ColorJitter(
    brightness=0.5, contrast=0.5, saturation=0.5, hue=0.5)
apply(img, color_aug)

# 所有策略一起使用
augs = torchvision.transforms.Compose([
    torchvision.transforms.RandomVerticalFlip(),
    color_aug, shape_aug])
apply(img, augs)
```

`小结`

- 图像增广基于现有的训练数据生成随机图像，来提高模型的泛化能力。
- 为了在预测过程中得到确切的结果，我们通常对训练样本只进行图像增广，而在预测过程中不使用带随机操作的图像增广。
- 深度学习框架提供了许多不同的图像增广方法，这些方法可以被同时应用。
- 相比不增广，增广的train acc小，test acc大，某种程度上减轻了过拟合

## 微调

> 迁移学习中的常见技巧:*微调*（fine-tuning），从*源数据集*学到的知识迁移到*目标数据集*

微调包括以下四个步骤：

1. 在源数据集（例如ImageNet数据集）上**预训练**神经网络模型，即*源模型*。
2. 创建一个新的神经网络模型，即*目标模型*。这将复制源模型上的所有模型设计及其参数（输出层除外）。我们假定这些模型参数包含从源数据集中学到的知识，这些知识也将适用于目标数据集。我们还假设源模型的输出层与源数据集的标签密切相关；因此不在目标模型中使用该层。
3. 向目标模型添加输出层，其输出数是目标数据集中的类别数。然后随机初始化该层的模型参数。
4. 在目标数据集（如椅子数据集）上训练目标模型。输出层将从头开始进行训练，而所有其他层的参数将根据源模型的参数进行微调。

<img src="https://zh-v2.d2l.ai/_images/finetune.svg" alt="../_images/finetune.svg" style="zoom: 80%;" />

> 当目标数据集比源数据集小得多时，微调有助于提高模型的泛化能力。

`训练`

- 是一个目标数据集上的正常训练任务，但使用更强的正则化
  - 使用更小的学习率
  - 更少的数据迭代
- 源数据集远复杂于目标数据，通常微调效果更好

`小结`

- 微调通过使用在大数据集上得到的预训练好的模型来初始化模型权重来完成提升精度
- 预训练模型质量很重要
- 微调通常速度更快，精度更高

## 目标检测

`边界框`

- 目标检测不仅可以识别图像中所有感兴趣的物体，还能识别它们的位置，该位置通常由矩形边界框表示。
- 我们可以在两种常用的边界框表示（中间，宽度，高度）和（左上，右下）坐标之间进行转换。

### 锚框

一块区域，计算机的预测，而边缘框是真实标签

- 两次预测
  - 先预测锚框内是否含有物体
  - 如果有，再预测锚框到真实边缘框的偏移

`IoU-交互比`

<img src="https://zh-v2.d2l.ai/_images/iou.svg" alt="../_images/iou.svg" style="zoom:67%;" />

- 用来计算两个框之间的相似度
  - 0表示无重叠，1表示重合

`赋予锚框标号`

- 每个锚框都是一个训练样本
- 锚框要么标注成背景，要么关联一个真实边缘框
- 大量的锚框导致大量的负类样本

`非极大值抑制（NAS）`

对于一个预测边界框B，目标检测模型会计算每个类别的预测概率。 假设最大的预测概率为p，则该概率所对应的类别B即为预测的类别。具体来说，我们将p称为预测边界框B的*置信度*（confidence）。在同一张图像中，所有预测的非背景边界框都按置信度降序排序，以生成列表L。

- 每个锚框预测一个边缘框
- NMS可以合并相似的预测
  - 选中是非背景类的最大预测值
  - 去掉所以其他和它IoU值大于θ的值
  - 重复上述过程直到所有预测要么被选中,要么被去掉

`总结`

- 我们以图像的每个像素为中心生成不同形状的锚框，一类目标检测算法基于锚框来预测。
- **交并比**（IoU）也被称为杰卡德系数，用于衡量两个边界框的相似性。它是相交面积与相并面积的比率。
- 在训练集中，我们需要给每个锚框两种类型的标签。一个是与锚框中目标检测的**类别**，另一个是锚框真实相对于边界框的**偏移量**。
- 在预测期间，我们可以使用非极大值抑制（NMS）来移除类似的预测边界框，从而简化输出，去掉冗余。

## 物体检测算法

### R-CNN

*R-CNN*首先从输入图像中选取若干（例如2000个）*提议区域*（如锚框也是一种选取方法），并标注它们的类别和边界框（如偏移量）。 [[Girshick et al., 2014](https://zh-v2.d2l.ai/chapter_references/zreferences.html#id43)]然后，用卷积神经网络对每个提议区域进行前向传播以抽取其特征。 接下来，我们用每个提议区域的特征来预测类别和边界框。

![../_images/r-cnn.svg](https://zh-v2.d2l.ai/_images/r-cnn.svg)

`四步骤`

- 使用启发式搜索算法来选择锚框
- 使用预训练模型来对每个锚框提取特征
- 训练一个SVM来对类别分类
  - 训练一个线性回归模型来预测边缘框偏移

### Fast R-CNN

R-CNN的主要性能瓶颈在于，对每个提议区域，卷积神经网络的前向传播是独立的，而没有共享计算。 由于这些区域通常有重叠，独立的特征抽取会导致重复的计算。 *Fast R-CNN* [[Girshick, 2015](https://zh-v2.d2l.ai/chapter_references/zreferences.html#id42)]对R-CNN的**主要改进**之一，是**仅在整张图象上执行卷积神经网络的前向传播**。

![../_images/fast-rcnn.svg](https://zh-v2.d2l.ai/_images/fast-rcnn.svg)

`兴趣区域（Rol）池化层`（对每个锚框生成固定长度的特征）

- 给定一个锚框，均匀分成n×m块，输出每块里的最大值
- 无论锚框多大，总是输出nm个值

![../_images/roi.svg](https://zh-v2.d2l.ai/_images/roi.svg)

### Faster R-CNN



- 使用一个区域提议网络来替代启发式搜索来获得更好的锚框

![../_images/faster-rcnn.svg](https://zh-v2.d2l.ai/_images/faster-rcnn.svg)

与Fast R-CNN相比，Faster R-CNN只将生成提议区域的方法从选择性搜索改为了区域提议网络，模型的其余部分保持不变。

### Mask R-CNN

- 如果有像素级别的标号，使用FCN来利用这些信息

![../_images/mask-rcnn.svg](https://zh-v2.d2l.ai/_images/mask-rcnn.svg)

Mask R-CNN是基于Faster R-CNN修改而来的。 具体来说，Mask R-CNN将兴趣区域汇聚层替换为了 ***兴趣区域对齐*层**，使用***双线性插值***（bilinear interpolation）来保留特征图上的空间信息，从而更适于像素级预测。 兴趣区域对齐层的输出包含了所有与兴趣区域的形状相同的特征图。 它们不仅被用于预测每个兴趣区域的类别和边界框，还通过额外的全卷积网络预测目标的像素级位置。

### R-CNN总结

- 最早且有名的一类基于锚框和CNN的目标检测算法
- Faster R-CNN和Mask R-CNN是在求高精度场景下的常用算法
  - R-CNN对图像选取若干提议区域，使用卷积神经网络对每个提议区域执行前向传播以**抽取其特征**，然后再用这些特征来预测提议区域的类别和边界框。
  - Fast R-CNN对R-CNN的一个主要改进：只对**整个图像**做卷积神经网络的前向传播。它还引入了**兴趣区域汇聚层**，从而为具有不同形状的兴趣区域抽取相同形状的特征。
  - Faster R-CNN将Fast R-CNN中使用的选择性搜索替换为参与训练的**区域提议网络**，这样后者可以在减少提议区域数量的情况下仍保证目标检测的精度。
  - Mask R-CNN在Faster R-CNN的基础上引入了一个**全卷积网络**，从而借助目标的**像素级**位置进一步提升目标检测的精度。

### SSD

> 单发多框检测

- 一个基础网络提取特征，多个卷积层块来减半高宽
- 每段都生成锚框
  - 底部段拟合小物体，顶部段拟合大物体
- 对每个锚框预测类别和边缘框

![../_images/ssd.svg](https://zh-v2.d2l.ai/_images/ssd.svg)

`小结`

- 单发多框检测是一种多尺度目标检测模型。基于基础网络块和各个多尺度特征块
- 以**每个像素为中心的产生多个锚框**，单发多框检测生成不同数量和不同大小的锚框，并通过预测这些锚框的类别和偏移量检测不同大小的目标。
- 在训练单发多框检测模型时，**损失函数**是根据锚框的类别和偏移量的预测及标注值计算得出的。

### YOLO

- 因为SSD锚框大量重叠，浪费计算

- YOLO将图片均匀分成S × S个锚框，每个锚框预测B个边缘框                                                                                                                                                                                                                              

  

## 语义分割

它重点关注于如何将图像分割成属于不同语义类别的区域。 与目标检测不同，语义分割可以识别并理解图像中每一个像素的内容：其语义区域的标注和预测是像素级的。与目标检测相比，语义分割标注的像素级的边框显然更加精细。

![../_images/segmentation.svg](https://zh-v2.d2l.ai/_images/segmentation.svg)

`Pascal VOC2012 语义分割数据集`

`小结`

- 语义分割通过将图像划分为属于不同语义类别的区域，来识别并理解图像中像素级别的内容。
- 语义分割的一个重要的数据集叫做Pascal VOC2012。
- 由于语义分割的输入图像和标签在像素上一一对应，输入图像会被随机裁剪为固定尺寸而不是缩放。

- 多应用于自动驾驶与医疗图像

## 转置卷积

- 卷积无法增大输入的高宽，通常要么不变，要么减半
- 转置卷积则用来增大输入的高宽

`2×2的输入张量计算卷积核为2×2的转置卷积`

![../_images/trans_conv.svg](https://zh-v2.d2l.ai/_images/trans_conv.svg)

`填充，步幅，多通道`

在转置卷积中

- 填充被应用于的输出（常规卷积将填充应用于输入）。 例如，当将高和宽两侧的填充数指定为1时，转置卷积的输出中将删除第一和最后的行与列。

- 步幅被指定为中间结果（输出），而不是输入。

  <img src="https://zh-v2.d2l.ai/_images/trans_conv_stride2.svg" alt="../_images/trans_conv_stride2.svg" style="zoom:67%;" />

- 卷积核为2×2，步幅为2的转置卷积。阴影部分是中间张量的一部分，也是用于计算的输入和卷积核张量元素。

`小结`

- 本质还是卷积，卷积使其变小，转置卷积相反，使其变大
- 转置卷积还原的是通道大小，即矩阵大小size，不是值的还原
  - 例如，4×4输入卷积3×3卷积核得到2×2输出
  - 反过来，输出变输入2×2，通过3×3，得到4×4
  - <img src="https://img-blog.csdnimg.cn/1e7f5e8d17494f0b9104898ab345acdb.gif" alt="img" style="zoom: 80%;" />
- 与卷积是做下采样（缩小图像）不同，它通常用作上采样（放大图像）
- 与反卷积不同，反卷积是逆运算

## 全连接卷积神经网络（FCN）

> *全卷积网络*（fully convolutional network，FCN）采用卷积神经网络实现了从图像像素到像素类别的变换，全卷积网络将中间层特征图的高和宽变换回输入图像的尺寸：这是通过引入的*转置卷积*（transposed convolution）实现的。

![../_images/fcn.svg](https://zh-v2.d2l.ai/_images/fcn.svg)

- FCN是深度神经网络做语义分割的奠基性工作
- 它用转置卷积层替换CNN最后的全连接层，从而实现每个像素的预测

`小结`

- 全卷积网络先使用**卷积神经网络抽取图像特征**，然后通过**1×1卷积层**将通道数变换为**类别个数**，最后通过**转置卷积层**将特征图的高和宽变换为输入图像的尺寸。
- 在全卷积网络中，我们可以将转置卷积层**初始化**为**双线性插值的上采样**。

## 样式迁移

> 将样式图片中的样式迁移到内容图片上，得到合成图片

- 使用卷积神经网络，自动将一个图像中的风格应用在另一图像之上，即*风格迁移*

![../_images/style-transfer.svg](https://zh-v2.d2l.ai/_images/style-transfer.svg)

`方法`

1. 首先，我们初始化合成图像，例如将其初始化为内容图像。 该合成图像是风格迁移过程中唯一需要更新的变量，即风格迁移所需迭代的模型参数。
2. 然后，我们选择一个**预训练的卷积**神经网络来**抽取图像的特征**，其中的模型参数在训练中无须更新。
3. 这个深度卷积神经网络凭借多个层逐级抽取图像的特征，我们可以选择其中某些层的输出作为内容特征或风格特征。

![../_images/neural-style.svg](https://zh-v2.d2l.ai/_images/neural-style.svg)

> 实线箭头和虚线箭头分别表示前向传播和反向传播

- 前向传播（实线箭头方向）计算风格迁移的损失函数
- 反向传播（虚线箭头方向）迭代模型参数，即不断更新合成图像

风格迁移损失函数由三部分组成：

1. *内容损失*使合成图像与内容图像在内容特征上接近； 
2. *风格损失*使合成图像与风格图像在风格特征上接近； 
3. *全变分损失*则有助于减少合成图像中的噪点



# 循环神经网络

## 序列模型

处理序列数据需要**统计工具**和新的深度神经网络架构

`自回归模型`

![../_images/sequence-model.svg](https://zh-v2.d2l.ai/_images/sequence-model.svg)

`小结`

- **内插法**（在现有观测值之间进行估计）和**外推法**（对超出已知观测范围进行预测）在实践的难度上差别很大。因此，对于你所拥有的序列数据，在训练时始终要尊重其时间顺序，即最好**不要基于未来的数据**进行训练。
- 序列模型的估计需要专门的统计工具，两种较流行的选择是**自回归模型**和**隐变量自回归模型**。
- 对于时间是向前推进的因果模型，正向估计通常比反向估计更容易。
- 对于直到时间步t的观测序列，其在时间步t+k的预测输出是“k步预测”。随着我们对预测时间k值的增加，会造成误差的快速累积和预测质量的极速下降。

## 文本预处理

- 文本是序列数据的一种最常见的形式之一。
- 为了对文本进行预处理，我们通常将**文本拆分为词元**，构建词表将**词元**字符串映射为**数字索引**，并将**文本数据**转换为**词元索引**以供模型操作。

## 语言模型

- 给定文本序列，语言模型的目标是估计联合概率
- 应用
  - 预训练模型（BERT，GPT-3）
  - 生成文本，给定一些词，不断预测，持续生成后面的词汇
  - 判断多个序列中哪个更常见，语音识别

`小结`

- 语言模型是自然语言处理的关键。
- n元语法通过截断相关性，为处理长序列提供了一种实用的模型。
- 长序列存在一个问题：它们很少出现或者从不出现。
- 齐普夫定律支配着单词的分布，这个分布不仅适用于一元语法，还适用于其他n元语法。
- 通过拉普拉斯平滑法可以有效地处理结构丰富而频率不足的低频词词组。
- 读取长序列的主要方式是随机采样和顺序分区。在迭代过程中，后者可以保证来自两个相邻的小批量中的子序列在原始序列上也是相邻的。

## RNN

> *循环神经网络*（recurrent neural networks，RNNs） 是具有隐状态的神经网络。 在介绍循环神经网络模型之前， 我们首先回顾 [4.1节](https://zh-v2.d2l.ai/chapter_multilayer-perceptrons/mlp.html#sec-mlp)中介绍的多层感知机模型。

`困惑度`（衡量语言模型好坏的标准）

- 使用平均交叉熵

困惑度的最好的理解是“**下一个词元的实际选择数的调和平均数**”。 我们看看一些案例：

- 在最好的情况下，模型总是完美地估计标签词元的概率为1。 在这种情况下，模型的困惑度为1。
- 在最坏的情况下，模型总是预测标签词元的概率为0。 在这种情况下，困惑度是**正无穷大**。
- 在基线上，该模型的预测是词表的所有可用词元上的均匀分布。 在这种情况下，困惑度等于词表中唯一词元的数量。 事实上，如果我们在没有任何压缩的情况下存储序列， 这将是我们能做的最好的编码方式。 因此，这种方式提供了一个重要的上限， 而任何实际模型都必须超越这个上限。

### 梯度裁剪

梯度爆炸的一种常见且相对容易的解决方案是：

- 在通过网络向后传播误差并使用其更新权重之前，**更改误差的导数**。 两种方法包括：
  - 给定选定的向量范数（ vector norm）来重新缩放梯度；
  - 以及裁剪超出预设范围的梯度值。 这些方法一起被称为梯度裁剪（gradient clipping）。

`小结`

- 对隐状态使用循环计算的神经网络称为循环神经网络（RNN）。
- 循环神经网络的输出取决于输入和前一时间的隐变量
- 应用到语言模型中时，循环神经网络根据当前词预测下一次时刻词
- 使用困惑度来评价语言模型的质量。

## 门控循环单元（GRU）

`重置门和更新门`

- 重置门允许我们控制“可能还想记住”的过去状态的数量；（能关注） 

- 更新门将允许我们控制新状态中有多少个是旧状态的副本。（能遗忘）

![../_images/gru-1.svg](https://zh-v2.d2l.ai/_images/gru-1.svg)

输入是由当前时间步的输入和前一时间步的隐状态给出。 两个门的输出是由使用sigmoid激活函数的两个全连接层给出。

`候选隐状态`

![../_images/gru-2.svg](https://zh-v2.d2l.ai/_images/gru-2.svg)

总之，门控循环单元具有以下两个显著特征：

- 重置门有助于捕获序列中的短期依赖关系。
- 更新门有助于捕获序列中的长期依赖关系。

## 长短期记忆网络（LSTM）

> 长短期记忆网络的设计灵感来自于计算机的逻辑门;

<img src="https://zh-v2.d2l.ai/_images/lstm-0.svg" alt="../_images/lstm-0.svg" style="zoom: 80%;" />

- 忘记门：将值朝0减少
- 输入门：决定是否忽略掉数据
- 输出门：决定是不是使用隐状态

就如在门控循环单元中一样， 当前时间步的输入和前一个时间步的**隐状态**作为数据送入长短期记忆网络的门中， 如图所示。 它们由三个具有sigmoid激活函数的全连接层处理， 以计算输入门、遗忘门和输出门的值。 因此，这三个门的值都在(0,1)的范围内。

`隐状态`

![../_images/lstm-3.svg](https://zh-v2.d2l.ai/_images/lstm-3.svg)

`小结`

- 长短期记忆网络有三种类型的门：输入门、遗忘门和输出门。
- 长短期记忆网络的隐藏层输出包括“隐状态”和“记忆元”。只有隐状态会传递到输出层，而记忆元完全属于内部信息。
- 长短期记忆网络可以缓解梯度消失和梯度爆炸。

## 深层循环神经网络

![../_images/deep-rnn.svg](https://zh-v2.d2l.ai/_images/deep-rnn.svg)

该图为深度循环神经网络结构，有多个隐藏层

`总结`

- 深度循环神经网络使用多个隐藏层来获得更多的非线性性

- 在深度循环神经网络中，隐状态的信息被传递到当前层的下一时间步和下一层的当前时间步。
- 有许多不同风格的深度循环神经网络， 如长短期记忆网络、门控循环单元、或经典循环神经网络。 这些模型在深度学习框架的高级API中都有涵盖。
- 总体而言，深度循环神经网络需要大量的调参（如学习率和修剪） 来确保合适的收敛，模型的初始化也需要谨慎。

## 双向循环神经网络

如果我们希望在循环神经网络中拥有一种机制， 使之能够提供与隐马尔可夫模型类似的前瞻能力， 我们就需要修改循环神经网络的设计。

概念上， 只需要增加一个“从**最后一个词元开始从后向前运行**”的循环神经网络， 而不是只有一个在前向模式下“从第一个词元开始运行”的循环神经网络。

*双向循环神经网络*（bidirectional RNNs） 添加了**反向传递信息的隐藏层**，以便更灵活地处理此类信息。

![../_images/birnn.svg](https://zh-v2.d2l.ai/_images/birnn.svg)

该图为具有单个**隐藏层**的**双向**循环神经网络的架构。

- 隐藏层有两个
  - 前向RNN隐层
  - 反向RNN隐层
- 合并两个隐状态得到输出

`小结`

- 通过反向更新的隐藏层来利用方向时间信息
- 通常用来对序列抽取特征，填空，而不是预测未来
- 在双向循环神经网络中，每个时间步的隐状态由当前时间步的前后数据同时决定。
- 双向循环神经网络与概率图模型中的“前向-后向”算法具有相似性。
- 双向循环神经网络主要用于序列编码和给定双向上下文的观测估计。
- 由于梯度链更长，因此双向循环神经网络的训练代价非常高。

## 机器翻译与数据集

> **语言模型**是自然语言处理的关键， 而***机器翻译***是语言模型最成功的基准测试。 因为机器翻译正是将输入序列转换成输出序列的 *序列转换模型*（sequence transduction）的核心问题。
>
> ***机器翻译***（machine translation）指的是 将序列从一种语言自动翻译成另一种语言。

`神经网络机器翻译方法`

- 强调的是端到端的学习
- 机器翻译的数据集是由源语言和目标语言的文本序列对组成的

`小结`

- 机器翻译指的是将文本序列从一种语言自动翻译成另一种语言。
- 使用单词级词元化时的词表大小，将明显大于使用字符级词元化时的词表大小。为了缓解这一问题，我们可以**将低频词元视为相同的未知词元**。
- 通过截断和填充文本序列，可以保证所有的文本序列都具有相同的长度，以便以小批量的方式加载。

# 编码器-解码器

> *编码器*（encoder）： 它接受一个长度可变的序列作为输入， 并将其转换为具有固定形状的编码状态。 
>
> *解码器*（decoder）： 它将固定形状的编码状态映射到长度可变的序列。

![../_images/encoder-decoder.svg](https://zh-v2.d2l.ai/_images/encoder-decoder.svg)

- 该模型中，编码器负责表示输入，解码器负责输出

`小结`

- “编码器－解码器”架构可以将长度可变的序列作为输入和输出，因此适用于机器翻译等序列转换问题。
- 编码器将长度可变的序列作为输入，并将其转换为具有固定形状的编码状态。
- 解码器将具有固定形状的编码状态映射为长度可变的序列。

> 细节：
>
> - 编码器没有输出
> - 编码器最后时间步的隐状态用作解码器的初始隐状态

## 序列到序列学习（seq2seq）

在机器翻译中使用两个RNN进行seq2seq

![../_images/seq2seq.svg](https://zh-v2.d2l.ai/_images/seq2seq.svg)

- 编码器是一个RNN，读取输入句子
  - 可以双向
- 解码器使用另外一个RNN来输出

`衡量序列评估好坏`

BLEU（bilingual evaluation understudy） 最先是用于评估机器翻译的结果， 但现在它已经被广泛用于测量许多应用的输出序列的质量。 原则上说，对于预测序列中的任意n元语法（n-grams）， BLEU的评估都是这个n元语法是否出现在标签序列中。

BLEU定义

<img src="C:\Users\zzy\AppData\Roaming\Typora\typora-user-images\image-20221204111207068.png" alt="image-20221204111207068" style="zoom:80%;" />

`总结`

- seq2seq从一个句子生成另一个句子
- 编码器和解码器都是RNN
- 编码器最后时间隐状态给到解码器初始隐状态来完成信息传递
- 使用BLEU来衡量生成序列的好坏

# 01-Regression

##  Machine Learning

> 概况：机器学习就是让机器具备找一个函式的能力。

##  三个机器学习任务

- Regression：要找的函式,他的输出是一个数值
- Classification:函式的输出,就是从设定好的选项裡面,选择一个当作输出
- Structured Learning:机器产生有结构的东西的问题——学会创造

## 找函式的过程：三个步骤

1. 带有未知参数的函式

   - 带有Unknown的Parameter的Function => model

2. 定义一个东西叫做Loss(损失函数)

   - Loss它也是一个Function,那这个Function它的输入,是 我们Model裡面的参数
     - L越大,代表一组参数越不好,这个大L越小,代表现在这一组参数越好
     - 计算方法：求取估测的值跟实际的值（Label） 之间的差距
       - MAE(mean absolute error)——平均绝对误差
       - ![{\displaystyle \mathrm {MAE} ={\frac {\sum _{i=1}^{n}\left|y_{i}-x_{i}\right|}{n}}={\frac {\sum _{i=1}^{n}\left|e_{i}\right|}{n}}.}](https://wikimedia.org/api/rest_v1/media/math/render/svg/3ef87b78a9af65e308cf4aa9acf6f203efbdeded)
       - MSE(mean square error)——均方误差
       - ![img](https://bkimg.cdn.bcebos.com/formula/845ae0515359869c505b869451eb4777.svg)
       - Cross-entropy:计算**概率分布**之间的差距——交叉熵
   
3. Optimization(优化)

   - 找到能让损失函数值最小的参数

   - 具体方法：Gradient Descent（梯度下降）

     1. 随机选取初始值 $w_0$

     2. 计算在 $w=w_0$的时候,*w*这个参数对*loss*的微分是多少

     3. 根据微分（梯度）的方向，改变参数的值

        - **改变的大小取决于：**
          1. 斜率的大小
          2. 学习率的大小**（超参数）**

     4. 什么时候停下来？

        - 自己设置上限**（超参数）**

        - 理想情况：微分值为0（极小值点），不会再更新⇒有可能陷入局部最小值，不能找到全局最小值

          **事实上：局部最小值不是真正的问题！！！**

##  Liner Model（线性模型）

==Sigmoid函数==

公式：

![{\displaystyle S(t)={\frac {1}{1+e^{-t}}}.}](https://wikimedia.org/api/rest_v1/media/math/render/svg/a26a3fa3cbb41a3abfe4c7ff88d47f0181489d13)

​	

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F18d619c4-5e89-4888-9f05-45e5e181d27f%2FUntitled.png?table=block&id=9300df07-51a3-4c32-805f-bd88c8e6c9e1&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom: 67%;" />

- Update：每次更新一次参数叫做一次 Update,
- Epoch：把所有的 Batch 都看过一遍,叫做一个 Epoch

模型变型⇒ReLU（Rectified Linear Unit，线性整流单元）

- 把两个 ReLU 叠起来,就可以变成 Hard 的 Sigmoid

==Softmax==

公式：

![{\displaystyle \sigma (\mathbf {z} )_{j}={\frac {e^{z_{j}}}{\sum _{k=1}^{K}e^{z_{k}}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e348290cf48ddbb6e9a6ef4e39363568b67c09d3)  for *j* = 1, …, *K*.

**归一化指数函数。**它是二分类函数[sigmoid](https://so.csdn.net/so/search?q=sigmoid&spm=1001.2101.3001.7020)在多分类上的推广，目的是将多分类的结果以概率的形式展现出来。

> 多类分类模型
>
> ​		使用Softmax 操作子得到每个类的预测置信度
>
> ​		使用交叉熵来衡量预测和标号的区别

# 02.1-DeepLearning-General Guidance

![img](https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6586a506-6ac8-43ad-a1d9-28e6de0b1fdb%2FUntitled.png?table=block&id=53a2d275-3157-4673-93d0-c137a8465213&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2)

##  如何使模型达到更好的效果？

1. 分析在训练数据上的Loss

   - Model Bias

     - 所有的function集合起来,得到一个function的set.但是这个function的set太小了,没有包含任何一个function,可以让我们的loss变低⇒**可以让loss变低的function,不在model可以描述的范围内。**

       ⇒解决方法：重新设计一个Model，**一个更复杂的、更有弹性的、有未知参数的、需要更多features的function

   - Optimization

   - 区分两种情况

     - 看到一个你从来没有做过的问题,也许你可以先跑一些比较小的,比较浅的network,或甚至用一些,不是deep learning的方法⇒比较容易做Optimize的,它们比较不会有optimization失败的问题
     - 如果你发现你深的model,跟浅的model比起来,深的model明明弹性比较大,但loss却没有办法比浅的model压得更低,那就代表说你的optimization有问题

2. 分析测试数据上的Loss

   - Overfitting：training的loss小,testing的loss大,这个有可能是overfitting

     如果你的model它的**自由度很大**的话,它可以**产生非常奇怪的曲线**,导致训练集上的结果好,但是测试集上的loss很大

   - 解决方法

     - 增加训练集 
     - 限制模型，使其弹性不那么大
       - 给它**比较少的参数（比如神经元的数目）；模型**共用参数**
       - 使用**比较少的features**
       - Early Stopping
       - Regularization
       - Dropout

==选出有较低testing-loss的模型==

- Cross Validation（交叉验证）

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F5782b3e5-2e99-488f-ba63-aaec6b59631d%2FUntitled.png?table=block&id=89a42d1a-8be3-41f5-923a-95f55307fb0d&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:50%;" />

1. 把Training的资料分成两半,一部分叫作Training Set,一部分是Validation Set
2. 在Validation Set上面,去衡量它们的分数,你根据Validation Set上面的分数,去挑选结果，不要管在public testing set上的结果，避免overfiting（过拟合）

> 总结
>
> - 训练数据集：训练模型参数
>
> - 验证数据集：选择模型超参数
> - 非大数据集上通常使用k-折交叉验证
>
> > k折交叉验证（ k-Folder Cross Validation），经常会用到的。 k折交叉验证先将数据集 D随机划分为 k个大小相同的互斥子集，即 ，每次随机的选择 k-1份作为训练集，剩下的1份做测试集。当这一轮完成后，重新随机选择 k份来训练数据。若干轮（小于 k ）之后，选择损失函数评估最优的模型和参数。注意，**交叉验证法评估结果的稳定性和保真性在很大程度上取决于 k取值**。

==分验证集==

- N-fold Cross Validation （N倍交叉验证）

  **N-fold Cross Validation**就是你先把你的训练集切成N等份,在这个例子里面我们切成N等份,切完以后,你拿其中一份当作Validation Set,另外N-1份当Training Set,重复N次

  把这多个模型,在这三个setting下,通通跑过一次,把三种状况的结果都平均起来,看看谁的结果最好；最后再把选出来的model（这里是model 1）,用在全部的Training Set上,训练出来的模型,再用在Testing Set上面

## 权重衰退

模型在训练的过程中可能**过拟合**，这一般是由于数据复杂度太低而模型容量太大导致的，简而言之就是数据太简单，模型太复杂，模型学习到了数据的一切，包括噪音。此时，权重往往会很大（受噪音影响），显然模型并没有训练到最优（虽然它记住了训练数据的一切，但是对于新的样本泛化能力很差）。所以，**我们想要适当降低权重，使模型接近最优，这样模型的泛化性能提升就适当的解决了过拟合问题，这就是权重衰退。**

<img src="https://img-blog.csdnimg.cn/8e27156290e74d158ebd777a585b9c6b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5qmZ5a2Q5ZCWMjE=,size_17,color_FFFFFF,t_70,g_se,x_16" alt="img" style="zoom:67%;" />

> 总结
>
> - 权重衰退通过L2正则项使得模型参数不会过大，从而控制模型复杂度。
>
> - 正则项权重是控制模型复杂度的超参数。

## 丢弃法

2012年，Alex、Hinton在其论文《ImageNet Classification with Deep Convolutional Neural Networks》中用到了Dropout算法，用于防止过拟合。

Dropout可以作为训练深度神经网络的一种正则方法供选择。在每个训练批次中，通过忽略一部分的神经元（**让其隐层节点值为0**），可以明显地减少过拟合现象。**这种方式可以减少隐层节点间的相互作用**，高层的神经元需要低层的神经元的输出才能发挥作用，如果**高层神经元过分依赖某个低层神经元，就会有过拟合**发生。在一次正向/反向的过程中，通过随机丢弃一些神经元，迫使高层神经元和其它的一些低层神经元协同工作，可以有效地防止神经元因为接收到过多的同类型参数而陷入过拟合的状态，来提高泛化程度。

正常：

<img src="https://microsoft.github.io/ai-edu/%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B/A2-%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86/%E7%AC%AC7%E6%AD%A5%20-%20%E6%B7%B1%E5%BA%A6%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/img/16/dropout_before.png" alt="img" style="zoom: 67%;" />

丢弃后：

<img src="https://microsoft.github.io/ai-edu/%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8B/A2-%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86/%E7%AC%AC7%E6%AD%A5%20-%20%E6%B7%B1%E5%BA%A6%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/img/16/dropout_after.png" alt="img" style="zoom:67%;" />

> 总结
>
> - 丢弃法将一些输出项随机置0来控制模型复杂度
> - 常作用在多层感知机（MLP）的隐藏层输出上
> - 丢弃概率是控制模型复杂度的超参数

## 批量归一化

- 最初论文目的是减少内部协变量转移（作者所说的“内部协变量转移”类似于上述的投机直觉，即**变量值的分布在训练过程中会发生变化**）
- 后续论文指出它是通过在每个小批量加入噪音，来控制模型复杂度

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20221023111910705.png" alt="image-20221023111910705" style="zoom:67%;" />

> 理解：在每一层输入和上一层输出之间加入了一个新的计算层，对数据的分布进行额外的约束，从而增强模型的泛化能力。 但是批量归一化同时也降低了模型的拟合能力，归一化之后的输入分布被强制拉到均值为0和标准差为1的正态分布上来。
>
> 
>
>  作用是持续加深深层网络的收敛速度，这使得研究人员能够训练100层以上的网络

- 批量规范化和其他层之间的一个关键区别是
  - 由于批量规范化在完整的小批量上运行，因此我们不能像以前在引入其他层时那样忽略批量大小。

- 可学习的参数为γ-缩放（gamma）和β-偏移（beta）
- 作用在
  - 全连接层和卷积层输出上，激活函数前
  - 全连接层和卷积层输入上
- for 全连接层，作用在特征维
- for 卷积层，作用在通道维

`代码`

```python
def batch_norm(X, gamma, beta, moving_mean, moving_var, eps, momentum):
    # 通过is_grad_enabled来判断当前模式是训练模式还是预测模式
    if not torch.is_grad_enabled():
        # 如果是在预测模式下，直接使用传入的移动平均所得的均值和方差
        X_hat = (X - moving_mean) / torch.sqrt(moving_var + eps)
    else:
        assert len(X.shape) in (2, 4)
        if len(X.shape) == 2:
            # 使用全连接层的情况，计算特征维上的均值和方差
            mean = X.mean(dim=0)
            var = ((X - mean) ** 2).mean(dim=0)
        else:
            # 使用二维卷积层的情况，计算通道维上（axis=1）的均值和方差。
            # 这里我们需要保持X的形状以便后面可以做广播运算
            mean = X.mean(dim=(0, 2, 3), keepdim=True)
            var = ((X - mean) ** 2).mean(dim=(0, 2, 3), keepdim=True)
        # 训练模式下，用当前的均值和方差做标准化
        X_hat = (X - mean) / torch.sqrt(var + eps)
        # 更新移动平均的均值和方差
        moving_mean = momentum * moving_mean + (1.0 - momentum) * mean
        moving_var = momentum * moving_var + (1.0 - momentum) * var
    Y = gamma * X_hat + beta  # 缩放和移位
    return Y, moving_mean.data, moving_var.data
```

`小结`

- 批量归一化固定小批量中的均值和方差，然后学习出适合的偏移和缩放
- 加速收敛速度，但一般不改变模型精度

# 02.2-类神经网络优化技巧

## critical point 概述

==临界点==：梯度 （grad） 为 0 的点

loss,无法再下降,也许是因为卡在了critical point ⇒ local minima（局部最小值） OR saddle point（鞍点）

- 局部最小值 -> 可能无路可走
- 鞍点 -> 旁边还是有路可走

`判断θ附近loss的梯度 -> 泰勒展开 -> 海塞矩阵`

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcad56cb3-6fc6-4488-b1cd-b567fea2075c%2FUntitled.png?table=block&id=1038a41a-a019-41ec-9e08-2e9a062e9bfc&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:50%;" />

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F515415e7-dcaa-499c-8dcb-9268061f824e%2FUntitled.png?table=block&id=5d24c4da-36ca-43f5-9ea0-1c8935326271&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:50%;" />

- g：一次微分，当到达临界点时，表示g为0，则绿色项不存在，原式等于第一项加红色项

- H：二次微分，根据红色项来判断临界点为哪种情况

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc88a449d-25e8-4c35-9e3f-5afe3e46e7b0%2FUntitled.png?table=block&id=15c6ddd8-63da-45eb-8499-085c395e3542&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom: 67%;" />

- 红色部分 大于0，左边永远大于右边 L(θ`)，所以该点为最小值点
- 红色部分 小于0，左边永远小于右边 L(θ`)，所以该点为最大值点
- 红色部分有时大于0有时小于0，则为鞍点

> 注：如果走到鞍点，可以利用H的特征向量确定参数的更新方向
>
> 令特征值小于0，得到对应的特征向量u,在θ`的位置加上u,沿著u的方向做update得到θ,就可以让loss变小。
>
> > Local Minima比Saddle Point少的多

###  Small Batch v.s. Large Batch

`batch_size 取大取小的关系：`

- 结论1：使用较小的BatchSize，在更新参数时会有Noisy（图中曲线弯弯折折）⇒有利于训练
  
  - 不同的Batch 求得的Loss略有差异，可以避免局部极小值“卡住”
  
  ![img](https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9ec2c77d-56dd-4f99-8efc-782ffa48586f%2FUntitled.png?table=block&id=b876aaa1-e583-4960-997f-2a5d62968d22&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2)

- 结论2：使用较小的BatchSize,可以避免Overfitting ⇒ 有利于测试(Testing)
  
  - SB 在数据集（测试集）表现更好
  
  <img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F149f4354-b2a9-41ee-bf78-a3e96d1ca042%2FUntitled.png?table=block&id=91fc00ba-88ff-4e1b-bcf4-2512b8047204&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom: 80%;" />
  
- 总结：BatchSize是一个需要调整的参数，它会影响训练速度与优化效果。

  <img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F3158ec11-5567-4ef4-a7d6-79d3db676675%2FUntitled.png?table=block&id=6160f7de-7bcc-4299-8e1d-39cf98246302&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=1090&userId=&cache=v2" alt="img" style="zoom: 80%;" />

### Momentum（动量）

> 所谓的 Momentum, Update 的方向不是只考虑现在的 Gradient,而是考虑过去所有 Gradient 的总合

- Vanilla Gradient Descent（一般的梯度下降）⇒ 只考虑梯度的方向，向反方向移动
- Gradient Descent + Momentum（考虑动量）⇒ 综合梯度+前一步的方向

### 总结 - 关于“小梯度”

- 临界点的梯度为 0 
- 临界值点可以是鞍点或者局部最小值点
  - 可以通过海塞矩阵确定
  - 局部最小值可能很少见
  - 有可能沿着海塞矩阵的特征向量的方向逃出鞍点
- 较小的批次规模和动量有利于避开临界点

### 自适应学习率

`Error Surface`: 根据不同参数，计算Loss得到的等高线图

- Training  stuck 训练卡住或暂停不一定是最小梯度 -> Loss不再下降时，未必说明到达临界点，梯度可能还很大

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb0915a78-64fd-4120-87b1-2a03ca9f3318%2FUntitled.png?table=block&id=6c742f56-dd7f-4176-b101-1aedf57fe0ab&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:67%;" />

- 客制化“学习率”
  - 较大的学习率：Loss在山谷的两端震荡而不会下降
  - 较小的学习率：梯度较小时几乎难以移动

> 客制化“梯度” ⇒  不同的参数（大小）需要不同的学习率
>
> > **基本原则：**
> >
> > - 某一个方向上gradient的值很小,非常的平坦⇒learning rate调大一点,
> > - 某一个方向上非常的陡峭,坡度很大⇒learning rate可以设得小一点

`Adagrad`: 每次更新的𝜂就是等于前一次的𝜂再除以𝜎^t，而 σ^t则代表的是第 t 次以前的所有梯度更新值之平方和开根号(root mean square)。

- gradient都比较大,σ就比较大,在update的时候 参数update的量就比较小。
  - 缺陷 ⇒ 不能“实时”考虑梯度的变化情况

`RMSProp`:

添加参数$\alpha$，越大说明过去的梯度信息**更重要**

- α设很小趋近於0,就代表这一步算出的gᵢ相较於之前所算出来的gradient而言比较重要
- α设很大趋近於1,就代表现在算出来的gᵢ比较不重要,之前算出来的gradient比较重要

`最常用的策略：Adam=RMSProp + Momentum`

​	Adam其实就是加了momentum的RMSProp，下图的公式mt代表的是momentum，就是前一个时间点的movement（momentum动量），vt就是RMSProp裡的σ，式子虽然看起來很复杂，但其實跟RMSProp很類似，

​	每次更新都會调整新的gradient的比重。所以**Adam**继承两者的优点，适合大部分的状况，为目前最常使用的优化方法。

<img src="https://miro.medium.com/max/1400/1*EmEGVj6OvV0kEdsT-nyIlg.png" alt="img" style="zoom:50%;" />![img](https://miro.medium.com/max/780/1*_oz4zR8-sUB6a7lf3j42Bw.png)

> Learning Rate Decay：随著时间的不断地进行,随著参数不断的update,η越来越小

`Warm Up⇒让learning rate先变大后变小`

- 在一开始，**σ^t**下标 **i **的估计值有很大的方差

- σ指示某一个方向它到底有多陡/多平滑,这个统计的结果,要看得够多笔数据以后才精准,所以一开始我们的统计是不精準的。一开始learning rate比较小，是让它探索收集一些有关error surface的情报，在这一阶段使用较小的learning rate，限制参数不会走的离初始的地方太远；等到σ统计得比较精準以后再让learning rate慢慢爬升

### 总结

1. 使用动量，考虑过去的梯度**“大小”与“方向”**
2. 引入$\sigma$,考虑过去梯度的“**大小**”（RMS）
3. 使用LearningRate Schedule

### 另一种思路

- 将Error Surface“铲平”  ⇒  Batch Normalization 批次标准化（归一化）
- **解决：给不同的 dimension同样的数值范围  ⇒  Feature Normalization(归一化)**
  - 一种Standardization（标准化）方法：对不同数据样本向量的**同一维**进行归一化
  - 做完 normalize 以后,这个 dimension 上面的数值就会平均是 0,然后它的 variance就会是 1,所以**这一排数值的分布就都会在 0 上下**
  - 对每一个 dimension都做一样的 normalization,就会发现所有 feature 不同 dimension 的数值都在 0 上下,那你可能就可以**制造一个,比较好的 error surface**
- 在深度学习中，每一层都需要一次Normalization
  - 对向量的对应element做求平均、标准差的运算，求得向量$\mu,\sigma$
  - 对每个向量$z$,利用$\mu,\sigma$对对应element进行归一化，得到$\tilde{z}$
  - 继续后续的步骤

> 注意理解：“对一批z数据进行归一化”  ⇒  “网络”模型变为能够一次处理“一批x数据”的模型，数据之间相互关联
>
> Batch Normalization：实际上做Normalization时，只能考虑有限数量的数据⇒考虑一个Batch内的数据⇒近似整个数据集
>
> > Batch Normalization适用於 batch size 比较大时。其中data可以认为足以表示整个 corpus 的分布；从而，将对整个 corpus做 Feature Normalization 这件事情,改成只在一个 batch中做 Feature Normalization作为近似



# 02.3-DeepLearning-Loss of Classification

> 各种损失函数的建议

## Classification as Regression

`Classification with softmax`

![img](https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F96fb2865-7d3f-42ea-acee-6da18cdd6260%2FUntitled.png?table=block&id=60e4d4be-6caf-4908-8b47-02633b3d4976&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2)

我们的目标只有0跟1,而y有任何值,我们就使用Softmax先把它Normalize到0到1之间,这样才好跟 label 的计算相似度

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff52d8d7e-0ba4-4e96-8109-6eb741ec3a2d%2FUntitled.png?table=block&id=15c46703-192d-46ae-9e96-4918fd19edcb&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:67%;" />

经过计算后：

- 输出值变成0到1之间
- 输出值的和為1
- 原本大的值跟小的值的**差距更大**

Softmax的输入，称作**Logit**

> 二分类：使用sigmoid与softmax是等价的

## 优化目标：减小y^和y'之间的差距e

- 在分类问题上，交叉熵相比较 MSE 更加适合

- > 注：在Pytorch中，softmax 函数被内建在Cross-enrtopy 损失函数中

- 从优化角度出发进行讨论，使用MSE时，左上角的位置虽然Loss很大，但梯度平坦，难以优化；而Cross-entropy则更易收敛⇒改变Loss函数，也会影响训练的过程

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F1f6dae63-ffbc-41c6-83ee-bcf286ef2764%2FUntitled.png?table=block&id=e028b452-d68c-4a12-be9e-42962d24b75a&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom: 80%;" />



# 03-CNN

==网络模型的架构设计思想==

## 场景： 图片分类

- 一般过程：
  - 把所有图片都先 Rescale 成大小一样
  - 把每一个类别,表示成一个 One-Hot 的 Vector（Dimension 的长度就决定了模型可以辨识出多少不同种类的东西,）
  - 将图像【输入】到模型中
- 如何将图片输入到模型中？⇒  一般思路：展平→参数量过大

## 神经元角度

`观察（1）：模型通过识别一些“特定模式”来识别物体，而非“整张图”`

`简化（1）：设定“感受野”（Receptive Field）`

- 每个神经元只需要考察特定范围内的图像信息，将图像内容展平后输入到神经元中即可

  - 感受野之间可以重叠
  - 一个感受野可以有多个神经元“守备”
  - 感受野大小可以“有大有小”
  - 感受野可以只考虑某一些Channel
  - 感受野可以是“长方形”的
  - 感受野不一定要“相连”

- 感受野的基本设置

  - 看所有的Channel

    一般在做影像辨识的时候会看全部的 Channel。那么，在描述一个 Receptive Field 的时候,无需说明其Channel数，只要讲它的**高、宽⇒Kernel Size**

    一般不做过大的kernal Size，**常常设定为**$3\times3$

  - 每个感受野会有**不止一个神经元进行“守备”⇒输出通道数/卷积核数目**

  - 不同的感受野之间的关系⇒感受野的平移位移：stride【hyperparameter】

    一般希望感受野之间有重叠，避免交界处的pattern被忽略

  - 感受野超出影响的范围⇒padding（补值）

    补0；补平均值；补边缘值……

  - 垂直方向移动

`观察（2）：同样的pattern，可能出现在图片的“不同位置”`

`简化（2）：不同 Receptive Field 的 Neuron共享参数⇒ Parameter Sharing权值共享`

- 对每个感受野，都使用一组相同的神经元进行守备；这一组神经元被称作filter，对不同感受野使用的filter参数相同



### 卷积层的优势

> 卷积层是“受限”（弹性变小）的FC（全连接层）

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff84607df-a9c7-4df9-a346-6e64b748e790%2FUntitled.png?table=block&id=32d002ac-b6d6-46b1-8658-e6a1b1ede39c&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:67%;" />

- FC可以通过“学习”决定要看到的“图片”的范围。加上“感受野”概念后，就只能看某一个范围。
- FC可以自由决定守备不同“感受野”的各个神经元参数。加上“权值共享”概念后，守备不同感受野的**同一个滤波器（filter）参数相同。**

**分析：**

- 一般而言，Model Bias 小,Model 的 Flexibility 很高的时候,它比较容易 Overfitting,Fully Connected Layer可以做各式各样的事情,它**可以有各式各样的变化**,但是它可能没有办法在任何**特定的任务**上做好
- CNN 的 Bias 比较大，它是专门為影像设计的，所以它在影像上仍然可以做得好。

## 滤波器角度

卷积层中有若干个filter，每个filter可以用来“抓取”图片中的某一种特征（特征pattern的大小，小于感受野大小）。

ilter的参数，其实就是神经元中的“权值（weight）”。

不同的filter扫过一张图片，将会产生“新的图片”，每个filter将会产生图片中的一个channel⇒**feature map**

filter的计算是**“内积”**：filter跟图片对应位置的数值直接相乘，所有的都乘完以后再相加。



`多层卷积`

- 多层卷积⇒让“小”卷积核看到“大”pattern

## 总结

- 对全连接层使用**平移不变性**和**局部性**得到卷积层
- 填充和步幅
  - 这两者是卷积层的超参数
  - **padding**  避免信息损失，填充在输入周围添加额外的行或列，来控制输出形状的减少量，例如输入3×3，输出为4×4
  - **stride**  压缩一部分信息，步幅是每次滑动卷积核窗口的行或列步长，可以**成倍的减少输出形状**

两个角度理解“卷积”：神经元角度（neuron）与滤波器（filter）

|        |    不用看整张图片范围     | 图片不同位置的相同模式pattern  |
| :----: | :-----------------------: | :----------------------------: |
| 神经元 |      只要守备感受野       | 守备不同感受野的神经元共用参数 |
| 滤波器 | 使用滤波器侦测模式pattern |      滤波器“扫过”整张图片      |

## 池化层 Pooling

==图片降采样不影响图片的辨析⇒Pooling（池化）把图片变小，减小运算量==

> Pooling本身没有参数,所以它不是一个 Layer，没有要 Learn 的东西。行为类似于一个 Activation Function,（Sigmoid ， ReLU ），是一个 Operator，行为固定
>
> 分类：
>
> - Max pooling 最大池化层：每个窗口最强的模式信号
> - Avg Pooling 平均池化层：将最大池化层中的“最大”操作替换为“平均”

总结：

- 池化层返回窗口中最大或平均值
- 缓解卷积层对位置的敏感性
- 同样有窗口大小，填充，步幅等超参数

## The whole CNN（典型分类网络结构）

conv-pooling-...（循环）-flatten-FC-softmax

- 一般：卷积与池化交替使用。pooling⇒可有可无（算力支撑）

- 算力足够 ->  无需池化
  Pooling对于 Performance,会带来一点伤害的。如果你运算资源足够支撑你不做 Pooling 的话,很多 Network 的架构的设计,往往今天就不做 Pooling,全 Convolution。

> 最后注意：
>
> CNN 并不能够处理影像放大缩小,或者是旋转的问题。所以在做影像辨识的时候,往往都要做 Data Augmentation（数据增强），把你的训练数据截一小块出来放大缩小、把图片旋转,CNN 才会做到好的结果。
>
> 有一个架构叫 spacial Transformer Layer可以处理。

# 04-Self-attention

## attention

> **Attention机制**就是让`编码器编码出来的向量根据解码器要解码的东西动态变化的一种机制`，貌似来源灵感就是人类视觉在看某一个东西的时候会有选择的针对重要的地方看。

`计算attention`

1. 第一步是将query和每个key进行相似度计算得到权重，常用的相似度函数有点积，拼接，感知机等
2. 第二步一般是使用一个softmax函数对这些权重进行归一化
3. 第三步将权重和相应的键值value进行加权求和得到最后的attention。目前在NLP研究中，key和value常常都是同一个，即**key=value** 

> **Attention**机制发生在Target（输出）的元素Query（查询）和Source（输入）中的所有元素之间。
>
> - 比如entity1，entity2，entity3….，attn会输出[0.1，0.2，0.5，….]这种，告诉你entity3重要些。
>
> 而 **Self-attention**，它指的不是Target和Source之间的Attention机制，而是Source内部元素之间或者Target内部元素之间发生的Attention机制，也可以理解为Target=Source这种特殊情况下的注意力计算机制。**Q=K=V**。
>
> - self attention会给你一个矩阵，告诉你 entity1 和entity2、entity3 ….的关联程度、entity2和entity1、entity3…的关联程度。
>
> 总结：
>
> - attention是source对target的attention
>
> - self attention 是source 对source的attention。
>
> 例如：
>
> Transformer中在计算权重参数时将文字向量转成对应的KQV，只需要在Source处进行对应的矩阵操作，用不到Target中的信息。

## Self-attention v.s. CNN

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff7af7984-e535-4c87-bcc7-b678b6e5402f%2FUntitled.png?table=block&id=bc9d4c30-939d-49bf-b64f-7fa7e6a5a189&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:67%;" />

`两者对比`

- CNN：感知域（receptive field）是人为设定的，只考虑范围内的信息
- Self-attention：考虑一个像素和整张图片的信息⇒自己学出“感知域”的形状和大小

**结论：CNN就是 Self-attention 的特例,Self-attention 只要设定合适的参数,它可以做到跟 CNN 一模一样的事情**

**self attention,是更 flexible 的 CNN**

**⇒self-attention需要更多的数据进行训练，否则会欠拟合；否则CNN的性能更好**

- Self-attention 它弹性比较大,所以需要比较多的训练资料,训练资料少的时候,就会 overfitting
- 而 CNN 它弹性比较小,在训练资料少的时候,结果比较好,但训练资料多的时候,它没有办法从更大量的训练资料得到好处

## Self-attention v.s. RNN（循环神经网络）

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fbbf60895-7a71-416f-b617-003aa847020e%2FUntitled.png?table=block&id=93343e26-2054-43ba-bcf7-102f0e3e8cc1&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:67%;" />

`主要区别：`

- 对 RNN 来说,假设最右边这个黄色的 vector,要考虑最左边的这个输入,那它必须要把最左边的输入存在 memory 裡面都**不能够忘掉**,一路带到最右边,才能够在最后一个时间点被考虑
- 但对 Self-attention 来说没有这个问题,可以在整个 sequence 上非常远的 vector之间**轻易地抽取信息**,所以这是 RNN 跟 Self-attention,一个不一样的地方

**Self-attention 有一个优势,是它可以平行处理所有的输出，效率更高：**

- Self-attention:四个 vector 是平行产生的,并不需要等谁先运算完才把其他运算出来
- RNN 是没有办法平行化的，必须依次产生

# 05-Transformer

## 应用

> 语音，机器翻译，聊天机器人，语音合成

> 由于自注意力同时具有***并行计算***和***最短的最大路径长度***这两个优势，所以想到用该想法来设计深度学习架构。> > 
>
> > transformer模型完全基于注意力机制，没有任何卷积层或循环神经网络层
> >
> > 更准确地讲，Transformer由且仅由**self-Attenion**和**Feed Forward Neural Network**组成。一个基于Transformer的可训练的神经网络可以通过堆叠Transformer的形式进行搭建

<img src="https://zh-v2.d2l.ai/_images/transformer.svg" alt="../_images/transformer.svg" style="zoom: 80%;" />

## 解决的问题

考虑到 RNN 相关算法只能从左向右依次计算或者从右向左依次计算，这种机制带来了两个问题：

1. 时间片 t 的计算依赖 t−1 时刻的计算结果，这样限制了模型的并行能力；（后者的输入过分依赖前者输出）

2. 顺序计算的过程中信息会丢失，尽管LSTM等门机制的结构一定程度上缓解了长期依赖的问题，但是对于特别长期的依赖现象,LSTM（长短期记忆）依旧无能为力。

   > LSTM（Long-Short Term Memory）：一种特殊的 [RNN](https://easyai.tech/ai-definition/rnn/)，能够学习长期依赖性。公共LSTM单元由单元，输入门，输出门和忘记门组成。该单元记住任意时间间隔内的值，并且三个门控制进出单元的信息流。

   > GRU（Gate Recurrent Unit）：和LSTM一样，也是为了解决长期记忆和反向传播中的梯度等问题而提出来的。功能相当于LSTM，但更易计算，GRU内部少了一个”门控“，参数比LSTM少，考虑到硬件的**计算能力**和**时间成本**，GRU优先

**Transformer**的提出解决了上面两个问题，首先它使用了Attention机制，将序列中的任意两个位置之间的距离是缩小为一个常量（可以为1）；其次它不是类似RNN的顺序结构，因此具有更好的并行性，符合现有的GPU框架。

## 本质

> 一个Encoder-Decoder的结构

<img src="https://pic1.zhimg.com/v2-5a252caa82f87920eadea2a2e93dc528_r.jpg" alt="img" style="zoom: 67%;" />

<img src="https://pic3.zhimg.com/80/v2-c14a98dbcb1a7f6f2d18cf9a1f591be6_1440w.webp" alt="img" style="zoom: 50%;" />

由上图，编码器由6个编码block组成，同样解码器是6个解码block组成。与所有的生成模型相同的是，编码器的输出会作为解码器的输入

在Transformer的encoder中，数据首先会经过一个叫做`self-attention`的模块得到一个加权之后的特征向量 Z ，这个 Z 便是论文公式1中的 Attention(Q,K,V) ：

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20221008140244425.png" alt="image-20221008140244425" style="zoom:80%;" />

得到 Z 之后，它会被送到encoder的下一个模块，即`Feed Forward Neural Network`(前馈神经网络)。这个全连接有两层，第一层的激活函数是ReLU，第二层是一个线性激活函数，可以表示为：

> 前馈神经网络：又叫[多层感知器](https://so.csdn.net/so/search?q=多层感知器&spm=1001.2101.3001.7020)（Multi-Layer Perceptron，MLP），是一种最简单的[神经网络](https://baike.baidu.com/item/神经网络/16600562?fromModule=lemma_inlink)，各神经元分层排列，每个神经元只与前一层的神经元相连。接收前一层的输出，并输出给下一层，各层间没有反馈

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20221008140402702.png" alt="image-20221008140402702" style="zoom:80%;" />

`Self-Attention是Transformer最核心的内容`

self-attention中，每个单词有3个不同的向量，它们分别是**Query向量（ Q ），Key向量（ K ）和Value向量（ V ）**，长度均是64。它们是通过3个不同的权值矩阵由嵌入向量 X 乘以三个不同的权值矩阵 WQ ， WK ， WV 得到，其中三个矩阵的尺寸也是相同的。均是 512×64 。

`Attention的计算方法，整个过程可以分成7步：`

1. 如上文，将输入单词转化成嵌入向量；
2. 根据嵌入向量得到 q ， k ， v 三个向量；
3. 为每个向量计算一个score： score=q⋅k ；
4. 为了梯度的稳定，Transformer使用了score归一化，即除以 dk ；
5. 对score施以softmax激活函数；
6. softmax点乘Value值 v ，得到加权的每个输入向量的评分 v ；
7. 相加之后得到最终的输出结果 z ： z=∑v 。

> 最后一点强调其采用了[残差网络](https://zhuanlan.zhihu.com/p/42706477)中的short-cut结构，目的当然是解决深度学习中的退化问题,层加深可能导致“网络”退化                

## 优缺点

**优点**：

1. 虽然Transformer最终也没有逃脱传统学习的套路，Transformer也只是一个全连接（或者是一维卷积）加Attention的结合体。但是其设计已经足够有创新，因为其抛弃了在NLP中最根本的RNN或者CNN并且取得了非常不错的效果，算法的设计非常精彩，值得每个深度学习的相关人员仔细研究和品位。
2. Transformer的设计最大的带来性能提升的关键是将任意两个单词的距离是1，这对解决NLP中棘手的长期依赖问题是非常有效的。
3. Transformer不仅仅可以应用在NLP的机器翻译领域，甚至可以不局限于NLP领域，是非常有科研潜力的一个方向。
4. 算法的并行性非常好，符合目前的硬件（主要指GPU）环境。

**缺点**：

1. 粗暴的抛弃RNN和CNN虽然非常炫技，但是它也使模型丧失了捕捉局部特征的能力，RNN + CNN + Transformer的结合可能会带来更好的效果
2. Transformer失去的位置信息其实在NLP中非常重要，而论文中在特征向量中加入Position Embedding也只是一个权宜之计，并没有改变Transformer结构上的固有缺陷。

## Seq2Seq（*序列到序列*）

**Encoder-Decoder**结构的网络，它的输入是一个序列，输出也是一个序列。在Encoder中，将序列转换成一个固定长度的向量，然后通过Decoder将该向量转换成我们想要的序列输出出来。

==Encoder==

- 输入序列,可以使用RNN，CNN，Self-attention
- 内部 block 包含若干层, Self-attention+FC在送入block之前，需先进行positional encoding(位置编码)

==Decoder==

- 输出序列，Decoder 会把自己的输出,当做接下来的输入。
  
  - 如果产生错误则会导致Error Propagation，步步错 	–> 	解决：Teacher Forcing
  
  - > teacher-forcing 在训练网络过程中，每次不使用上一个state的输出作为下一个state的输入，而是直接使用训练数据的标准答案(ground truth)的对应上一项作为下一个state的输入。

==Encoder-Decoder之间的信息传递⇒CrossAttention==

过程：

1. Encoder输入一排向量,输出一排向量 $a^1,a^2,a^3$，经过transform產生 Key1 Key2 Key3（ $k^1,k^2,k^3$)，以及 $v^1,v^2,v^3$.（encoder 拿到 K，V）
2. Decoder 会先吃 BEGIN,得到一个向量,输入多少长度的向量,输出就是多少向量。乘上一个矩阵做一个 Transform,得到一个 Query 叫做  $q$（decoder 拿到 Q）
3. 利用q,k计算attention的分数，并做Normalization，得到 $\alpha_1',\alpha_2',\alpha_3',$
4. $\alpha_1',\alpha_2',\alpha_3',$与 $v^1,v^2,v^3$做weighted sum（加权求和），得到  $v$.
5. $v$丢进Fully-Connected 的Network，做接下来的任务。

> 总结：Decoder 就是产生一个q,去 Encoder 抽取信息出来,当做接下来的 Decoder 的Fully-Connected 的 Network 的 Input

==评测标准==

`BLEU`

> BLEU 就是用来衡量[机器翻译](https://so.csdn.net/so/search?q=机器翻译&spm=1001.2101.3001.7020)文本与参考文本之间的相似程度的指标,取值范围在0-1, 取值越靠近1表示机器翻译结果越好。

<img src="C:\Users\11842\AppData\Roaming\Typora\typora-user-images\image-20221009103458199.png" alt="image-20221009103458199" style="zoom:67%;" />

**但是**，在训练时，是对每一个生成的token进行优化，使用的指标是交叉熵。

换言之，训练的时候,是看 Cross Entropy,但是我们实际上你作业真正评估的时候,看的是 BLEU Score。

**并且，不能把BLEU作为LOSS⇒无法微分。**

**解决办法：**遇到你在 Optimization 无法解决的问题,用 RL 硬 Train 一发。遇到你无法 Optimize 的 Loss Function,把它当做是 RL 的 Reward,把你的 Decoder 当做是 Agent。

==Scheduled Sampling==

Scheduled Sampling是指RNN训练时时会**随机使用模型真实label**来作为下一个时刻的输入，而不像原先那样只会使用预测输出，原先是Teacher Forcing的缘故，但会使训练集和测试集效果不一，所以偶尔需要给 decoder一些错误的输入 —> 采用计划采样。

缺点是会损坏平行化能力。

# 06-Generative Model(GAN)

`生成对抗网络`

属于非监督学习，通过两个神经网络相互博弈的方式进行学习

## 应用

Anime Face Generation（动画人脸生成）

Progressive GAN——真实人脸生成⇒生成“没有看过的\连续变化的”人脸

## 结构

`生成器 Generation`

- 对于生成器，输入需要一个n维度向量，输出为图片像素大小的图片。因而首先我们需要得到输入的向量。

  > Tips: 生成器可以是任意可以输出图片的模型，比如最简单的全连接神经网络，又或者是反卷积网络等。

  这里输入的向量我们将其视为携带输出的某些信息，比如说手写数字为数字几，手写的潦草程度等等。由于这里我们对于输出数字的具体信息不做要求，只要求其能够最大程度与真实手写数字相似（能骗过判别器）即可。所以我们使用**随机生成的向量来作为输入**即可，这里面的随机输入最好是满足常见分布比如均值分布，高斯分布等。

  > Tips: 假如我们后面需要获得具体的输出数字等信息的时候，我们可以对输入向量产生的输出进行分析，获取到哪些维度是用于控制数字编号等信息的即可以得到具体的输出。而在训练之前往往不会去规定它。

`判别器 Discriminator`

- 往往是常见的判别器，输入为图片，输出为图片的真伪标签。

  > Tips: 判别器与生成器一样，可以是任意的判别器模型，比如全连接网络，或者是包含卷积的网络等等。

## 训练

基本流程如下：

- 初始化判别器D的参数 θd 和生成器G的参数 θg 。
- 从真实样本中采样 m 个样本 { x1,x2,...xm } ，从先验分布噪声中采样 m 个噪声样本 { z1,z2,...,zm } 并通过生成器获取 m 个生成样本 { x~1,x~2,...,x~m } 。固定生成器G，训练判别器D尽可能好地准确判别真实样本和生成样本，尽可能大地区分正确样本和生成的样本。
- **循环k次更新判别器之后，使用较小的学习率来更新一次生成器的参数**，训练生成器使其尽可能能够减小生成样本与真实样本之间的差距，也相当于尽量使得判别器判别错误。
- 多次更新迭代之后，最终理想情况是使得判别器判别不出样本来自于生成器的输出还是真实的输出。亦即最终样本判别概率均为0.5。

> Tips: 之所以要训练k次判别器，再训练生成器，是因为要先拥有一个好的判别器，使得能够教好地区分出真实样本和生成样本之后，才好更为准确地对生成器进行更新。更直观的理解可以参考下图：

<img src="https://pic2.zhimg.com/v2-11cdb09371a33f4526a8a7f79e0e39f1_r.jpg" alt="img" style="zoom: 50%;" />

> 注：图中的**黑色虚线**表示真实的样本的分布情况，**蓝色虚线**表示判别器判别概率的分布情况，**绿色实线**表示生成样本的分布。 Z 表示噪声， Z 到 x 表示通过生成器之后的分布的映射情况。

我们的目标是使用生成样本分布（绿色实线）去拟合真实的样本分布（黑色虚线），来达到生成以假乱真样本的目的。

可以看到在**（a）**状态处于最初始的状态的时候，生成器生成的分布和真实分布区别较大，并且判别器判别出样本的概率不是很稳定，因此会先训练判别器来更好地分辨样本。
通过多次训练判别器来达到**（b）**样本状态，此时判别样本区分得非常显著和良好。然后再对生成器进行训练。
训练生成器之后达到**（c）**样本状态，此时生成器分布相比之前，逼近了真实样本分布。
经过多次反复训练迭代之后，最终希望能够达到**（d）**状态，生成样本分布拟合于真实样本分布，并且判别器分辨不出样本是生成的还是真实的（判别概率均为0.5）。也就是说我们这个时候就可以生成出非常真实的样本啦，目的达到。

### 训练的难点

- 生成器和判别器需要 match each other（棋逢对手）

这两个 Network,这个 Generator 跟 Discriminator,它们是**互相砥砺,才能互相成长的**,只要其中一者,发生什么问题停止训练,另外一者就会跟著停下训练,就会跟著变差。我们需要保证二者的loss在这一过程中不断下降。

**困难：**Discriminator 跟 Generator,它们互动的过程是自动的。我们不会在中间,每一次 Train Discriminator 的时候都换 Hyperparameter（超参数）。如果有一次loss没有下降，那整个训练过程都有可能出现问题。

`GAN for sequence Generation⇒不能用GD`

由于取了max，这一运算使得Discriminator的score对decoder参数不可微分，也就不能做GD。

不能使用GD⇒使用Reinforcement Learning（强化学习）？→GAN+RL难以训练

**解决：**直接从随机的初始化参数开始Train 它的 Generator,然后让 Generator 可以產生文字,它最关键的就是爆调 Hyperparameter,跟一大堆的 Tips

## 生成器评估

- 直觉的方法：人眼来看⇒不客观、不稳定、代价高
- 自动化方法：影像分类系统

`Quality：对一张图片`

将图片输入影像辨识系统：

- 这个概率的分布如果越集中，说明产生的图片能够被影像分类系统“很肯定地”分辨出来，代表说现在产生的图片可能越好。
- 反之，如果产生的图像是“四不像”，图片分类系统感到困惑，概率分布变得平坦。

`Diversity：对所有（一批）图片`

- Diversity-Mode Collapse(模型崩溃）⇒训练输出的分布局限在很小的范围。
- Diversity-Mode Dropping(模型丢弃）⇒训练输出的分布范围较大，但没有完全覆盖真实数据分布（多样性减小）。

评估多样性：把影像辨识系统对所有生成结果的输出平均起来。

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fb9a35b8b-fe11-4aaa-9ac1-1d7808592205%2FUntitled.png?table=block&id=0f03650e-417e-423a-aca1-76b75a6ea4c7&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom:67%;" />

- 平均分布集中⇒多样性低
- 平均分布“均匀”⇒多样性高

`量化指标`

Inception Score（IS）⇒基于CNN的Inception网络

> 基于paper：***A Note on the Inception Score***

**很多关于 GAN 生成图片的论文中，作者评价其模型表现的一项重要指标是 Inception Score（下文简称 IS）**

主要考虑两方面：

- 清晰度
- 多样性

言归正传：将生成器 生成结果放进Inception网络，通过输出的分布结果来衡量。如果 Quality 高,那个 Diversity 又大,那 Inception Score 就会比较大

`Fréchet Inception Distance (FID)`

Frechet Inception 距离（FID）是评估生成图像质量的度量标准，专门用于评估生成对抗网络的性能。

一些情况下，生成的图像是“同一类别”的，看“分布”并不合适。

同样将图片送入Inception Network，取Softmax 之前的 Hidden Layer 输出的向量，来代表这张图片，利用这个向量来衡量两个分布之间的关系。

⇒假设真实数据和生成数据的两个分布，都是从高斯分布中抽样得到的，计算两个高斯分布之间的Fréchet Distance，越小代表分布越接近，图片品质越高。

**问题：**

- 将任意分布都视为“高斯分布”会有问题
- 计算FID需要大量采样，计算量大。

## 举例

Conditional Generation⇒supervised learning

### Case 1:Text-to-image 

<img src="https://diamond-mule-bee.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F00475494-2ba0-4859-b34b-4e56deb3b17e%2FUntitled.png?table=block&id=d912cb25-2a9c-457a-a6dd-13561a566535&spaceId=effd33e2-527b-43dc-aef5-7b5ee7b83428&width=2000&userId=&cache=v2" alt="img" style="zoom: 67%;" />

**操控 Generator 的输出**,我们给它一个 Condition条件 x，再从一个简单分布中抽样一个$z$,让generator**根据 x 跟 z 来产生 y。**

例如，x 就是一段文字，对希望得到的人脸形象进行描述。

> 如何训练？

魔改DIscriminator**：**

Discriminator 不是只吃图片 y,它还要吃 Condition x。一方面**图片要好**,另外一方面,这个**图片跟文字的叙述必须要是相配**的,Discriminator 才会给高分。

### Case 2：Image Translation（pix2pix）

> 注：利用Supervised Learning训练的问题→输出“模糊”
>
> 解决：单纯用 GAN 的话,它有一个小问题,所以它產生出来的图片,比较真实,但是它的问题是它的创造力,想像力过度丰富,如果你要做到最好,往往就是 GAN 跟 Supervised Learning,同时使用。

### Case 3：声音-图片

### Case 4： 产生会动的人像

# 07-Self-Supervised Learning（BERT）

`自我监督学习`

> BERT(Bidirectional Encoder Representation from Transformers)是2018年10月由Google AI研究院提出的一种预训练模型

BERT是一个**transformer的Encoder**，BERT可以输入一行向量，然后输出另一行向量，输出的长度与输入的长度相同。BERT一般用于**自然语言处理**，一般来说，它的输入是一串文本。当然，也可以输入语音、图像等“序列”。

## Masking Input

随机盖住一些输入的文字，被mask的部分是随机决定的。

`MASK的方法：`

- 第一种方法是，用一个**特殊的符号替换句子中的一个词**，我们用 **"MASK "标**记来表示这个特殊符号，你可以把它看作一个新字，这个字完全是一个新词，它不在你的字典里，这意味着mask了原文。
- 另外一种方法，**随机**把某一个字**换成另一个字**。中文的 "湾"字被放在这里，然后你可以选择另一个中文字来替换它，它可以变成 "一 "字，变成 "天 "字，变成 "大 "字，或者变成 "小 "字，我们只是用随机选择的某个字来替换它

两种方法**都可以使用，使用哪种方法也是随机决定的。**

`训练方法：`

1. 向BERT输入一个句子，先随机决定哪一部分的汉字将被mask。
2. 输入一个序列，我们把BERT的相应输出看作是另一个序列
3. 在输入序列中寻找mask部分的相应输出，将这个向量通过一个Linear transform（矩阵相乘），并做Softmax得到一个分布。
4. 用一个one-hot vector来表示MASK的字符，并使输出和one-hot vector之间的交叉熵损失最小。

> 💡 本质上，就是在解决一个**分类问题**。BERT要做的是**预测什么被盖住。**

`BERT的实际用途`⇒下游任务（Downstream Tasks）

`预训练与微调：`

- 预训练：产生BERT的过程
- 微调：利用一些特别的信息，使BERT能够完成某种任务

BERT只学习了两个“填空”任务。

- 一个是掩盖一些字符，然后要求它填补缺失的字符。
- 预测两个句子是否有顺序关系。

但是，BERT可以被应用在其他的任务【真正想要应用的任务】上，可能与“填空”并无关系甚至完全不同。【胚胎干细胞】当我们想让BERT学习做这些任务时，只**需要一些标记的信息，就能够“激发潜能”**。

## 评价任务集——GLUE

**（General Language Understanding Evaluation）**

