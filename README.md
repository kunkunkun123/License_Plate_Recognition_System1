# 车牌识别系统项目说明书

## 1. 项目概述

本项目是一个基于Cognex VisionPro的车牌识别系统，采用C#开发的Windows客户端应用程序和Python开发的图像预处理服务器。系统能够对输入的车辆图片进行预处理、车牌区域检测和字符识别，最终输出车牌号码。

### 1.1 项目背景

随着智能交通系统的发展，车牌识别技术在交通管理、安防监控、停车场管理等领域有着广泛的应用。本项目旨在开发一个高效、准确的车牌识别系统，满足上述场景的需求。

### 1.2 项目目标

- 实现对各种光照条件、角度和质量的车辆图片进行车牌识别
- 提供图像增强功能，提高识别率
- 支持手动选择车牌区域，解决自动检测失败的情况
- 提供友好的用户界面和操作流程
- 实现客户端与服务器的高效通信

## 2. 系统架构

系统采用客户端-服务器架构，分为两个主要部分：

### 2.1 客户端 (C# Windows Forms)

- 位置：`/PC_app/WindowsFormsApp1/`
- 负责用户界面交互、图像显示、调用VisionPro进行车牌识别
- 与服务器通信，发送图像并接收预处理结果

### 2.2 服务器端 (Python)

- 位置：`/Python_图像预处理/chepai_server/`
- 提供基于Django的RESTful API服务
- 负责图像预处理，包括图像增强、去噪、二值化等操作
- 优化图像以提高车牌识别率
- 包含车牌区域检测算法

### 2.3 系统流程图

```
+----------------+    +------------------+    +------------------+
|                |    |                  |    |                  |
| 图像输入/加载  |--->| 图像预处理(服务器)|--->| 车牌区域检测    |
|                |    |                  |    |                  |
+----------------+    +------------------+    +------------------+
                                                       |
                                                       v
+----------------+    +------------------+    +------------------+
|                |    |                  |    |                  |
| 结果显示       |<---| 车牌字符识别     |<---| 字符分割        |
|                |    |                  |    |                  |
+----------------+    +------------------+    +------------------+
```

## 3. 功能模块介绍

### 3.1 图像加载模块

- 支持加载JPG、JPEG、PNG、BMP等格式图片
- 自动适应图像大小，进行缩放处理
- 显示原始图像

### 3.2 图像增强模块

- 提供亮度、对比度、饱和度和锐化调整
- 实时预览增强效果
- 保存增强后的图像用于后续处理

### 3.3 手动选择模块

- 支持用户手动框选车牌区域
- 提供放大、缩小和重置缩放功能
- 裁剪选定区域并用于后续处理

### 3.4 服务器通信模块

- 发送图像到服务器进行预处理
- 接收并显示处理后的图像
- 监控服务器状态，每10秒检查一次
- 显示服务器响应时间

### 3.5 车牌识别模块

- 基于Cognex VisionPro的车牌识别引擎
- 加载VPP文件配置识别参数
- 对预处理后的图像进行车牌识别
- 显示识别结果和处理时间

### 3.6 状态管理模块

- 显示系统当前状态：就绪、处理中、成功、错误
- 提供动态状态指示器，显示处理进度
- 记录处理时间和结果信息

## 4. 使用说明

### 4.1 系统要求

- 操作系统：Windows 10及以上
- .NET Framework 4.5及以上
- Cognex VisionPro 9.0及以上
- Python 3.6及以上 (服务器端)
- Django 4.2.* (服务器端)

### 4.2 安装步骤

1. **客户端安装**
   - 安装Cognex VisionPro软件
   - 将`PC_app`文件夹复制到目标计算机
   - 运行`WindowsFormsApp1.exe`

2. **服务器安装**
   - 安装Python 3.6及以上版本
   - 进入`Python_图像预处理/chepai_server/`目录
   - 安装依赖包：`pip install -r requirements.txt`
   - 运行服务器：`python manage.py runserver`

### 4.3 操作流程

#### 基本流程

1. 启动客户端应用程序
2. 确认服务器状态为"在线"
3. 点击"加载图片"按钮，选择要识别的车辆图片
4. 点击"开始识别"按钮，系统会自动完成以下步骤：
   - 将图片发送到服务器进行预处理
   - 接收预处理后的图像
   - 使用VisionPro进行车牌识别
   - 显示识别结果

#### 高级功能

1. **图像增强**
   - 加载图片后，点击"图像增强"按钮
   - 调整亮度、对比度、饱和度和锐化参数
   - 点击"确定"应用增强效果

2. **手动选择车牌区域**
   - 加载图片后，点击"手动选择"按钮
   - 在图像上拖动鼠标选择车牌区域
   - 使用放大/缩小按钮进行精确选择
   - 点击"确定"应用选择

3. **服务器设置**
   - 点击"服务器设置"按钮
   - 输入服务器地址(默认为http://127.0.0.1:8000/app1/files/)
   - 点击"确定"保存设置

## 5. 技术栈

### 5.1 客户端

- 开发语言：C#
- UI框架：Windows Forms
- 图像处理：System.Drawing
- 车牌识别：Cognex VisionPro
- 网络通信：HttpClient

### 5.2 服务器端

- 开发语言：Python
- Web框架：Django 4.2.*
- REST API框架：Django REST Framework 3.14.*
- 图像处理：OpenCV-Python, NumPy, PIL
- 跨域资源共享：django-cors-headers
- WSGI服务器：Gunicorn (生产环境)

## 6. 部署指南

### 6.1 开发环境部署

1. **客户端**
   - 安装Visual Studio 2019及以上
   - 安装Cognex VisionPro SDK
   - 打开`PC_app/WindowsFormsApp1/WindowsFormsApp1.sln`
   - 编译并运行项目

2. **服务器**
   - 安装Python 3.6及以上
   - 进入`Python_图像预处理/chepai_server/`目录
   - 安装依赖：`pip install -r requirements.txt`
   - 应用数据库迁移：`python manage.py migrate`
   - 运行开发服务器：`python manage.py runserver`

### 6.2 生产环境部署

1. **客户端**
   - 使用Release模式编译项目
   - 将编译后的文件和必要的DLL打包
   - 在目标机器上安装Cognex VisionPro Runtime
   - 部署应用程序包

2. **服务器**
   - 安装Python环境
   - 安装依赖：`pip install -r requirements.txt`
   - 配置Django生产设置：
     ```python
     # settings.py
     DEBUG = False
     ALLOWED_HOSTS = ['your-domain.com', 'localhost', '127.0.0.1']
     ```
   - 收集静态文件：`python manage.py collectstatic`
   - 使用Gunicorn运行：`gunicorn chepai_server.wsgi:application`
   - 配置Nginx作为反向代理

### 6.3 配置说明

1. **客户端配置**
   - 配置文件位置：应用程序目录下的`config.txt`
   - 可配置项：服务器URL

2. **服务器配置**
   - 配置文件位置：`Python_图像预处理/chepai_server/chepai_server/settings.py`
   - 可配置项：数据库设置、媒体文件路径、静态文件路径、调试模式等

## 7. 常见问题解答

### 7.1 识别问题

**Q: 为什么车牌识别结果包含"?"字符？**

A: 这表示某些字符无法被准确识别。可能的原因包括：
- 图像质量不佳
- 光照条件不理想
- 车牌角度不正
- 车牌部分被遮挡

解决方案：
- 使用图像增强功能调整图像
- 使用手动选择功能精确框选车牌区域

**Q: 识别速度较慢怎么办？**

A: 识别速度受多种因素影响：
- 图像分辨率过高
- 服务器负载过重
- 网络连接不稳定

解决方案：
- 使用较低分辨率的图像
- 确保服务器资源充足
- 检查网络连接

### 7.2 服务器问题

**Q: 服务器状态显示为"离线"怎么办？**

A: 可能的原因包括：
- Django服务器未启动
- 服务器地址配置错误
- 网络连接问题

解决方案：
- 确认Django服务器已启动：`python manage.py runserver`
- 检查服务器地址配置，默认应为`http://127.0.0.1:8000/app1/files/`
- 检查网络连接和防火墙设置

**Q: 如何在不同计算机上部署服务器？**

A: 步骤如下：
1. 在目标计算机上安装Python环境
2. 复制`Python_图像预处理`文件夹到目标计算机
3. 进入`chepai_server`目录
4. 安装依赖：`pip install -r requirements.txt`
5. 应用数据库迁移：`python manage.py migrate`
6. 运行服务器：`python manage.py runserver 0.0.0.0:8000`（允许外部访问）
7. 在客户端中配置新的服务器地址

### 7.3 其他问题

**Q: 如何添加新的VPP文件？**

A: 将新的VPP文件复制到应用程序目录，并命名为`VisionPro_chepaishibie.vpp`，或在启动应用程序后通过加载图片时选择加载VPP文件。

**Q: 临时文件占用磁盘空间怎么办？**

A: 系统会在`%TEMP%/PlateRecognition`目录下创建临时文件，大多数临时文件会在使用后自动删除。如果发现临时文件堆积，可以手动清理该目录。

**Q: 服务器端如何查看上传的图片？**

A: 服务器保存上传的图片在`media`目录下，可以通过Django管理界面或直接访问`http://127.0.0.1:8000/app1/file-list/`查看文件列表。

## 8. 项目结构

### 8.1 客户端结构

```
PC_app/
└── WindowsFormsApp1/
    ├── WindowsFormsApp1.sln    # Visual Studio解决方案文件
    └── WindowsFormsApp1/       # 主项目目录
        ├── Form1.cs            # 主窗体代码
        ├── ImageEnhancementForm.cs  # 图像增强窗体
        ├── ManualSelectionForm.cs   # 手动选择窗体
        └── Program.cs          # 程序入口点
```

### 8.2 服务器端结构

```
Python_图像预处理/
├── chepai_server/             # Django项目目录
│   ├── app1/                  # 主应用目录
│   │   ├── migrations/        # 数据库迁移文件
│   │   ├── models.py          # 数据模型定义
│   │   ├── serializer.py      # REST API序列化器
│   │   ├── tuxiang.py         # 图像处理核心代码
│   │   ├── urls.py            # URL路由配置
│   │   └── views.py           # 视图函数和API端点
│   ├── chepai_server/         # 项目配置目录
│   │   ├── settings.py        # 项目设置
│   │   ├── urls.py            # 主URL配置
│   │   └── wsgi.py            # WSGI应用配置
│   ├── media/                 # 上传文件存储目录
│   ├── manage.py              # Django管理脚本
│   └── requirements.txt       # 依赖包列表
├── 待处理图像/                # 待处理图像示例目录
└── 已处理图片/                # 处理结果示例目录
```
