# Infinite Terrain Generator

一个完全离线、零依赖的单文件程序化地形生成器。通过确定性 Seed 创建可连续扩展的 Chunk 地形，支持笔刷编辑、范围管理、本地存档，以及 GLB、glTF、JSON 和 PNG 导出。

程序仅由原生 HTML、CSS、JavaScript 和 Canvas 2D 构成，无需安装、构建、服务器或外部资源。

![\images\image1.png]

## 主要功能

- Seed 驱动的确定性无限地形
- 5×5 可编辑 Chunk 窗口与外围 Ghost Chunk
- FBM、Ridged Noise、Domain Warp 与地形平滑
- 抬高、降低、平滑、压平四种地形笔刷
- 边缘保护、撤销与重做
- 已生成 Chunk 的选择、框选、清除与范围管理
- 基于稀疏 Delta 的浏览器本地存档
- 导出 GLB、glTF、世界记录 JSON 和当前视图 PNG
- 线框、白模和地貌着色三种预览模式

## 快速开始

下载或克隆仓库，直接使用现代浏览器打开：

```text
Infinite_Terrain_Generator.html
```

程序可通过 `file://` 完全离线运行。遇到浏览器本地文件限制时，也可启动静态服务器：

```bash
python -m http.server 8080
```

然后访问：

```text
http://localhost:8080/Infinite_Terrain_Generator.html
```

## 操作

### 地形视图

| 操作               | 功能   |
| ---------------- | ---- |
| 左键拖动             | 旋转视角 |
| 右键拖动             | 平移视角 |
| 滚轮               | 缩放视角 |
| 启用笔刷后左键拖动        | 编辑地形 |
| 启用笔刷后 Alt + 左键拖动 | 旋转视角 |

### Chunk 范围地图

| 操作         | 功能          |
| ---------- | ----------- |
| 单击         | 切换 Chunk 选择 |
| 拖框         | 替换当前选择      |
| Shift + 拖框 | 追加选择        |
| Alt + 拖框   | 移除选择        |
| 右键拖动       | 平移地图        |
| 滚轮         | 缩放地图        |

只有被选中的已生成 Chunk 会进入 GLB 或 glTF 文件。

## 工作原理

当前编辑窗口为 5×5 Chunk，外围加载一圈 Ghost Chunk，用于连续计算和接缝过渡。所有窗口共享全局顶点坐标，笔刷编辑仅保存相对于基础地形的稀疏高度差值，因此无需保存完整无限高度图。

浏览器会根据 Seed、分辨率、块尺寸和地形参数生成独立的 `localStorage` 存档。建议定期导出 JSON 世界记录进行备份。

![\images\image2.png]

![\images\image3.png]

## 导出

- **GLB**：单一二进制 glTF 2.0 文件。
- **glTF**：内嵌 Base64 Buffer 的单一 `.gltf` 文件。
- **JSON**：保存 Seed、参数、Chunk 清单和稀疏 Delta。
- **PNG**：保存当前 Canvas 视图。

当前 glTF 场景结构：

```text
InfiniteTerrain_Root
├─ Chunk_<x>_<z>
├─ Chunk_<x>_<z>
└─ ...
```

每个 Chunk 为独立 Mesh，可分别导入 Blender、Godot、Unity、Unreal Engine 或其他支持 glTF 2.0 的工具。

## 当前限制

- 当前导出仅包含地形上表面，不是封闭网格。
- 暂不生成水体、碰撞体、导航网格、LOD 或纹理贴图。
- Canvas 2D 预览由 CPU 渲染，大范围高分辨率场景可能影响性能。
- 超大范围导出可能受到浏览器内存限制。

## 仓库结构

```text
.
├─ Infinite_Terrain_Generator.html
├─ README.md
└─ LICENSE
```

## 许可证

本项目采用 [MIT License](LICENSE) 开源许可证。
