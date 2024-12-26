# RADIATE 项目运行教程

本教程将指导您如何运行 RADIATE 项目，包括环境设置、数据准备、代码运行步骤等。

---

## 1. 环境配置

### 1.1 安装 Python
- 本项目需要 Python 3.8 或更高版本。检查您是否已安装 Python：
  ```bash
  python --version
  ```
  或者
  ```bash
  python3 --version
  ```

- 如果未安装，请从 [Python 官网](https://www.python.org/) 下载并安装。

---

### 1.2 创建虚拟环境（推荐）
为避免依赖冲突，建议使用虚拟环境运行项目。

#### 在 Linux/MacOS
```bash
python3 -m venv venv
source venv/bin/activate
```

#### 在 Windows
```bash
python -m venv venv
venv\Scripts\activate
```

---

### 1.3 安装项目依赖
在激活的虚拟环境中，运行以下命令安装所需的依赖库：

```bash
pip install -r requirements.txt
```

如果项目没有提供 `requirements.txt` 文件，可以手动安装以下主要依赖：
```bash
pip install numpy opencv-python matplotlib pandas pyyaml
```

---

## 2. 数据准备

### 2.1 下载 RADIATE 数据集
从 [RADIATE 官方网站](https://github.com/valentinbarral/radiate-dataset) 下载数据集，并解压到本地。例如，将数据集解压到 `data/radiate/` 目录中。

### 2.2 检查数据结构
确保数据目录结构类似于以下格式：
```
data/radiate/
└── tiny_foggy/
    ├── zed_left/               # 左相机图像
    ├── zed_right/              # 右相机图像
    ├── velo_lidar/             # 激光雷达数据
    ├── Navtech_Cartesian/      # 雷达数据
    ├── annotations/annotations.json  # 注释文件
    ├── config/config.yaml      # 配置文件
    ├── timestamps/
        ├── camera.txt          # 相机时间戳
        ├── lidar.txt           # 激光雷达时间戳
        ├── radar.txt           # 雷达时间戳
```

---

## 3. 项目运行

### 3.1 配置项目路径
确保在 `radiate.py` 文件中正确设置了数据集的路径。例如：
```python
root_path = 'data/radiate/'
sequence_name = 'tiny_foggy'
```

### 3.2 运行项目代码
在项目根目录下运行主程序文件：
```bash
python main.py
```
或直接运行提供的示例代码：
```bash
python example.py
```

---

## 4. 运行过程说明

1. **加载数据序列**
   - 代码会从 `data/radiate/tiny_foggy/` 加载相机、激光雷达、雷达等传感器数据。
   - 同时加载时间戳文件和注释数据。

2. **逐帧播放**
   - 根据设定的时间步长 `dt`（默认 0.25 秒），逐帧提取传感器数据。
   - 可视化结果会通过 OpenCV 的 `cv2.imshow` 显示在多个窗口中。

3. **显示内容**
   - 左相机和右相机的图像（可以叠加目标物体的 3D 边界框）。
   - 激光雷达的鸟瞰图。
   - 激光雷达投影到相机图像的结果。
   - 雷达图像及其注释。

---

## 5. 常见问题排查

### 问题 1：无法找到某些模块
**解决方案**：
- 确认已安装所有依赖库。
- 检查 Python 的路径是否正确，或在运行时使用虚拟环境。

### 问题 2：数据文件加载错误
**解决方案**：
- 检查数据路径是否正确。
- 确保数据解压后的目录结构符合项目要求。

### 问题 3：显示窗口卡顿或关闭
**解决方案**：
- 检查 `seq.vis_all(output, 0)` 中的 `wait_time` 参数，设置为较大的值（如 `100`）以暂停一段时间观察结果。

---

## 6. 高级设置

### 修改时间步长
您可以通过调整 `dt` 参数更改帧间的时间间隔：
```python
dt = 0.1  # 每 0.1 秒显示一帧
```

### 保存可视化结果
如果需要保存可视化结果，可以修改 `config.yaml` 文件中的 `save_images` 参数为 `True`。

---

## 7. 总结

通过本教程，您应该能够成功运行 RADIATE 项目并可视化传感器数据。如果需要更多的自定义功能，可以参考 `radiate.py` 文件中的方法进行扩展。