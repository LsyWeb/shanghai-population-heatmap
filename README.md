# 上海市人口居住热力分布图

# Shanghai Population Residential Density Map

[![Vercel](https://img.shields.io/badge/Vercel-Deployed-black?logo=vercel)](https://shanghai-population-heatmap.vercel.app)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

## 在线预览 | Live Demo

🔗 **https://shanghai-population-heatmap.vercel.app**

## 项目简介 | About

交互式上海各区人口居住热力分布可视化，支持 2023 年末与 2024 年末两组常住人口数据对比。可按常住人口和人口密度两种维度切换查看，点击区域或左侧列表可高亮对应区域。

An interactive heatmap visualization of Shanghai's population residential density by district. Supports comparing 2023 year-end and 2024 year-end resident population data, with switching between total population and population density views.

## 功能特性 | Features

- 🗺️ **热力色阶地图**：深紫 → 明黄，直观反映人口 / 密度高低
- 📅 **双年份切换**：2023 年末 / 2024 年末数据对比
- 🔄 **双模式切换**：常住人口（万人）/ 人口密度（人/km²）
- 💬 **交互 Tooltip**：悬停显示各区详细数据
- 📋 **左侧排名列表**：点击高亮对应区域
- 📊 **右侧统计面板**：TOP 3 排名、全市汇总数据

## 数据来源 | Data Source

| 年份 | 数据来源 |
|------|---------|
| 2023 年末 | 上海市统计年鉴 2024 |
| 2024 年末 | 各区 2024 年国民经济和社会发展统计公报 |

## 本地运行 | Run Locally

无需安装任何依赖，直接用浏览器打开：

No installation required. Simply open in your browser:

```bash
# macOS
open index.html

# 或直接双击 index.html 文件
# Or double-click index.html
```

## 技术栈 | Tech Stack

| 技术 | 说明 |
|------|------|
| HTML / CSS / JavaScript | 纯原生实现，零依赖 |
| SVG | 上海行政区地图绘制 |
| Google Fonts | Noto Serif SC、JetBrains Mono |
| Vercel | 静态托管与部署 |

## 贡献 | Contributing

欢迎提交 Issue 或 Pull Request！请参阅 [CONTRIBUTING.md](./CONTRIBUTING.md) 了解详情。

Contributions are welcome! Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

## 许可证 | License

[MIT](./LICENSE) © 2024 LsyWeb
