# Project Standardization Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 为上海人口热力图项目添加标准开源 GitHub 项目文件，使其符合方案 B（标准开源规范）。

**Architecture:** 纯静态 HTML 项目，无构建步骤，无依赖。所有新增文件均为文档/配置类，直接写入仓库根目录和 .github/ 目录，完成后统一推送至 GitHub remote。

**Tech Stack:** Markdown, Git, GitHub Issue Templates

---

### Task 1: 完善 .gitignore

**Files:**
- Modify: `/tmp/shanghai-population-heatmap/.gitignore`

**Step 1: 写入完整 .gitignore 内容**

```
# Vercel
.vercel

# macOS
.DS_Store
.AppleDouble
.LSOverride

# Editor
.vscode/
.idea/
*.swp
*.swo

# Logs
*.log
```

**Step 2: 验证文件内容**

```bash
cat /tmp/shanghai-population-heatmap/.gitignore
```
Expected: 显示上述完整内容

**Step 3: Commit**

```bash
cd /tmp/shanghai-population-heatmap
git add .gitignore
git commit -m "chore: add complete .gitignore"
```

---

### Task 2: 添加 MIT LICENSE

**Files:**
- Create: `/tmp/shanghai-population-heatmap/LICENSE`

**Step 1: 写入 MIT License（作者 LsyWeb，2024 年）**

```
MIT License

Copyright (c) 2024 LsyWeb

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

**Step 2: Commit**

```bash
cd /tmp/shanghai-population-heatmap
git add LICENSE
git commit -m "chore: add MIT license"
```

---

### Task 3: 创建 README.md（中英双语）

**Files:**
- Create: `/tmp/shanghai-population-heatmap/README.md`

**Step 1: 写入 README.md**

内容结构：
```markdown
# 上海市人口居住热力分布图
# Shanghai Population Residential Density Map

[![Deploy with Vercel](https://vercel.com/button)](https://shanghai-population-heatmap.vercel.app)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

## 在线预览 | Live Demo

🔗 https://shanghai-population-heatmap.vercel.app

## 项目简介 | About

交互式上海各区人口居住热力分布可视化，数据来源为 2024 年上海统计年鉴。

An interactive heatmap visualization of Shanghai's population residential density by district, based on the 2024 Shanghai Statistical Yearbook.

## 功能特性 | Features

- 热力色阶地图：深紫 → 明黄，直观反映人口/密度高低
- 双模式切换：常住人口（万人）/ 人口密度（人/km²）
- 交互 Tooltip：悬停显示各区详细数据
- 左侧排名列表：点击高亮对应区域
- 右侧统计面板：TOP 3 排名、全市汇总数据

## 数据来源 | Data Source

上海市统计年鉴 2024 / 各区统计公报
Shanghai Statistical Yearbook 2024

## 本地运行 | Run Locally

无需安装任何依赖，直接用浏览器打开：

No installation required. Simply open in browser:

```bash
open index.html   # macOS
# 或双击 index.html 文件
```

## 技术栈 | Tech Stack

- 纯原生 HTML / CSS / JavaScript
- 零外部依赖（Zero dependencies）
- SVG 地图绘制
- Google Fonts（Noto Serif SC、JetBrains Mono）

## 贡献 | Contributing

欢迎贡献！请参阅 [CONTRIBUTING.md](./CONTRIBUTING.md)。

Contributions are welcome! Please read [CONTRIBUTING.md](./CONTRIBUTING.md).

## 许可证 | License

[MIT](./LICENSE) © LsyWeb
```

**Step 2: 验证文件存在**

```bash
ls -la /tmp/shanghai-population-heatmap/README.md
```
Expected: 显示文件大小 > 0

**Step 3: Commit**

```bash
cd /tmp/shanghai-population-heatmap
git add README.md
git commit -m "docs: add bilingual README with demo link and feature overview"
```

---

### Task 4: 创建 CONTRIBUTING.md

**Files:**
- Create: `/tmp/shanghai-population-heatmap/CONTRIBUTING.md`

**Step 1: 写入 CONTRIBUTING.md**

```markdown
# 贡献指南 | Contributing Guide

感谢你对本项目的关注！以下是参与贡献的方式。

## 提交 Issue

请使用 GitHub Issue 反馈问题或建议：

- **Bug 反馈**：使用 Bug Report 模板，尽量提供复现步骤和浏览器信息
- **功能建议**：使用 Feature Request 模板，描述使用场景

## 提交 Pull Request

1. Fork 本仓库
2. 基于 `main` 分支创建新分支：`git checkout -b feature/your-feature`
3. 修改代码并测试（直接在浏览器中打开 `index.html` 验证）
4. 提交代码：`git commit -m "feat: describe your change"`
5. 推送分支：`git push origin feature/your-feature`
6. 在 GitHub 上发起 Pull Request，描述你的改动

## 数据更新

如需更新各区人口数据，修改 `index.html` 中 `districts` 数组的对应字段：

```js
{ id: 'pudong', name: '浦东新区', pop: 568.2, area: 1210, ... }
// pop: 常住人口（万人）
// area: 辖区面积（km²）
```

数据请以上海市统计局发布的官方数据为准。

## 代码风格

- 本项目为单文件纯原生实现，保持零依赖原则
- CSS 变量集中定义在 `:root` 中，新增颜色/尺寸请复用已有变量
- JS 函数命名使用 camelCase
```

**Step 2: Commit**

```bash
cd /tmp/shanghai-population-heatmap
git add CONTRIBUTING.md
git commit -m "docs: add contributing guide"
```

---

### Task 5: 创建 GitHub Issue 模板

**Files:**
- Create: `/tmp/shanghai-population-heatmap/.github/ISSUE_TEMPLATE/bug_report.md`
- Create: `/tmp/shanghai-population-heatmap/.github/ISSUE_TEMPLATE/feature_request.md`

**Step 1: 创建目录**

```bash
mkdir -p /tmp/shanghai-population-heatmap/.github/ISSUE_TEMPLATE
```

**Step 2: 写入 bug_report.md**

```markdown
---
name: Bug 反馈 | Bug Report
about: 报告一个问题帮助我们改进 | Report a bug to help us improve
title: '[BUG] '
labels: bug
assignees: ''
---

## 问题描述 | Describe the Bug

<!-- 清晰简洁地描述问题 | A clear and concise description of the bug -->

## 复现步骤 | Steps to Reproduce

1. 打开页面 '...'
2. 点击 '...'
3. 看到错误 '...'

## 期望行为 | Expected Behavior

<!-- 描述你期望发生的事情 -->

## 截图 | Screenshots

<!-- 如有截图请粘贴在此 -->

## 环境信息 | Environment

- 浏览器 | Browser: [e.g. Chrome 120, Safari 17]
- 操作系统 | OS: [e.g. macOS 14, Windows 11]
- 设备 | Device: [e.g. Desktop, iPhone 15]
```

**Step 3: 写入 feature_request.md**

```markdown
---
name: 功能建议 | Feature Request
about: 为本项目建议新功能 | Suggest an idea for this project
title: '[FEAT] '
labels: enhancement
assignees: ''
---

## 功能描述 | Feature Description

<!-- 清晰描述你希望增加的功能 -->

## 使用场景 | Use Case

<!-- 这个功能解决什么问题？在什么场景下会用到？ -->

## 参考资料 | References

<!-- 相关截图、链接或参考项目（可选） -->
```

**Step 4: Commit**

```bash
cd /tmp/shanghai-population-heatmap
git add .github/
git commit -m "chore: add GitHub issue templates (bug report & feature request)"
```

---

### Task 6: 推送所有变更到 GitHub

**Step 1: 确认本地 commits 状态**

```bash
cd /tmp/shanghai-population-heatmap
git log --oneline
```
Expected: 显示 5+ 条新 commit（task 1-5 的提交）

**Step 2: 推送到 remote**

```bash
git push origin main
```
Expected: 显示各文件上传成功

**Step 3: 验证 GitHub 仓库**

```bash
~/bin/gh repo view LsyWeb/shanghai-population-heatmap
```
Expected: 显示 README 内容和仓库信息
