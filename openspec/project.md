# Project Context

## Purpose
基于 Cesium 与 Three.js 的纯前端雪景演示，展示叠加粒子雪花、风场噪声和相机同步的实现方法，方便在浏览器端快速验证与演示。

## Tech Stack
- 纯静态 HTML + ES Modules（无打包流程）
- CesiumJS 1.121（CDN 引用）用于三维地球渲染
- Three.js 0.160.0（importmap 指向 jsdelivr）用于粒子雪花与渲染
- simplex-noise 4.0.1 生成风场噪声
- lil-gui（three/addons 内置版本）提供参数控制面板
- 开发/本地预览：`python -m http.server 8000` 直接起静态服务器

## Project Conventions

### Code Style
- 使用 ES Module 直接在 `<script type="module">` 中编写，依赖 CDN importmap
- JS 变量/函数使用 camelCase，常量用 const，参数集中在单一 `params` 对象
- 保持 4 空格缩进、分号结尾，少量必要的中英注释说明关键逻辑
- 核心逻辑集中在 `cesium-hellow.html`，避免额外打包/构建步骤

### Architecture Patterns
- 单页叠加架构：Cesium Viewer 作为底层地球场景，Three.js 通过透明 Canvas (`#threeCanvas`) 覆盖在上层
- 全局状态：`viewer`（Cesium）、`scene/renderer/threeCamera`（Three）、`params`（可调参数）、`particleData`（粒子状态）
- 功能模块化函数：`initThreeJS`、`initParticles`、`syncCesiumToThreeCamera`、`render/animate`、`initGUI` 等
- 资源加载：全部走 CDN（Cesium/Lil-GUI/Three.js/simplex-noise），无本地构建产物
- 影像底图：默认清空自带图层后加载 OpenStreetMap，无需密钥

### Testing Strategy
- 以人工体验为主：本地起静态服后在浏览器打开 `http://localhost:8000/cesium-hellow.html`
- 核验要点：雪花粒子出现且数量/大小/颜色可调；相机同步正常；风场噪声随参数变化；GUI 控件可实时生效
- 浏览器兼容性手动检查：Chrome/Firefox/Safari/Edge 均应正常渲染
- 性能回归：调高粒子数量、调整抗锯齿与 `powerPreference`，确认帧率可接受

### Git Workflow
- 默认分支为 `master`，推送触发 GitHub Pages 工作流部署静态站点
- 轻量变更可直接在 `master` 上提交；较大改动建议分支提交后合并
- 保持提交信息简洁描述本次改动范围（如粒子参数、相机同步、UI 调整）

## Domain Context
- 场景是 3D 地球（Cesium）叠加 Three.js 粒子雪花，依赖相机姿态同步与风场噪声驱动
- 粒子通过 `Points + BufferGeometry` 渲染，`simplex-noise` 驱动风向/扰动，GUI 实时调参
- 目标是演示效果与教学示例，非生产级复杂地理业务

## Important Constraints
- 避免使用需要密钥的底图或服务，默认仅用 OpenStreetMap 公开瓦片
- 纯前端静态站点，无后端；需保持在常见浏览器中可直接加载运行
- 性能敏感：粒子上限约 50k，禁用多余特效，渲染器使用 `alpha: true`、`powerPreference: "high-performance"`
- 叠加渲染需保持 Three.js Canvas `pointer-events: none`，确保 Cesium 交互不被遮挡

## External Dependencies
- CDN 资源：`https://cesium.com/.../Cesium.js`、`https://cdn.jsdelivr.net/npm/three@0.160.0/...`、`https://cdn.jsdelivr.net/npm/simplex-noise@4.0.1/...`、`three/addons/libs/lil-gui.module.min.js`
- 瓦片服务：`https://a.tile.openstreetmap.org/` 作为默认影像图层
- 托管/部署：GitHub Pages（由 `master` 推送触发），本地预览使用 Python 简单 HTTP 服务
