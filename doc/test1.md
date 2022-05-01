一、指令选型背景
1. 采用scratchpad memory进行数据存储
a. 对于cnn网络，卷积运算占大部分算子的运算时间，约为80%。
b. 卷积运算的核心在于矩阵的频繁从内存加载，故采用scratchpad memory在片上集成sram对于源数据（eg.卷积核）数据进行暂存，避免大量的访存操作。
2. 采用int8作为单位元素大小
a. 现有的深度学习框架 比如：TensorFlow，pytorch，Caffe， MixNet等，在训练一个深度神经网络时，往往都会使用 float 32（Full Precise ，简称FP32）的数据精度来表示，权值、偏置、激活值等。但是如果一个网络很深的话，比如像VGG，ResNet这种，网络参数是极其多的，计算量就更多了（比如VGG 19.6 billion FLOPS, ResNet-152 11.3 billion FLOPS）。实际上有些人认为，即便在推理时使用低精度的数据（比如INT8），在提升速度的同时，也并不会造成太大的精度损失。
b. 在现有的许多移动端推理网络如ncnn等都是支持int8量化的。
3. 采用int8作为scratchpad的单元大小，scratchpad大小初定为128*8bit=1Kib
a. 对于ncnn等网络常用的1*1，3*3，5*5，7*7卷积核大小，采用1Kib的scratchpad大小能够较好的暂存featuremap和kernel数据。
b. scratchpad从0开始索引编址，支持0-127个索引的数据读写，每个元素大小为一个字节。

