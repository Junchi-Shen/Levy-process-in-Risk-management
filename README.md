## Nov 7 2025
更新了对于数据的研究部分 特别是针对2D数据集 和 图片数据集 

以下是关于如何修改数据的情况 

我们已经有的数据的情况是

MNIST: 

```

train_dataset[0]

(tensor([[[-1., -1., -1.,  ..., -1., -1., -1.],
          [-1., -1., -1.,  ..., -1., -1., -1.],
          [-1., -1., -1.,  ..., -1., -1., -1.],
          ...,
          [-1., -1., -1.,  ..., -1., -1., -1.],
          [-1., -1., -1.,  ..., -1., -1., -1.],
          [-1., -1., -1.,  ..., -1., -1., -1.]]]),
 5)

 ```

 其中 tuple的第一个是具体的训练数据 但是我没想好如何处理数据的

可能的可以尝试一下把第二个部分改为一个长252的序列？不过我不确定效果如何？

## Nov 15 2025

大部分的数据逻辑情况是 我们先从get_dataset获取数据，然后再把他变成一个DataLoader 最后传入到TrainingManager

下一步要做的是 搞清楚 进入TrainingManager后的影响 以及发生什么？

## Nov 21 2025

目前搞清楚了数据的传递逻辑是，现在研究进入TrainingManager的后续

在进入了TrainingManager后，读取数据集对应的yml文件，最后决定使用哪个模型以及对应的参数

数据类型一般为：

A. 数据本身：
```
data:
  dataset: mnist 
  channels: 1
  image_size: 32
  random_flip: true
```
B. 方法参数:

```
dlpm:
  alpha: 1.8 
  reverse_steps: 1000
  isotropic: true
  ...
```
C. 模型参数 (model)

```
model:
  model_type: "ddpm"
  attn_resolutions: [2, 4]
  channel_mult: [1, 2, 2, 2]
  ...
```
D. 训练参数 (training)

```
training:
  batch_size: 256
  num_workers: 0
  dlpm:
    ema_rates: [0.99]
    ...
```
E. 优化器参数 (optim)

```
optim:
  optimizer: adamw
  lr: 0.0005
  ...
```
F.运行参数

```
run:
  epochs: 900
  eval_freq: null
  checkpoint_freq: 300
```

