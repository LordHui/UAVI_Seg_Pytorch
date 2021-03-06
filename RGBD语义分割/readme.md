## RGBD语义分割实验总结

### 实验数据
- 数据：7830张320 x 320尺寸的image、dsm_data、label；
- **数据处理部分**
  - Depth原始数据（高程数据）是32位的tiff格式图像，需转换为8位的tiff格式图，**使用python的代码直接转换会线性压缩图像的对比性，所以要先找到图像像素的极值，将其像素之间的差做出来，再使用代码转换为uint8文件**。转换代码地址：[32to8.py](https://github.com/yearing1017/Deeplabv3_Pytorch/blob/master/utils/32to8.py)
  - png切割代码：[png_crop.py](https://github.com/yearing1017/Deeplabv3_Pytorch/blob/master/utils/png_crop.py)
  - 在dataset3中有筛选出的特征较均衡的图像，使用代码筛选切割好的dsm高程数据；代码：[file_find.py](https://github.com/yearing1017/Deeplabv3_Pytorch/blob/master/utils/file_find.py)
  - 基于dataset3加入dsm高程数据：[dataset4的调整记录](https://github.com/yearing1017/Deeplabv3_Pytorch/issues/2#issuecomment-696058635)

### 1. [ACNet_v0923-v0925版本](https://github.com/yearing1017/Deeplabv3_Pytorch/tree/master/RGBD%E8%AF%AD%E4%B9%89%E5%88%86%E5%89%B2/ACNet_v0923)

#### 1.1 实验简介
- 网络：ACNet原始代码，未改动；
- ACNet_v0923baseline：resnet50的layer1-4；
- ACNet_v0925baseline：resnet101的layer1-4；
- 训练与验证：5折交叉验证；
- 训练轮次数：epoch = 40；
- 模型的评价标准：MIoU；

#### 1.2 实验结果
- 较[ccnet_v3_0607](https://github.com/yearing1017/CCNet_PyTorch)版本各项指标均有提升；
- 具体acc及miou指标如下：

|     版本&指标    |  Acc   |  MIoU  | Kappa  |  背景  |  房屋  |  道路  |  车辆  |
| :-----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| CCNet0607_epo35 | 0.9603 | 0.8057 | 0.8684 | 0.9722 | 0.8907 | 0.9216 | 0.7745 |
| ACNet0923_epo36 | 0.9635 | 0.8220 | 0.8802 | 0.9710 | 0.9191 | 0.9588 | 0.8167 |
| ACNet0925_epo38 | 0.9583 | 0.8016 | 0.8625 | 0.9716 | 0.9060 | 0.9002 | 0.7994 |

### 2. [ACNet_v0927版本](https://github.com/yearing1017/Deeplabv3_Pytorch/tree/master/RGBD%E8%AF%AD%E4%B9%89%E5%88%86%E5%89%B2/ACNet_v0927)

#### 2.1 实验简介
- 网络：ACNet的decode阶段普通的卷积层换为CC_Module；
- baseline：resnet50的layer1-4；
- 训练与验证：5折交叉验证；
- 训练轮次数：epoch = 45；
- 模型的评价标准：MIoU；

#### 2.2 实验结果

|     版本&指标    |  Acc   |  MIoU  | Kappa  |  背景  |  房屋  |  道路  |  车辆  |
| :-----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| ACNet0927_epo44 | 0.9573 | 0.7935 | 0.8618 | 0.9677 | 0.9415 | 0.9521 | 0.8032 |


### 3. [ACNet_v0928版本](https://github.com/yearing1017/Deeplabv3_Pytorch/tree/master/RGBD%E8%AF%AD%E4%B9%89%E5%88%86%E5%89%B2/ACNet_v0928)

#### 3.1 实验简介
- 网络：ACNet的通道注意力换为CC_Module；
- baseline：resnet50的layer1-4；
- 训练与验证：5折交叉验证；
- 训练轮次数：epoch = 45；
- 模型的评价标准：MIoU；

#### 3.2 实验结果

|     版本&指标    |  Acc   |  MIoU  | Kappa  |  背景  |  房屋  |  道路  |  车辆  |
| :-----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| ACNet0928_epo44 | 0.9352 | 0.7172 | 0.7878 | 0.9491 | 0.8509 | 0.9534 | 0.6594 |

### 4. [ACNet_v0930版本](https://github.com/yearing1017/Deeplabv3_Pytorch/tree/master/RGBD%E8%AF%AD%E4%B9%89%E5%88%86%E5%89%B2/ACNet_CBAM)

#### 4.1 实验简介
- 网络：ACNet的通道注意力换为CBAM模块；
- baseline：resnet50的layer1-4；
- 训练与验证：5折交叉验证；
- 训练轮次数：epoch = 45；
- 模型的评价标准：MIoU；

#### 4.2 实验结果

|     版本&指标    |  Acc   |  MIoU  | Kappa  |  背景  |  房屋  |  道路  |  车辆  |
| :-----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| ACNet0930_epo42 | 0.9616 | 0.8192 | 0.8733 | 0.9744 | 0.9113 | 0.9472 | 0.8420 |

### 5. ACNet_v1002版本

#### 5.1 实验简介
- 网络：ACNet0923对照FocalLoss和CrossEntropyLoss的效果；
- baseline：resnet50的layer1-4；
- 训练与验证：5折交叉验证；
- 训练轮次数：epoch = 40；
- 模型的评价标准：MIoU；

#### 5.2 实验结果

|     版本&指标    |  Acc   |  MIoU  | Kappa  |  背景  |  房屋  |  道路  |  车辆  |
| :-----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| ACNet1002_epo38 | 0.9492 | 0.7681 | 0.8355 | 0.9634 | 0.8688 | 0.9394 | 0.7913 |

### 6. [ACNet_v1004版本](https://github.com/yearing1017/Deeplabv3_Pytorch/tree/master/RGBD%E8%AF%AD%E4%B9%89%E5%88%86%E5%89%B2/ACNet_1004)

#### 6.1 实验简介
- 网络：ACNet_del_M_model；删除了中间的Merge分支；
- baseline：resnet50的layer1-4；
- 训练与验证：5折交叉验证；
- 训练轮次数：epoch = 40；
- 模型的评价标准：MIoU；

#### 6.2 实验结果

|     版本&指标    |  Acc   |  MIoU  | Kappa  |  背景  |  房屋  |  道路  |  车辆  |
| :-----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| **ACNet1004** | **0.9638** | **0.8204** | **0.8807** | **0.9713** | **0.9229** | **0.9643** | **0.8317** |

### 7. [ACNet_v1005版本](https://github.com/yearing1017/Deeplabv3_Pytorch/tree/master/RGBD%E8%AF%AD%E4%B9%89%E5%88%86%E5%89%B2/ACNet_1005)

#### 6.1 实验简介
- 网络：ACNet_del_M_model；删除了中间的Merge分支；
- 变量：ACNet1004对照试验：使用`torch.cat`替换融合的`+`;
- baseline：resnet50的layer1-4；
- 训练与验证：5折交叉验证；
- 训练轮次数：epoch = 40；
- 模型的评价标准：MIoU；

#### 6.2 实验结果

|     版本&指标    |  Acc   |  MIoU  | Kappa  |  背景  |  房屋  |  道路  |  车辆  |
| :-----: | :----: | :----: | :----: | :----: | :----: | :----: | :----: |
| ACNet1005 | 0.9619 | 0.8065 | 0.8740 | 0.9715 | **0.9288** | 0.9425 | 0.7376 |

