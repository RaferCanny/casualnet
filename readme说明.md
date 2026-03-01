## Readme
![输入图片说明](/imgs/2026-03-01/gOoA9DPCGvTR2MwD.png)
这是casual net的文件结构。使用layer1 = 3，layer2 = 2 ，layer3 = 8 ，gamma = 0.4 这一组超参数验证。Datasets是数据集.
![输入图片说明](/imgs/2026-03-01/oTQMZU4LQI2ZRtVP.png)

输入格式是28*28*3的光流图

 - STSNet_whole_norm_u_v_os存储整脸光流原始图像，用于裁剪面部关键区域（眼、唇、鼻）的光流子块
 
- three_norm_u_v_os存储按受试者，训练或者测试集，情感类别划分的光流数据索引，用于构建训练或者测试集
每个subject训练是完全独立的，互不影响（日志中的警告不影响正常训练）
- calculate_all_results_CASMEII.py：适配 CASMEII 数据集（受试者列表为 sub01~sub26 等）；

- calculate_all_results_SMIC.py：适配 SMIC 数据集（受试者列表为 s01~s20 等）；

- calculate_all_results_SAMM.py：适配 SAMM 数据集（受试者列表为 006~037 等）；

- calculate_all_results.py：适配 “CASMEII+SMIC+SAMM” 合并数据集（包含所有受试者 ID）。
 
- Models.py：定义CausalNet核心模型，包含时空因果注意力、交叉注意力等组件，提取光流的时空特征，实现表情分类。

- main_train.py：基础训练脚本，加载光流数据集、裁剪人脸关键区域特征，固定超参训练CausalNet，输出 F1/UAR 等指标并保存模型权重。

- main_train_for_parameter_tuning.py：调参专用训练脚本，遍历网络层数、gamma 等超参，限定训练受试者列表，优化模型参数。

- eval.py：模型评估脚本，加载训练好的权重，对指定受试者的测试集做推理，计算准确率并保存评估结果（准确率、混淆矩阵）。
 
### datasets结构
datasets结构
└── STISet_whole_norm_u_v_os（光流图，未裁剪）
    └── three_norm_u_v_os
        ├── 006
        │   ├── u_test
        │   │   └── 0 -- 少量图片
        │   └── u_train
        │       ├── 0 -- 少量图片
        │       └── 1 -- 大量图片
        ├── 037
        ├── s01
        ├── s20
        ├── sub01
         ├── sub26
         ├——……等等
        

> 注：u_test 有的只有一个类别
