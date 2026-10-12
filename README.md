# 计算机视觉应用

面向人工智能通识课程与计算机视觉应用教学，以肺部病灶随访为例，理解如何把真实问题转化为检测、分割、配准与目标追踪任务。

## 课堂演示

| 资源 | 用途 | 打开方式 |
| --- | --- | --- |
| [病灶追踪交互演示](lesion-tracking.html) | 观察位置差异、配准后的候选以及病灶外观，并揭晓教学示例中的对应关系 | 下载HTML后用浏览器打开，可离线操作；GitHub文件预览不会运行HTML |
| [配准演示：原速前10秒](registration-10s.mp4) | 引出呼吸、体位等造成的位置差异，以及建立位置对应的必要性 | 下载后播放，或插入PPT |
| [预计位置与揭晓：15秒](tracking-reveal-15s.mp4) | 展示基线目标和候选，随后揭晓示例对应关系 | 下载后播放，或插入PPT |

课堂主线：**发现病灶 → 勾画区域 → 建立位置对应 → 确认同一病灶 → 比较真实变化**。

## 问题与方法

| 真实问题 | 对应任务 | 课堂讲法 |
| --- | --- | --- |
| 小病灶容易被遗漏 | 目标检测、多尺度特征 | 先找到病灶在哪里 |
| 需要描述大小与边界 | 图像分割 | 再确定哪些像素或体素属于病灶 |
| 两次检查的解剖位置不对应 | 影像配准 | 呼吸和体位改变，让同一位置的坐标发生偏移 |
| 同一病灶外观会变，不同病灶也可能相似 | 目标追踪 | 在位置线索帮助下，确认哪个是原来的病灶 |

## 前沿论文与知识讲解

按教学脉络排列，兼顾经典方法基础与近期研究。**DLT和LesionLocator是本仓库的重点阅读论文。**

| 论文 | 年份 / 来源 | 解决的问题 | 核心思路 | 输出 / 可带走的知识 | 原文与资源 |
| --- | --- | --- | --- | --- | --- |
| Feature Pyramid Networks for Object Detection（FPN） | 2017 / CVPR，经典基础 | 不同大小目标的检测 | 自顶向下与横向连接，融合不同分辨率的特征 | 多尺度特征支持检测；理解细节与语义信息的互补 | [原文](https://openaccess.thecvf.com/content_cvpr_2017/html/Lin_Feature_Pyramid_Networks_CVPR_2017_paper.html) |
| Segment anything in medical images（MedSAM） | 2024 / Nature Communications | 专用分割模型跨任务泛化有限 | 将SAM适配至医学影像，根据框等空间提示勾画目标 | 分割掩膜；理解提示可以指定“分割谁” | [原文](https://www.nature.com/articles/s41467-024-44824-z) · [作者仓库](https://github.com/bowang-lab/MedSAM) |
| **Deep Lesion Tracker: Monitoring Lesions in 4D Longitudinal Imaging Studies（DLT）** | **2021 / CVPR** | 两次CT中病灶位置和外观变化，以及相似区域干扰 | 仿射配准提供位置先验；解剖信号与孪生3D特征融合；互相关形成匹配响应 | **对应病灶中心**；位置线索与外观匹配共同帮助确认身份 | [原文](https://openaccess.thecvf.com/content/CVPR2021/html/Cai_Deep_Lesion_Tracker_Monitoring_Lesions_in_4D_Longitudinal_Imaging_Studies_CVPR_2021_paper.html) · [作者仓库 / DLS基准](https://github.com/JimmyCai91/DLT) |
| **LesionLocator: Zero-Shot Universal Tumor Segmentation and Tracking in 3D Whole-Body Imaging** | **2025 / CVPR** | 在多次随访中持续追踪并更新病灶区域 | 非刚性配准传播上一次的病灶掩膜，以其为空间提示在当前CT中重新分割，再传递新掩膜 | **每次检查的病灶掩膜**；理解历史结果如何帮助当前预测 | [CVPR原文](https://openaccess.thecvf.com/content/CVPR2025/html/Rokuss_LesionLocator_Zero-Shot_Universal_Tumor_Segmentation_and_Tracking_in_3D_Whole-Body_CVPR_2025_paper.html) · [预印本](https://arxiv.org/abs/2502.20985) · [作者代码](https://github.com/MIC-DKFZ/LesionLocator) |

更多方法解释与课堂提问见 [前沿知识讲解](frontier-guide.md)。

## 学习顺序

1. 打开交互演示，观察两次检查中位置如何不同。
2. 对齐参照点，再判断预计位置附近哪个候选可能对应基线目标。
3. 先说明判断依据，再揭晓示例答案。
4. 阅读DLT图2：追踪如何使用位置和外观特征？
5. 阅读LesionLocator图2：上一次的掩膜如何帮助下一次分割？

## 演示与论文的关系

HTML与视频是课堂示意，候选对应关系预先设定，并未运行真实CT配准或DLT推理。图中的血管帮助理解周围解剖结构；不表示DLT显式抽取血管分叉并进行规则匹配。

论文中的模型、代码、数据和本仓库的教学演示分别管理。DLT作者仓库主要公开DLS标注、数据转换与评价资源，不能据此假定提供了完整追踪网络和权重。第三方论文、代码和数据以各自来源及许可为准。


## 材料领域作业：钢材表面缺陷智能质检

**应用场景：**高速产线上，相机连续拍摄钢材表面。请根据真实缺陷图像，设计一套智能质检方案，说明如何找到缺陷、识别类型，以及支持后续生产决策。

### 真实数据集：NEU钢材表面缺陷数据库

由东北大学发布，包含热轧带钢的六类典型表面缺陷，共1,800张200×200像素灰度图，每类300张。

| 类别 | 英文标记 |
| --- | --- |
| 裂纹 | crazing |
| 夹杂 | inclusion |
| 斑块 | patches |
| 麻点 | pitted surface |
| 氧化铁皮压入 | rolled-in scale |
| 划痕 | scratches |

- [官方介绍、样例图与下载入口](https://faculty.neu.edu.cn/songkc/en/zdylm/263265/)：选择**NEU-DET**，它同时提供图像、缺陷类别与位置标注；NEU-CLS适合图像分类。
- [NEU-DET百度网盘（官方页面链接）](https://pan.baidu.com/s/1nBbO-jWDm1_NHDQsc1dRkg)，提取码：**pmqx**。
- [NEU-DET Google Drive（官方页面链接）](https://drive.google.com/file/d/1qrdZlaDi272eA79b0uCwwqPrm2Q_WI3k/view)：下载可能需要登录。
- [材料图像分析参考GitHub：NEU-Cluster](https://github.com/rccohn/NEU-Cluster)：提供迁移学习与聚类示例，属于第三方分类项目，可供拓展阅读。

### 小组任务

先从数据集中选取**三类缺陷，每类两张，共六张图**，记录文件名，观察并标出缺陷位置，再完成下表。基础作业不要求训练模型。

| 环节 | 需要完成的内容 | 成果要求 |
| --- | --- | --- |
| **任务定义：拆清需求** | “判断是否有缺陷”“确定缺陷位置”“识别缺陷类型”“测量缺陷范围”分别对应什么视觉任务？本组方案需要哪些输出？ | 画出输入→任务→输出流程图，并用所选图像举例 |
| **任务适配：应对难点** | 从六张图中找出两种实际困难，例如细小目标、低对比度、形状变化或背景纹理干扰；每种困难提出一项数据或模型调整，并解释理由 | 两组“图像证据→困难→调整方法”；小目标问题可联系课堂多尺度特征 |
| **任务演化：走向产线** | 从单张图像识别扩展到连续产线，如何避免重复计数，并把同一缺陷关联起来？还需要补充哪些数据和标注？ | 补充连续追踪流程，说明至少两项新增数据需求 |

**数据边界：**NEU-DET是静态缺陷图像集，不能直接验证连续追踪；追踪需要视频或连续帧、时间顺序以及跨帧缺陷身份标注。该数据集中的图像均属于缺陷类别；若要训练“合格/不合格”判断，还需补充无缺陷样本，并根据质量标准确定判定规则。强反光和高速运动模糊可作为产线扩展条件讨论，不必声称它们在所选样本中存在。

### 提交与评价

在课程信息平台提交：**1份方案图＋300—500字设计说明＋6张带文件名的样例图**。设计说明包括任务选择、两项针对性调整、连续追踪的数据需求。

教师评价＋学生自评＋学生互评，共同检查：需求是否拆清、方法是否针对图像中的困难、追踪方案是否说明数据条件。
