# Project Standardization Design

**Date:** 2026-02-27
**Topic:** GitHub 项目规范化
**Approach:** 方案 B — 标准开源规范

## Overview

为 `shanghai-population-heatmap` 静态可视化项目添加标准开源 GitHub 项目文件，使项目结构规范，便于社区访问和贡献。

## File Structure

```
shanghai-population-heatmap/
├── index.html                          # 主可视化页面（已有）
├── README.md                           # 项目文档（中英双语）
├── LICENSE                             # MIT 开源许可证
├── .gitignore                          # 完整忽略规则
├── CONTRIBUTING.md                     # 贡献指南
└── .github/
    └── ISSUE_TEMPLATE/
        ├── bug_report.md               # Bug 反馈模板
        └── feature_request.md          # 功能建议模板
```

## File Details

### README.md
- 中英双语
- 徽章：Vercel 部署状态、License
- 在线预览链接（Vercel）
- 功能介绍：热力图、人口/密度切换、交互 tooltip
- 数据来源：2024 上海统计年鉴
- 本地运行方式（直接打开 index.html，无需构建）
- 技术栈：纯原生 HTML/CSS/JavaScript，零依赖

### LICENSE
- MIT 许可证
- 作者：LsyWeb

### .gitignore
- 补充 OS 文件（.DS_Store）
- 编辑器文件（.vscode/、.idea/）
- 保留 .vercel 条目

### CONTRIBUTING.md
- 如何提交 Issue
- 如何提交 Pull Request
- 数据更新说明

### .github/ISSUE_TEMPLATE/bug_report.md
- 问题描述
- 复现步骤
- 期望行为
- 浏览器/环境信息

### .github/ISSUE_TEMPLATE/feature_request.md
- 功能描述
- 使用场景
- 参考资料

## Success Criteria

- 访问 GitHub 仓库首页即可看到完整 README
- 用户可通过 Issue 模板规范反馈问题
- 项目可被 fork 并本地运行，无需任何依赖
