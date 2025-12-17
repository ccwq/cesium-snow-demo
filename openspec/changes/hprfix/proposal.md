# Change: HPR 地球对齐与 GUI 同步

## Why
Cesium 场景使用球体地面，但当前 HPR 与雪花下落方向按固定直角坐标假设处理，南北半球或不同经纬度下会出现“下落”反转、雪花向上漂的问题；同时 lil-gui 未展示 Cesium 相机的实时 HPR/XYZ，调试困难。

## What Changes
- 基于 Cesium 椭球法线构建局部 East-North-Up 坐标，重映射 Three 相机 HPR 与雪花重力/风场方向，确保无论南北半球都朝向地面。
- 将 Cesium 相机的 heading/pitch/roll 与局部位置实时同步到 lil-gui，并保持 GUI 改动可回写到 Three 相机以便交互调试。
- 为极区与快速飞行场景补充稳定性处理与验证要点，避免姿态跳变或方向翻转。

## Impact
- Affected specs: align-hpr
- Affected code: cesium-hellow.html（相机同步、雪花风场方向、GUI 参数同步）
