# Facial Expression Capture Plugin

面部表情捕捉插件 - 从 2D 图像（包括漫画、插图）中提取面部表情并映射到 3D 模型

## 功能特性

- 🎭 **表情提取**: 从 2D 图像中提取面部表情（支持真人和动漫风格）
- 🎯 **ARKit 兼容**: 输出标准 ARKit 52 个 blendshape 参数
- 🔧 **插件支持**: 提供 Blender 和 Unity 插件
- 📦 **批量处理**: 支持单张图像或序列图像处理

## 架构设计

```
输入图像 (R18表情图)
    ↓
面部检测 & 关键点提取 (MediaPipe/OpenCV)
    ↓
表情特征分析
    ↓
ARKit Blendshape 映射 (52个参数)
    ↓
导出格式:
  - JSON (通用)
  - FBX (Blender/Unity)
  - Alembic (动画缓存)
```

## ARKit Blendshape 列表

支持的 52 个标准 ARKit 表情单元：

### 眼部 (16个)
- eyeBlinkLeft, eyeBlinkRight
- eyeLookDownLeft, eyeLookDownRight
- eyeLookInLeft, eyeLookInRight
- eyeLookOutLeft, eyeLookOutRight
- eyeLookUpLeft, eyeLookUpRight
- eyeSquintLeft, eyeSquintRight
- eyeWideLeft, eyeWideRight

### 眉毛 (8个)
- browDownLeft, browDownRight
- browInnerUp
- browOuterUpLeft, browOuterUpRight

### 嘴部 (28个)
- mouthClose
- mouthFunnel, mouthPucker
- mouthLeft, mouthRight
- mouthSmileLeft, mouthSmileRight
- mouthFrownLeft, mouthFrownRight
- mouthDimpleLeft, mouthDimpleRight
- mouthStretchLeft, mouthStretchRight
- mouthRollLower, mouthRollUpper
- mouthShrugLower, mouthShrugUpper
- mouthPressLeft, mouthPressRight
- mouthLowerDownLeft, mouthLowerDownRight
- mouthUpperUpLeft, mouthUpperUpRight

### 脸颊和下巴 (6个)
- cheekPuff
- cheekSquintLeft, cheekSquintRight
- jawOpen, jawForward
- jawLeft, jawRight

## 安装

```bash
cd /home/user/PromptHMR/facial_capture
pip install -r requirements.txt
```

## 使用方法

### Python API

```python
from facial_capture import FacialExpressionCapture

# 初始化
capturer = FacialExpressionCapture()

# 从图像提取表情
blendshapes = capturer.extract_from_image("input.jpg")

# 导出为 ARKit 格式
capturer.export_arkit("output.json", blendshapes)
```

### Blender 插件

1. 打开 Blender
2. Edit > Preferences > Add-ons > Install
3. 选择 `blender_addon/facial_capture_addon.py`
4. 启用插件
5. 在 3D View 侧边栏找到 "Facial Capture" 面板

### Unity 插件

1. 将 `unity_plugin/` 文件夹复制到 Unity 项目的 `Assets/` 目录
2. 在 Unity 中打开 Window > Facial Capture
3. 拖拽图像到面板并点击 "Extract Expression"

## 技术栈

- **面部检测**: MediaPipe Face Mesh (478 个关键点)
- **表情分析**: 基于几何特征和深度学习的混合方法
- **导出格式**: JSON, FBX, Alembic
- **插件框架**: Blender Python API, Unity C# API

## 文件结构

```
facial_capture/
├── core/
│   ├── detector.py          # 面部检测
│   ├── expression_analyzer.py # 表情分析
│   ├── arkit_mapper.py      # ARKit 映射
│   └── exporter.py          # 导出器
├── blender_addon/
│   └── facial_capture_addon.py
├── unity_plugin/
│   ├── FacialCapture.cs
│   └── ARKitBlendshapes.cs
├── models/                   # 预训练模型
└── examples/                 # 示例代码
```

## License

遵循 PromptHMR 项目的许可证
