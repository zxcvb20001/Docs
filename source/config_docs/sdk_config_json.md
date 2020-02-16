# sdk_config.json 配置说明
================================

sdk_config.json为ADAS SDK提供标定文件路劲配置、模型相关配置、功能开关配置、后处理参数配置以及告警相关参数配置。

- [1. adas_detector_property](#adas-detector-property)
    - [1.1 FPGA_predictor](#fpga-predictor)
    - [1.2 GPU_predictor](#gpu-predictor)
    - [1.3 bgr_means](#bgr-means)
    - [1.4 input_limit](#input-limit)
    - [1.5 function_option](#function-option)
    - [1.6 post_processor_property](#post-processor-property)

- [2. alarm_property](#alarm-property)
    - [2.1 lane_alarm_property](#lane-alarm-property)
    - [2.2 object_alarm_property](#object-alarm-property)

- [3. others](#others)

-----------------

## 1. adas_detector_property
adas_detector_property配置对应平台（FPGA/GPU）下的深度学习模型对应的*文件路径*、*input_blob名称*、*output_blob名称以及设备号*。

### 1.1 FPGA_predictor

FPGA平台对应的模型配置

**1.1.1 combined_fpga_predictor_property**

定点融合模型predictor配置，该模型执行在FPGA上，通过FPGA加速inference过程。

| key word | expression |
| -------- | ---------- |
| *net_file*          | 融合模型的网络配置文件路径，对应prototxt |
| *model_file*        | 融合模型的权值文件路径，对应caffemodel   |
| *input_blob_names*  | 模型input_blob名称列表                   |
| *output_blob_names* | 模型output_blob名称列表                  |
| *gpu_device*        | 执行该模型inference的设备号              |

**1.1.2 fcw_fpga_additional_predictor_property**

取出定点融合模型FCW对应的output_blobs结果作为输入，即“combined_fpga_predictor_property”中的output_blob_names里的FCW相关层，该部分执行在arm-CPU上，输出机动车、非机动车和行人检测结果。

| key word | expression |
| -------- | ---------- |
| *net_file*          | FCW模型的网络配置文件路径，对应prototxt |
| *model_file*        | FCW模型的权值文件路径，对应caffemodel   |
| *input_blob_names*  | 模型input_blob名称列表                  |
| *output_blob_names* | 模型output_blob名称列表                 |
| *gpu_device*        | 执行该模型inference的设备号             |

**1.1.3 tlr_fpga_additional_predictor_property**

取出定点融合模型TLR对应的output_blobs结果作为输入，即“combined_fpga_predictor_property”中的output_blob_names里的FCW相关层，该部分执行在arm-CPU上，输出TrafficLight交通信号灯检测结果。

| key word | expression |
| -------- | ---------- |
| *net_file*          | TLR模型的网络配置文件路径，对应prototxt |
| *model_file*        | TLR模型的权值文件路径，对应caffemodel   |
| *input_blob_names*  | 模型input_blob名称列表                  |
| *output_blob_names* | 模型output_blob名称列表                 |
| *gpu_device*        | 执行该模型inference的设备号             |

**1.1.4 tsr_fpga_additional_predictor_property**

取出定点融合模型TSR对应的output_blobs结果作为输入，即“combined_fpga_predictor_property”中的output_blob_names里的FCW相关层，该部分执行在arm-CPU上，输出TrafficSign交通标识牌和交通标识标志检测结果。

| key word | expression |
| -------- | ---------- |
| *net_file*          | TSR模型的网络配置文件路径，对应prototxt |
| *model_file*        | TSR模型的权值文件路径，对应caffemodel   |
| *input_blob_names*  | 模型input_blob名称列表                  |
| *output_blob_names* | 模型output_blob名称列表                 |
| *gpu_device*        | 执行该模型inference的设备号             |

### 1.2 GPU_predictor

GPU平台对应的模型相关配置，目前浮点模型仅支持到fuse_v4.5版本

### 1.3 bgr_means

待补充

### 1.4 input_limit

模型输出限制，将图片resize到该尺寸后再进行inference

### 1.5 function_option

模型功能开关选项

| key word | expression | value |
| -------- | ---------- | ----- |
| *is_lka_detection_enabled*           | 模型车道线检测开关    | true/开，false/关 |
| *is_freespace_detection_enabled*     | 模型freespace检测开关 | true/开，false/关 |
| *is_lane_category_detection_enabled* | 模型车道线分类开关    | true/开，false/关 |
| *is_fcw_detection_enabled*           | 模型FCW目标检测开关   | true/开，false/关 |
| *is_tlr_detection_enabled*           | 模型TLR目标检测开关   | true/开，false/关 |
| *is_tsr_detection_enabled*           | 模型TSR目标检测开关   | true/开，false/关 |

### 1.6 post_processor_property
ADAS SDK后处理相关配置项

**1.6.1 lane_post_process_property**
车道线后处理配置

- detector_type：detector类型，可选项有`LANES_AND_STOPLINE_DETECTOR`和`GUIDELINES_DETECTOR`

- lanes_selector_type：selector类型，可选项有`GIANT_COMPONENT_SELECT`和`RANSAC_SELECT`

- lanes_fitter_type：车道线拟合方式
> 
| key word | expression |
| -------- | ---------- |
| *LINE_FIT*            | 线性拟合     |
| *QUADRATIC_CURVE_FIT* | 二阶曲线拟合 |
| *CUBIC_CURVE_FIT*     | 三阶曲线拟合 |
| *ITERATIVE_QUADRATIC_CURVE_FIT*  | 待补充 |
| *HERMITE_CUBIC_SPLINE_CURVE_FIT* | 待补充 |

- horizon_ratio

- wheel_base

- half_road_width

- front_wheel_center_x

- front_wheel_center_y

- lanes_prob_threshold：四条车道线概率阈值

- lanes_sum_threshold

- lanes_adaptive_CCA_thresholds

- is_apply_interframe_filter_enabled：是否使用帧间过滤，true/false

- is_apply_parallel_constraint_enabled：是否使用平行约束，true/false

- is_skip_frame_enabled：是否跳帧处理，true/false

- skip_frame_num：跳帧数目

- skip_quadratic_lane_A_thresh

- max_y_distance：y方向最远检测距离

- lane_sample_interval

- crop_width：如果模型采用crop裁剪，crop_width表示与模型对应的水平方向裁剪大小，否则应为0

- crop_height：如果模型采用crop裁剪，crop_height表示与模型对应的垂直方向裁剪大小，否则应为0

**1.6.2 object_post_process_property**
目标检测后处理配置

- is_object_use_freespace_fusion：是否使用freespace结果进行过滤，true/false

- vehicle_bbox_score_threshold：机动车bbox检测阈值，默认0.9，取值[0.0, 1.0]

- cyclist_bbox_score_threshold：非机动车bbox检测阈值，默认0.9，取值[0.0, 1.0]

- pedestrian_bbox_score_threshold：行人bbox检测阈值，默认0.9，取值[0.0, 1.0]

- traffic_light_bbox_score_threshold：交通信号灯bbox检测阈值，默认0.9，取值[0.0, 1.0]

- traffic_sign_bbox_score_threshold：交通标识bbox检测阈值，，默认0.9，取值[0.0, 1.0]

- nms_property：nms相关配置

- object_tracker_property：目标跟踪相关配置
    - block_num
    - block_num_distance
    - velocity_type

- crop_width：如果模型采用crop裁剪，crop_width表示与模型对应的水平方向裁剪大小，否则应为0

- crop_height：如果模型采用crop裁剪，crop_height表示与模型对应的垂直方向裁剪大小，否则应为0

**1.6.3 freespace_post_process_property**
freespace检测后处理配置

- world_coordinate_sample_interval：世界坐标系下采样差值系数

- prob_map_threshold：概率图检测阈值

- left_max_distance：左边最大距离

- right_max_distance：右边最大距离

- farthest_distance：最远检测距离

- crop_width：如果模型采用crop裁剪，crop_width表示与模型对应的水平方向裁剪大小，否则应为0

- crop_height：如果模型采用crop裁剪，crop_height表示与模型对应的垂直方向裁剪大小，否则应为0

## 2. alarm_property
alarm_property配置产品告警相关参数，包括LDW、FCW、PCW、HMW、UFCW对应的开关及约束参数

### 2.1 lane_alarm_property
LDW告警相关配置

| key word | expression | value |
| -------- | ---------- | ----- |
| *is_lane_alarm_enabled*                | 是否使用LDW告警功能                | true/开，false/关 |
| *is_alarm_once_every_depature_enabled* | 是否每次压线仅发1次告警            | true/开，false/关 |
| *min_distance_thres*                   |  | float |
| *farthest_warning_distance*            |  | float |
| *shortest_warning_lane_length*         |  | float |
| *alarm_interval*                       |  | float |
| *alarm_depature_speed*                 |  | float |
| *alarm_speed*                          | 告警车速限制，低于该车速不触发告警 | float |

### 2.2 object_alarm_property
目标检测相关功能告警配置，包括FCW、PCW、HMW和UFCW

| key word | expression | value |
| -------- | ---------- | ----- |
| *is_object_alarm_enabled* | 是否使用目标检测告警功能 | true/开，false/关 |
| *is_FCW_alarm_enabled*    | 是否使用FCW告警功能      | true/开，false/关 |
| *is_HMW_alarm_enabled*    | 是否使用PCW告警功能      | true/开，false/关 |
| *is_PCW_alarm_enabled*    | 是否使用HMW告警功能      | true/开，false/关 |
| *is_UFCW_alarm_enabled*   | 是否使用UFCW告警功能     | true/开，false/关 |

**2.2.1 object_alarm_shared_property**
目标检测各功能公共配置项

| key word | expression | value |
| -------- | ---------- | ----- |
| *object_alarming_distance_x_min*                 | 目标x方向最小值             | float        |
| *object_alarming_distance_x_max*                 | 目标x方向最大值             | float        |
| *lane_filtering_distance_y_max*                  | 车道线过滤后目标y方向最大值 | float        |
| *lane_filtering_distance_y_min*                  | 车道线过滤后目标y方向最小值 | flaot        |
| *object_default_lane_x_min*                      |                             | flaot        |
| *object_default_lane_x_max*                      |                             | flaot        |
| *object_if_use_width_filter*                     | 是否使用目标宽度过滤器      | true/开，false/关 |
| *object_time_response*                           |                             | float        |
| *object_acceleration_request*                    | 目标加速度要求              | float        |
| *object_acceleration_relative_threshold*         | 目标相对加速度阈值          | float        |
| *object_lane_filter_sectional_distances*         |                             | array[float] |
| *object_lane_filter_sectional_width_fractions*   |                             | array[float] |
| *object_ego_car_acceleration_suppress_threshold* |                             | float        |
| *object_ego_car_speed_table_num*                 |                             | int          |

**2.2.2 object_fcw_common_property**
FCW的特定配置项

| key word | expression | value |
| -------- | ---------- | ----- |
| *active_speed_max*                 | FCW工作的最大车速，超过该车速FCW停止工作    | float |
| *active_speed_min*                 | FCW工作的最小车速，低于该车速FCW停止工作    | float |
| *warning_time*                     | FCW告警时间，预测碰撞时间小于该值时发出告警 | float |
| *object_alarm_block_num_threshold* |  | int |
| *warning_time_stabilize_num*       |  | int |
| *alarming_time*                    | FCW语音告警时间，预测碰撞时间小于该值时发出语音告警 | float |

**2.2.3 object_pcw_common_property**
PCW的特定配置项，参考 *object_fcw_common_property*

**2.2.4 object_hmw_common_property**
HMW的特定配置项，参考 *object_fcw_common_property*

**2.2.5 object_ufcw_common_property**
UFCW的特定配置项，参考 *object_fcw_common_property*

## 3. others
- detect_calibration_input_file
    - [camera.bin] (./camera_bin)文件路径，为sdk生成[calibration.json](./calibration_json.md)提供输入，由client客户端生成，传递到host
    - 不可为空
- detect_calibration_file
    - [calibration.json](./calibration_json.md)文件路径，为sdk提供标定信息
    - 不可为空

