# Cesium 雪花效果演示

这是一个基于 Cesium 和 Three.js 的下雪效果演示项目，将 Three.js 粒子系统创建的雪花效果与 Cesium 地球场景无缝集成。

## 功能特性

### 🎨 视觉效果
- 逼真的雪花粒子效果
- 基于 Simplex 噪声的自然风场模拟
- 支持多细节层次的噪声控制
- 可调节的雪花数量、大小、透明度和颜色

### 📷 相机同步
- Three.js 相机与 Cesium 相机无缝同步
- 支持实时调整相机视角（航向角、俯仰角、翻滚角）
- 支持鼠标控制和 GUI 控制面板

### ⚙️ 可调节参数
- **雪花状态**：数量、大小、透明度、颜色
- **风场环境**：风速、下落速度、侧向风力、噪声强度、噪声密度、风场变化率
- **相机控制**：视野角度、位置调整
- **噪声控制**：噪声细节层次、重新生成噪声

### 🖥️ 用户界面
- 直观的控制面板（基于 lil-gui）
- 支持中英文对照的参数名称
- 实时参数调整和预览

## 技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| Cesium | 1.121 | 3D地球场景渲染 |
| Three.js | 0.160.0 | 雪花粒子系统 |
| simplex-noise | 4.0.1 | 风场噪声生成 |
| lil-gui | - | 交互式控制面板 |

## 使用方法

### 本地运行

1. 克隆或下载项目到本地

2. 在项目根目录启动本地服务器：

   ```bash
   python -m http.server 8000
   ```

3. 在浏览器中访问：
   ```
   http://localhost:8000/cesium-hellow.html
   ```

### 在线访问

项目已部署到 GitHub Pages，可直接访问：
- 主页面：<https://ccwq.github.io/cesium-snow-demo/cesium-hellow.html>

> 如果遇到 404，请先确认 GitHub Actions 部署已成功（仓库的 “Actions” > “Deploy static site to GitHub Pages” 工作流）。

## 项目结构

```
cesium-snow-demo/
├── cesium-hellow.html     # 主演示文件
├── snow.html              # 简化版雪花效果
├── .history/              # IDE历史文件（无需关注）
├── .trae/                 # 项目文档和计划
│   └── documents/         # 项目相关文档
└── README.md              # 项目说明文档
```

## 核心功能说明

### 雪花粒子系统

雪花粒子系统使用 Three.js 的 `Points` 和 `BufferGeometry` 创建，具有以下特点：

- 动态生成的雪花纹理
- 基于物理的运动模拟
- 边界检测和粒子重生
- 支持大量粒子（最高50,000个）

### 风场模拟

使用 Simplex 噪声生成自然的风场效果：

- 多八度噪声叠加，模拟复杂风场
- 可调节的噪声参数
- 支持实时重新生成噪声

### 相机同步机制

Three.js 相机与 Cesium 相机的同步实现：

- 实时获取 Cesium 相机的位置和方向
- 转换相机坐标系到 Three.js 空间
- 确保雪花效果与地球场景无缝融合

## 控制面板使用

项目提供了直观的 GUI 控制面板，分为以下几个部分：

### 📷 相机控制 (Camera HPR)
- 启用/禁用鼠标控制
- 调整航向角、俯仰角、翻滚角
- 调整相机位置
- 设置视野角度

### 🌬️ 环境与风场 (Environment)
- 调整整体风速
- 调整下落速度和侧向风力
- 调整噪声强度和密度
- 调整风场变化率

### 🎲 噪声控制 (Noise)
- 调整噪声细节层次
- 重新生成噪声
- 预览噪声场（预留功能）

### ❄️ 雪花状态 (Snow)
- 调整雪花数量
- 调整雪花大小
- 调整透明度
- 调整雪花颜色

## 部署说明

### GitHub Pages 部署

> 仓库默认分支是 `master`（非 `main`），工作流会自动将 `master` 分支发布到 GitHub Pages。

1. 确保项目已推送到 GitHub 仓库（默认分支：`master`）

2. 在 GitHub 仓库中：
   - 进入 "Settings" 页面
   - 点击 "Pages" 选项
   - 在 "Build and deployment" 部分，选择：
     - Source: `GitHub Actions`
   - 点击 "Save"

3. “Actions” 页面会自动触发 **Deploy static site to GitHub Pages** 工作流，完成后 Pages 站点会生成（地址如上）。

4. 访问 `https://<username>.github.io/<repository-name>/cesium-hellow.html` 查看效果

### 其他部署方式

项目使用纯静态 HTML/CSS/JavaScript 开发，可以部署到任何静态网站托管服务，如：
- Vercel
- Netlify
- Cloudflare Pages
- 传统 Web 服务器

## 浏览器兼容性

| 浏览器 | 兼容性 |
|--------|--------|
| Chrome | ✅ 支持 |
| Firefox | ✅ 支持 |
| Safari | ✅ 支持 |
| Edge | ✅ 支持 |

## 性能优化

- 使用 `BufferGeometry` 提高粒子渲染性能
- 启用 `AdditiveBlending` 增强视觉效果
- 关闭 `depthWrite` 减少绘制开销
- 使用 `powerPreference: "high-performance"` 优先使用高性能 GPU

## 开发说明

### 核心文件

- `cesium-hellow.html`：主演示文件，包含完整的 Cesium 和 Three.js 集成代码
- `snow.html`：简化版雪花效果，仅包含 Three.js 粒子系统

### 关键函数

- `initThreeJS()`：初始化 Three.js 场景和渲染器
- `initParticles()`：创建和初始化雪花粒子
- `syncCesiumToThreeCamera()`：同步 Cesium 相机到 Three.js 相机
- `render()`：主渲染循环，包含粒子更新和风场计算

## 许可证

本项目采用 MIT 许可证，详见 LICENSE 文件。

## 贡献

欢迎提交 Issue 和 Pull Request！

## 致谢

- [Cesium](https://cesium.com/) - 3D 地球和地理空间可视化库
- [Three.js](https://threejs.org/) - 3D 图形库
- [simplex-noise](https://github.com/jwagner/simplex-noise.js) - Simplex 噪声生成库
- [lil-gui](https://github.com/georgealways/lil-gui) - 轻量级 GUI 库

## 联系方式

如有问题或建议，欢迎通过以下方式联系：
- GitHub Issues：<https://github.com/ccwq/cesium-snow-demo/issues>
- Email：[请替换为您的邮箱]  

---

**Enjoy the snow! ❄️**
