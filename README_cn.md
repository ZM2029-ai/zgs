# 基于 RDK X5 的博物馆智能服务与安防巡检机器人

<p align="center">
  <b>Smart Museum Service & Security Patrol Robot Based on Horizon RDK X5</b>
</p>

<p align="center">
基于边缘AI、ROS2、云边协同、多传感融合技术的智能移动机器人系统
</p>


## 📖 项目简介

本项目设计并实现了一款面向博物馆场景的智能服务与安防巡检机器人系统。

机器人基于**地平线 Horizon RDK X5 边缘AI计算平台**，融合 ROS2 Humble、YOLOv8n视觉检测、SLAM自主导航、物联网云平台以及多模态人机交互技术，实现了：

- 白天智能导览服务
- 夜间自主安防巡检
- 展柜状态智能检测
- 环境安全监测
- 云端数据管理
- 智能门禁认证
- 安全出口障碍清理


系统采用：

```
边缘AI主控 + IoT节点 + 云平台
```

的云边协同架构，实现机器人自主运行、智能感知和远程管理。


---

# ✨ 系统功能


## 1. 智能导览模式

开放时间内，机器人可作为智能讲解员：

- 🎤 语音交互
- 🗺️ 自主导航到指定展区
- 🔊 自动播放展品讲解
- 🌐 支持中英文交互
- 😊 ESP32-S3屏幕显示表情及提示信息


工作流程：

```
游客语音请求

        ↓

ESP32-S3语音识别

        ↓

ROS2任务管理

        ↓

Nav2路径规划

        ↓

机器人自主到达目标位置

        ↓

语音讲解
```


---

## 2. 夜间安防巡检模式

闭馆后机器人自动进入巡检状态：

主要功能：

- 展柜玻璃破损检测
- 展柜门关闭状态检测
- 温湿度监测
- 烟雾检测
- 火焰检测
- 异常事件云端报警


异常处理流程：

```
视觉/传感器异常

        ↓

ROS2任务管理器

        ↓

本地语音报警

        ↓

MQTT上传云平台

        ↓

Flutter APP通知管理员
```


---

# 🏗️ 系统整体架构


```
                 华为云 IoT

                      ↑

                    MQTT

                      ↑

        ----------------------------

        |                          |

   BearPi IoT节点            独立安防节点


        |

       UART

        |

        ↓


              Horizon RDK X5

        ----------------------------

        |            |             |

       ROS2        AI推理        导航

        |            |             |

      Nav2       YOLOv8n        SLAM


        |

------------------------------------

|              |                   |

激光雷达     深度相机          摄像头

```


---

# 🔧 硬件组成


## 核心计算平台

| 模块 | 参数 |
|----|----|
| 主控平台 | Horizon RDK X5 |
| CPU | Cortex-A53 四核 |
| AI算力 | 5 TOPS NPU |
| 操作系统 | Ubuntu 22.04 |
| 软件框架 | ROS2 Humble |


---

## 传感器系统

|设备|功能|
|-|-|
|MS200激光雷达|SLAM建图、避障|
|Astra Pro深度相机|深度检测、障碍感知|
|MIPI摄像头|AI视觉检测|
|DHT11|温湿度采集|
|MQ-2|烟雾检测|
|火焰传感器|火灾检测|


---

## 控制与交互模块

|控制器|功能|
|-|-|
|STM32F103|底盘运动控制|
|ESP32-S3|语音交互、显示|
|BearPi|IoT数据上传|
|OpenMV|人脸识别、状态检测|


---

# 🤖 AI视觉检测系统


## YOLOv8n展柜检测模型

针对博物馆展柜场景，自建数据集训练轻量化检测模型。


检测目标：

- Glass Crack（玻璃破损）
- Hole（孔洞）
- Door State（柜门状态）


模型部署流程：

```
数据采集

 ↓

人工标注

 ↓

YOLOv8n训练

 ↓

ONNX转换

 ↓

BPU量化

 ↓

RDK X5部署
```


模型特点：

- INT8量化
- NPU硬件加速
- 无需云端推理
- 实时边缘检测


性能指标：

|指标|结果|
|-|-|
|推理速度|12 FPS|
|玻璃破损检测准确率|96.8%|
|柜门状态识别准确率|93.5%|


---

# 🗺️ ROS2自主导航系统


软件架构：

```
ROS2 Humble

        |

       Nav2

        |

 ----------------

 |              |

SLAM          AMCL

 |              |

地图构建       定位


        |

      TEB Planner

        |

     底盘控制
```


实现功能：

- GMapping建图
- AMCL定位
- TEB局部路径规划
- 麦克纳姆轮全向移动
- 动态避障


性能：

|指标|参数|
|-|-|
|定位误差|≤5cm|
|建图面积|≤500㎡|
|导航方式|自主规划|


---

# ☁️ 云边协同系统


数据链路：

```
传感器

 ↓

RDK X5

 ↓

BearPi

 ↓

MQTT

 ↓

华为云IoT

 ↓

Flutter APP
```


实现：

- 实时数据上传
- 异常报警
- 历史数据查询
- 机器人状态监控


---

# 🚪 智能门禁系统


采用：

```
人脸识别 + 指纹识别
```

双重身份认证。


硬件：

- STM32F103
- AS608指纹模块
- OpenMV人脸识别模块
- OLED显示


认证流程：

```
人脸验证

    +

指纹验证

    ↓

身份确认

    ↓

门锁开启
```


特点：

- 双重安全认证
- 非法用户拒绝访问
- 支持日志记录


---

# 🦾 安全出口智能清障


机器人利用深度相机检测：

- 通道障碍物
- 障碍物空间位置


处理流程：

```
目标检测

 ↓

三维坐标计算

 ↓

机械臂运动规划

 ↓

抓取并移除障碍
```


测试结果：

- 抓取成功率：85%
- 平均处理时间：6.2s


---

# 📊 系统性能测试


|测试项目|性能|
|-|-|
|展柜玻璃破损检测|96.8%|
|展柜关闭状态检测|93.5%|
|火焰检测|100%|
|云端上传延迟|≤2s|
|语音响应时间|≤1s|
|机械臂清障成功率|85%|
|门禁认证准确率|100%|


---

# 📂 项目目录结构


```
Museum-Robot

│

├── ros2_ws

│   ├── robot_bringup

│   ├── navigation

│   ├── perception

│   └── communication


├── yolo_model

│   ├── dataset

│   ├── weights

│   └── deployment


├── stm32

│   ├── chassis_controller

│   ├── door_control

│   └── arm_control


├── esp32

│   └── voice_display


├── cloud

│   └── mqtt_service


└── docs

    └── design_document.pdf

```


---

# 🚀 后续优化方向


- 使用YOLOv9等模型提升检测能力
- 引入视觉SLAM增强弱纹理环境定位
- 增加自主充电功能
- 集成大语言模型实现智能问答
- 多机器人协同巡检
- 增加AR室内导航


---

# 🛠️ 技术栈


## Robot

- ROS2 Humble
- Nav2
- SLAM
- TEB Planner


## AI

- YOLOv8n
- ONNX
- Horizon BPU
- INT8量化部署


## Embedded

- STM32
- ESP32-S3
- OpenMV


## Cloud

- MQTT
- Huawei Cloud IoT
- Flutter APP


---

# 📚 References


- ROS2 Documentation
- Horizon RDK X5 Developer Documentation
- YOLOv8 Documentation
- Nav2 Navigation Framework
- Huawei Cloud IoT Documentation


---

# ⭐ 项目总结


本项目完成了一套面向智慧博物馆场景的智能机器人系统，实现了：

**智能导览 + 自主巡检 + AI视觉检测 + 云端管理 + 智能安防**

通过边缘AI计算、多传感融合以及ROS2机器人框架，实现了机器人从环境感知、任务决策到执行控制的完整闭环。


欢迎 Star ⭐
