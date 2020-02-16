# alarm_simulator_config.json 配置说明
===========================================================

alarm_simulator_config.json为工具alarm_simulator的配置文件，alarm_simulator旨在提供adas-app的离线仿真功能，因此配置文件包含adas-app的配置文件[config.yml](./config_yml.md)路径、[sdk_config.json](./sdk_config_json.md)路径，simulator的I/O配置


- [1. input_property](#input-property)
- [2. output_property](#output-property)
    - [2.1 draw_property](#draw-property)



-----------------

## 1. input_property
输入相关配置

- adas_app_config_yml_path ：adas-app的配置文件[config.yml](./config_yml.md)路径

- sdk_config_json_path ：[sdk_config.json](./sdk_config_json.md)路径

- input_mode ：输入数据模式

| input_mode | expression | input data |
| ---------- | ---------- | ---------- |
| INPUT_DIR_PATH  | 从目录中收集待处理数据     | input_dir_path  |
| INPUT_JSON_SETS | 从json文件中收集待处理数据 | input_json_sets |

- input_dir_path
    - original_image_dir_path ：路测时保存原始图片的目录路径
    - roadtest_result_dir_path ：路测试保存的result json文件的目录路径

- input_json_sets ：array[string]，json数据集文件路径，每个json数据集文件中，每一个item包括ori_image和对应的roadtest_result_json的文件路径

## 2. output_property
输出相关配置

- output_dir_path ：输出目录路径

- is_save_simulator_result_img ：开关项，是否保存simulator的detection result图片，true/是，false/否

- is_save_simulator_result_json ：开关项，是否将simulator的detection result保存为json文件，true/是，false/否

- is_compare_with_roadtest ：开关项，是否和roadtest_result_json中的信息进行比较，目前为保留项，true/是，false/否

### 2.1 draw_property
绘制结果相关配置

- is_draw_simu_bbox_on_json_img ：开关项，是否将仿真的bbox和json中的bbox画在图片上，true/是，false/否

- is_draw_image_boundaries_line ：开关项，是否绘制calibration.json中定义的图像分割线到图片上，目前为保留项，true/是，false/否

- draw_type ：绘制方式
> “JOIN” 将仿真结果和路测结果拼接成图片；
> 
> “OVERLAP” 将仿真结果和路测记过画在同一张图片上


