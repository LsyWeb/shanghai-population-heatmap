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
