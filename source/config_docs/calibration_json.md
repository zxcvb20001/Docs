# calibration.json 配置说明
================================

calibration.json为标定文件，通过标定工具生成，用于像素坐标系和世界坐标系之间进行转换

- [1. calibration_params](#calibration-params)
- [2. calibration_result](#calibration-result)

-----------------

## 1. calibration_params

| key word | expression | value |
| -------- | ---------- | ----- |
| *bumper_points*                  | 虚拟保险杠端点，单位：米/m               | bumper_points[0]表示left bumper point；bumper_points[1]表示right bumper point |
| *image_plane*                    | 以图片左上角作为原点，图像坐标系下标定点对应的像素坐标，单位：pixel | point |
| *image_plane*                    | 以左前轮为坐标原点，世界坐标系下表定点对应的世界坐标，单位：米/m    | point |
| *image_plane_height_boundaries*  | 图片区域分割线，单位：pixel              | 图片坐标系下y方向下的像素值，目前主要用于在多H矩阵中用来选择H矩阵 |
| *world_plane_height_boundaries*  | 世界坐标系纵向测距范围分割线，单位：米/m | 与image_plane_height_boundaries对应，表示世界坐标系下的纵向区域分割，目前主要用于筛选object |
| *lane_sample_points*             | 车道线H矩阵对应的标定点对，单位：米/m    | 与image_plane对应，在世界坐标系下标定点点对的纵向距离 |
| *num_lane_H*                     | 车道线H矩阵的数量                        | int，默认1 |
| *num_lane_sample_points_per_H*   | 车道线每个H矩阵对应的标定点对的数量，一般为3对点 | int，默认3 |
| *object_sample_points*           | 目标检测H矩阵对应的标定点对,单位：米/m   |  |
| *num_object_H*                   | 目标检测H矩阵的数量                      | int，默认8 |
| *num_object_sample_points_per_H* | 车目标检测每个H矩阵对应的标定点对的数量，一般为3对点 | int，默认3 |

## 2. calibration_result

| key word | expression | value |
| -------- | ---------- | ----- |
| *lane_h_matrix*           | 根据标定参数生成的车道线H矩阵，3×3为1组，矩阵依次按行排列为向量格式   | array[double] |
| *lane_h_matrix_inverse*   | lane_h_matrix 的逆矩阵                                                | array[double] |
| *object_h_matrix*         | 根据标定参数生成的目标检测H矩阵，3×3为1组，矩阵依次按行排列为向量格式 | array[double] |
| *object_h_matrix_inverse* | object_h_matrix的逆矩阵                                               | array[double] |

