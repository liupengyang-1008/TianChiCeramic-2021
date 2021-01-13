# TianChiCeramic-2021

## 训练数据文件结构

```
└── dataset
    ├── Readme.md
    ├── train_annos.json
    └── train_imgs
```
train_imgs：训练图片数据，jpg格式
train_annos.json：训练标注数据，json格式
Readme.md：说明文件

## 标注说明

训练标注是train_annos.json，内容如下

```json
[
    {
        "name": "226_46_t20201125133518273_CAM1.jpg",
        "image_height": 6000,
        "image_width": 8192,
        "category": 4,
        "bbox": [
            1587,
            4900,
            1594,
            4909
        ]
    }
]
```

具体说明如下：
- name是图片名
    - "226_46"代表砖的唯一id，
    - "CAM1"代表相机1拍照所得，一般来说每块砖会有三张样本，分别是CAM1，CAM2，CAM3。 
- image_height和image_width是图片高宽，
- category"是类别id，
- bbox是目标框信息xyrb格式，分别指[左上角x坐标，左上角y坐标，右下角x坐标，右下角y坐标]

## 类别说明

```json
{
  "0": "背景",
  "1": "边异常",
  "2": "角异常",
  "3": "白色点瑕疵",
  "4": "浅色块瑕疵",
  "5": "深色点块瑕疵",
  "6": "光圈瑕疵"
 }
```

## 评测结果提交

参赛者需要提供一份json文件包含所有预测结果，文件内容如下:
```json
[
    {
        "name": "226_46_t20201125133518273_CAM1.jpg",
        "category": 4,
        "bbox": [
            5662,
            2489,
            5671,
            2497
        ],
        "score": 0.130577
    },
    {
        "name": "226_46_t20201125133518273_CAM1.jpg",
        "category": 2,
        "bbox": [
            6643,
            5416,
            6713,
            5444
        ],
        "score": 0.120612
    },
    {
        "name": "230_118_t20201126144204721_CAM2.jpg",
        "category": 5,
        "bbox": [
            3543,
            3875,
            3554,
            3889
        ],
        "score": 0.160216
    }
]
```

## 评估指标

赛题分数计算方式: 0.2ACC+0.8mAP

ACC：是有瑕疵或无瑕疵的分类指标，考察瑕疵检出能力。
其中提交结果name字段中出现过的测试图片均认为有瑕疵，未出现的测试图片认为是无瑕疵。

mAP：参照PASCALVOC的评估标准计算瑕疵的mAP值。
参考链接：https://github.com/rafaelpadilla/Object-Detection-Metrics 
具体逻辑见evaluator文件

需要指出，本次大赛评分计算过程中，分别在检测框和真实框的交并比(IoU)在阈值0.1，0.3，0.5下计算mAP，最终mAP取三个值的平均值。