# CLAUDE.md

本文件为 Claude Code（claude.ai/code）在此仓库中工作时提供指引。

## 项目概述

**零依赖、单文件**的上海各区人口居住热力分布交互可视化，数据基于上海统计年鉴，部署于 Vercel 静态托管。

在线演示：https://shanghai-population-heatmap.vercel.app

## 本地运行

无需构建，无需包管理器，无任何依赖：

```bash
# macOS
open index.html

# 或用任意静态文件服务器
python3 -m http.server 8080
```

## 架构

所有代码均位于 `index.html` 这一个自包含文件中，分为三个部分：

1. **CSS**（`:root` 变量 → 组件样式）—— 深色主题，7 档热力色阶（`--heat1` 至 `--heat7`），三列网格布局（左面板 260px | 地图 | 右面板 220px）

2. **HTML** —— 三面板布局：
   - 左侧：`#districtList` —— 区排名列表（由 JS 渲染）
   - 中间：`#map` SVG（600×580 viewBox）—— 多边形地图（由 JS 渲染）
   - 右侧：年份切换、显示模式切换、图例、汇总统计、TOP 3 列表

3. **JavaScript** —— 核心数据结构与函数：
   - `districts[]` —— 16 个区的对象数组，包含 `{ id, name, area (km²), cx, cy }`，用于标签定位
   - `yearData{}` —— 按年份 → 区 ID 双重索引的常住人口数据（万人），当前包含 `'2023'`（2023年末）和 `'2024'`（2024年末）两组
   - `currentYear` —— 当前选中年份状态变量，初始为 `'2023'`
   - `getPop(d)` —— 统一的年份感知人口数据访问函数，返回 `yearData[currentYear][d.id]`
   - `polygons{}` —— 各区 SVG 多边形点字符串（以 `id` 为键），坐标对应 600×580 ViewBox
   - `getHeatColor(normalized)` —— 在 7 个色阶点（深紫 → 亮黄）间插值
   - `getNormalized(mode)` —— 对当前模式的数值做 min-max 归一化
   - `renderMap(mode)` / `renderList(mode)` —— 切换模式时全量重渲染
   - `highlightDistrict(id)` —— 同步地图点击与列表点击的高亮状态
   - `setMode(mode, e)` —— 切换 `'population'`（常住人口）与 `'density'`（人口密度）视图
   - `setYear(year, e)` —— 切换年份，更新 `currentYear`、Header 文本、数据来源角标，触发全量重渲染

## 约定规范

- **零依赖**：不使用 npm、打包工具或外部 JS 库，Google Fonts 是唯一外部资源。
- **CSS 变量**：所有颜色和主题值在 `:root` 中定义，复用现有变量，新增变量也在此处添加。
- **JS 命名**：函数和变量使用 camelCase。
- **数据更新**：更新各区人口数据时，在 `index.html` 的 `yearData` 对象中编辑对应年份数据，使用上海市统计局官方数据。
  ```js
  yearData['2025'] = {
    pudong: 578.58, minhang: 272.5, ...
    // 单位：常住人口（万人），数据时点：年末
  }
  ```
- **地图几何**：各区形状在 `polygons{}` 中以 SVG 多边形点字符串存储，坐标为 600×580 ViewBox 空间。

## 提交规范

使用约定式提交格式，**描述信息必须使用中文**：

```
feat: 添加2025年末各区人口数据
fix: 修正虹口区人口密度计算错误
refactor: 将d.pop替换为getPop(d)统一数据访问
docs: 更新README数据来源说明
chore: 更新.gitignore
```

类型前缀保持英文（`feat:`、`fix:`、`docs:`、`chore:`、`refactor:`），冒号后的描述使用中文。
