# evaluation config.json 配置说明
================================

evaluation目前尚未与[calibration.json](./calibration_json.md)和[sdk_config.json](./sdk_config_json.md)实现统一，因此配置文件包括`config_fpga.json`和`config_x86.json`，二者的区别在于模型配置，具体可以参考[sdk_config.json](./sdk_config_json.md)，以下不再说明

evaluation的配置文件为evaluation提供模型评测相关的badcase筛选配置和产品指标配置


- [1. lane_evaluation_property](#lane-evaluation-property)
- [2. object_evaluation_property](#object-evaluation-property)
- [3. light_evaluation_property](#light-evaluation-property)
- [4. sign_evaluation_property](#sign-evaluation-property)
- [5. freespace_evaluation_property](#freespace-evaluation-property)
- [6. lane_category_evaluation_property](#lane-category-evaluation-property)



-----------------

## 1. lane_evaluation_property
车道线评测配置

| key word | expression | value |
| -------- | ---------- | ----- |
| is_enabled                | 开关项，是否开启车道线evaluation | true/是，false/否         |
| detector_property         | detector 配置                    |                           |
| product_evaluate_property | 产品指标评测                     | null                      |
| model_path                | 模型路径                         | null                      |
| bad_case_iou_threshold    | badcase筛选阈值                  | float，取值范围[0.0, 1.0] |
| test_dataset_info         | 用于evaluation的评测集配置       | null                      |

**product_evaluate_property 说明**

- is_enabled：开关项，是否启用产品指标evaluation，true/是，false/否， **产品指标评测和模型评测在1次运行中只能开启一个，如果开启了产品指标评测，则默认关闭模型评测**

- top_images_num_list：array[int]，从评测集中筛选出基于产品指标的得分最高的x张图片作为产品指标的验证集

- is_save_result：开关项，是否保存detection result图片，true/是，false/否

- is_generate_dataset_json：开关项，是否生成基于top_images_num_list中指定大小的json数据集，true/是，false/否， **生成的json数据集可直接替换test_dataset_info中的数据集进行评测**

- save_dir：detection result图片保存目录路径

**test_dataset_info 说明**

- saved_path：detection result的保存路径，若不为空，则会将每张图片的detection result保存到指定目录下， **若为空，则默认关闭**

- bad_case_saved_path：筛选出的badcase的保存路径，若不为空，则将badcase图片保存到该路径的目录下， **若为空，则默认关闭**

- dataset_root_path：若 _dataset_json_path_ 指定的json文件中的数据路径为相对路径，则该项应该设为相对路径的根目录路径，否则设为空

- dataset_json_path：json评测集文件路径

## 2. object_evaluation_property
目标检测评测配置，重复配置请参考[lane_evaluation_property](#evaluation-lane)

| key word | expression | value |
| -------- | ---------- | ----- |
| draw_score            | bbox的绘制阈值 | float，取值范围[0.0, 1.0] |
| min_overlap           | 最小overlap，用于判断bbox是否相对与gt为TP或者FP | float，取值范围[0.0, 1.0] |
| min_score             | 最小score，大于该值的bbox才认为是有效的bbox | float，取值范围[0.0, 1.0] |
| max_box_num           | 最大bbox数量阈值，大于该值的gt直接丢弃 | int |
| min_height_pedestrian | 若检测的bbox类别为行人，那么bbox的height若小于该值，标明目标距离太远，不应作为产品的评测gt | int |
| min_width_pedestrian  | 若检测的bbox类别为行人，那么bbox的width若小于该值，标明目标距离太远，不应作为产品的评测gt | int |
| min_height_vehicle    | 若检测的bbox类别为机动车，那么bbox的height若小于该值，标明目标距离太远，不应作为产品的评测gt | int |
| min_width_vehicle     | 若检测的bbox类别为机动车，那么bbox的width若小于该值，标明目标距离太远，不应作为产品的评测gt | int |
| min_height_cyclist    | 若检测的bbox类别为非机动车，那么bbox的height若小于该值，标明目标距离太远，不应作为产品的评测gt | int |
| min_width_cyclist     | 若检测的bbox类别为非机动车，那么bbox的width若小于该值，标明目标距离太远，不应作为产品的评测gt | int |

## 3. light_evaluation_property
目标检测交通信号灯评测配置，请参考[object_evaluation_property](#object-evaluation-property)

## 4. sign_evaluation_property
目标检测交通标识评测配置，请参考[object_evaluation_property](#object-evaluation-property)

## 5. freespace_evaluation_property
freespace 评测配置，请参考[lane_evaluation_property](#lane-evaluation-property)

## 6. lane_category_evaluation_property
车道线分类评测配置，请参考[lane_evaluation_property](#lane-evaluation-property)


