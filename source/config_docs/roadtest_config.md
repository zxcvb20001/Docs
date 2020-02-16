# roadtest config.json 配置说明
================================

roadtest config.json为roadtest工具的配置文件，提供framework所需的sdk_config.json的文件路径、网络通信配置、语音告警配置和PC端可视化配置。

- [1. CoreNodeProperty](#corenodeproperty)
- [2. AlarmNodeProperty](#alarmnodeproperty)
- [3. PreviewNodeProperty](#previewnodeproperty)
- [4. Others](#others)
    - sender_bind_address
    - reciever_bind_address
    - CalibrationFile
    - sdk_config_path


-----------------


## 1. CoreNodeProperty
FPGA端可执行文件配置

- input_type ：输入类型，默认为1
| input_type | expression | enum value |
| ---------- | ---------- | ---------- |
| 0          | 从USB摄像头输入视频流  | USB_CAMERA_INPUT    |
| 1          | 从相机模组中输入视频流 | CAMERA_STREAM_INPUT |
| 2          | 从视频文件中输入视频流 | VIDEO_INPUT         |

- camera_property
    - camera_device ：设备名称，默认“/dev/ttyUSB0”
    - height ：输入图像height
    - width ：输入图像width

- gps_device ：保留项

- video_path：当input_type为2时有效，表示输入视频路径

- run_fcw ：FCW功能开关项，true/开，false/关

- run_lka ：LKA功能开关项，true/开，false/关


## 2. AlarmNodeProperty
语音模块可执行文件配置

- interests ：可执行程序通过网络接收关键字列表

## 3. PreviewNodeProperty
PC端可视化可执行文件配置

- interests ：可执行程序通过网络接收关键字列表

- is_write_result_video_enabled ：功能开关，是否保存detection结果视频，true/开，false/关

- is_write_original_video_enabled ：功能开关，是否保存原始视频，true/开，false/关

- video_path ：detection结果视频保存目录路径，linux下最好末尾加'/'

- original_video_path ：原始视频保存目录路径，linux下最好末尾加'/'

- images_path ：图片保存目录路径，linux下最好末尾加'/'

- fps ：保存视频时的帧率，默认20

- log_property
    - is_output_to_txt_enabled ：功能开关，是否打印log输出，true/开，false/关
    - default_output_path ：log保存目录路径，linux下最好末尾加'/'


## 4. Others

- sender_bind_address ：指定发送端（ADAS产品）发送端口

- reciever_bind_address ：指定接收端（PC）接收某个IP地址发送的报文（这里为ADAS产品的IP地址）

- CalibrationFile ： [calibration.json](./calibration_json.md) 文件路径，用于转换世界坐标系和像素坐标系

- sdk_config_path ： [sdk_configl.json](./sdk_config_json.md) 文件路径，用于配置ADAS SDK
