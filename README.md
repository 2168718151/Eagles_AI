# YOLOv8 Android TFLite — 多任务移动端推理合集

基于 **YOLOv8** 模型 + **TensorFlow Lite** 的 Android 端实时推理应用合集，覆盖目标检测、实例分割、图像分类三大视觉任务。全部使用 Kotlin 编写，CameraX 驱动摄像头采集，GPU 委托加速推理。

---

## 📁 子项目

| 子项目 | 任务 | 核心推理文件 |
|--------|------|-------------|
| [`YOLOv8-Object-Detector-Android-Tflite`](./YOLOv8-Object-Detector-Android-Tflite/) | 🔲 目标检测 | `Detector.kt` |
| [`YOLOv8-Instance-Segmentation-Android-Tflite`](./YOLOv8-Instance-Segmentation-Android-Tflite/) | 🎯 实例分割 | `InstanceSegmentation.kt` |
| [`YOLOv8-Image-Classification-Android-Tflite`](./YOLOv8-Image-Classification-Android-Tflite/) | 🏷️ 图像分类 | `ImageClassification.kt` |

每个子项目都是独立的 Android 应用，可直接用 Android Studio 打开构建。

---

## 🧱 技术栈

| 组件 | 版本 / 说明 |
|------|-------------|
| 语言 | Kotlin + Java（`MyClassifierModel.java`） |
| 最低 SDK | 26 (Android 8.0) |
| 目标 SDK | 34 |
| 编译 SDK | 34 |
| JVM Target | 17 |
| 摄像头 | CameraX `1.4.0-beta02` |
| 推理引擎 | TensorFlow Lite `2.16.1` + GPU Delegate |
| UI | ViewBinding + Material Design |
| 图像分类子项目额外依赖 | ViewPager2 |

---

## 📂 代码结构（以目标检测为例）

```
YOLOv8-Object-Detector-Android-Tflite/
├── app/
│   ├── build.gradle.kts          # 依赖配置（TFLite、CameraX 等）
│   └── src/main/
│       ├── assets/               # 放置 .tflite 模型文件
│       ├── java/com/surendramaran/yolov8tflite/
│       │   ├── MainActivity.kt       # 主界面 + 相机绑定
│       │   ├── Detector.kt           # 推理核心：预处理 → 推理 → NMS → 后处理
│       │   ├── BoundingBox.kt        # 检测框数据类
│       │   ├── OverlayView.kt        # 检测框叠加绘制
│       │   ├── Constants.kt          # 模型路径、标签、阈值等配置
│       │   ├── MetaData.kt           # 模型元数据解析
│       │   └── MyClassifierModel.java # TFLite 模型封装（Java）
│       └── res/
│           ├── layout/           # XML 布局
│           └── values/           # 字符串、颜色等资源
├── build.gradle.kts
└── settings.gradle.kts
```

实例分割子项目额外包含：
- `ml/DrawImages.kt` — 分割掩码绘制
- `ml/ImageUtils.kt` — 图像缩放、高斯核
- `ml/SegmentationResult.kt` / `Output0.kt` / `Success.kt` — 分割结果数据类
- `ui/InstanceSegmentationFragment.kt` — Fragment 架构

---

## 🚀 快速开始

### 1. 准备模型

将你训练好的 YOLOv8 模型导出为 `.tflite` 格式，放入对应子项目的 `app/src/main/assets/` 目录。

### 2. 修改配置

编辑对应子项目的 `Constants.kt`，更新模型路径和标签：

```kotlin
// Constants.kt
const val MODEL_PATH = "your_model.tflite"
val LABELS = listOf("class1", "class2", ...)
```

### 3. 构建运行

用 Android Studio 打开子项目目录，Sync Gradle → Build → Run。

---

## 🔗 符号交叉引用

项目根目录下的 [`All_map.md`](./All_map.md) 包含完整的符号交叉引用映射（919 个符号，46 个源文件），每条记录精确到文件:行号，涵盖：

- **imports** — 604 条
- **variables** — 454 条  
- **functions** — 114 条
- **classes/interfaces** — 49 条

快速查找任意函数/类的定义位置和关联关系。

---

## ⚠️ 注意事项

- **实例分割**子项目不支持 int8 量化模型，请使用 **Float32** 或 **Float16** 格式。
- 目标检测使用 **NMS**（非极大值抑制）去除冗余框，IoU 阈值可在 `Constants.kt` 中调整。
- GPU 委托需要设备支持 OpenGL ES 3.1+。

---

## 📄 许可证

本仓库中用于将 YOLO 集成到移动端的代码以 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 授权。

YOLOv8 模型本身以 [AGPL v3](https://www.gnu.org/licenses/agpl-3.0.en.html) 授权。

---

## 📬 联系与支持

- 问题反馈：[GitHub Issues](https://github.com/surendramaran/Machine-Learning-in-Mobile/issues/new)
- 支持作者：[Patreon](https://www.patreon.com/SurendraMaran)
