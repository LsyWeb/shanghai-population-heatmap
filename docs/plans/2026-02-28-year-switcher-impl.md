# Year Switcher Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 在 `index.html` 单文件中添加 2025 年各区人口数据，并在右侧面板新增年份切换控件（2024 / 2025）。

**Architecture:** 新增 `yearData` 常量按年份索引人口数据，新增 `getPop(d)` 辅助函数统一数据访问，新增 `setYear(year)` 函数控制切换并触发全页重渲染。所有原 `d.pop` 访问均改为 `getPop(d)`，使渲染逻辑对年份透明。

**Tech Stack:** 原生 HTML / CSS / JavaScript，零依赖，单文件。无测试框架，验证方式为浏览器手动检查。

---

### Task 1: 添加 yearData 常量与辅助函数

**Files:**
- Modify: `index.html`（JS `// ============ DATA ============` 区块）

**Step 1: 在 `districts` 数组之后，`polygons` 之前，插入 `yearData` 常量**

找到 `index.html:414`（`];` 闭合 districts 数组的那行）之后，插入：

```js
// ============ YEAR DATA ============
const yearData = {
  '2024': {
    pudong:    568.2,  minhang:   272.5,  baoshan:   228.0,
    songjiang: 208.0,  jiading:   198.0,  qingpu:    140.0,
    putuo:     133.0,  yangpu:    132.0,  fengxian:  120.0,
    xuhui:     117.0,  jingan:    108.0,  jinshan:    80.0,
    hongkou:    76.0,  changning:  69.3,  chongming:  70.4,
    huangpu:    50.3,
  },
  '2025': {
    pudong:    578.58, minhang:   272.50, baoshan:   226.39,
    songjiang: 195.89, jiading:   189.04, qingpu:    128.77,
    putuo:     124.87, yangpu:    119.97, fengxian:  113.95,
    xuhui:     109.93, jingan:     92.93, jinshan:    81.23,
    hongkou:    75.75, changning:  68.53, chongming:  63.79,
    huangpu:    50.34,
  },
};

let currentYear = '2024';

function getPop(d) {
  return yearData[currentYear][d.id];
}
```

**Step 2: 在浏览器控制台验证数据可访问**

打开 `index.html`，控制台运行：
```js
yearData['2025']['pudong']  // 期望：578.58
yearData['2024']['huangpu'] // 期望：50.3
getPop({ id: 'minhang' })   // 期望：272.5
```

**Step 3: 提交**

```bash
git add index.html
git commit -m "feat: add yearData constant and getPop() helper for 2024/2025 data"
```

---

### Task 2: 将所有 d.pop 替换为 getPop(d)

**Files:**
- Modify: `index.html`（JS 函数区块）

本任务将 `getDensities`、`getNormalized`、`renderMap`、`renderList`、`showTooltip` 中所有 `d.pop` 改为 `getPop(d)`。

**Step 1: 修改 `getDensities()`**

原代码（`index.html:461-463`）：
```js
function getDensities() {
  return districts.map(d => d.pop * 10000 / d.area);
}
```
改为：
```js
function getDensities() {
  return districts.map(d => getPop(d) * 10000 / d.area);
}
```

**Step 2: 修改 `getNormalized()`**

原代码（`index.html:465-471`）中：
```js
const vals = mode === 'population'
  ? districts.map(d => d.pop)
  : getDensities();
```
改为：
```js
const vals = mode === 'population'
  ? districts.map(d => getPop(d))
  : getDensities();
```

**Step 3: 修改 `renderMap()` 中的 label 值显示**

原代码（`index.html:506-509`）：
```js
const val = mode === 'population'
  ? d.pop.toFixed(1) + '万'
  : Math.round(densities[i]).toLocaleString() + '/km²';
```
改为：
```js
const val = mode === 'population'
  ? getPop(d).toFixed(1) + '万'
  : Math.round(densities[i]).toLocaleString() + '/km²';
```

**Step 4: 修改 `renderList()` 中三处 d.pop 引用**

原代码（`index.html:516-519`）排序逻辑：
```js
return mode === 'population'
  ? b.pop - a.pop
  : (b.pop * 10000 / b.area) - (a.pop * 10000 / a.area);
```
改为：
```js
return mode === 'population'
  ? getPop(b) - getPop(a)
  : (getPop(b) * 10000 / b.area) - (getPop(a) * 10000 / a.area);
```

原代码（`index.html:521-524`）vals 定义：
```js
const vals = mode === 'population'
  ? districts.map(d => d.pop)
  : densities;
```
改为：
```js
const vals = mode === 'population'
  ? districts.map(d => getPop(d))
  : densities;
```

原代码（`index.html:533-535`）val 显示：
```js
const val = mode === 'population'
  ? d.pop.toFixed(1) + ' 万人'
  : Math.round(densities[idx]).toLocaleString() + ' /km²';
```
改为：
```js
const val = mode === 'population'
  ? getPop(d).toFixed(1) + ' 万人'
  : Math.round(densities[idx]).toLocaleString() + ' /km²';
```

原代码（`index.html:536-538`）bar 百分比：
```js
const barPct = mode === 'population'
  ? (d.pop / Math.max(...districts.map(x=>x.pop))) * 100
  : (densities[idx] / Math.max(...densities)) * 100;
```
改为：
```js
const barPct = mode === 'population'
  ? (getPop(d) / Math.max(...districts.map(x => getPop(x)))) * 100
  : (densities[idx] / Math.max(...densities)) * 100;
```

**Step 5: 修改 `renderList()` 中 TOP 3 的 val**

原代码（`index.html:562-565`）：
```js
const val = mode === 'population'
  ? d.pop.toFixed(1) + ' 万'
  : Math.round(densities[idx]).toLocaleString() + '/km²';
```
改为：
```js
const val = mode === 'population'
  ? getPop(d).toFixed(1) + ' 万'
  : Math.round(densities[idx]).toLocaleString() + '/km²';
```

**Step 6: 修改 `showTooltip()` 中的人口排名与显示**

原代码（`index.html:592`）：
```js
const rank = [...districts].sort((a,b) => b.pop-a.pop).findIndex(x=>x.id===d.id)+1;
```
改为：
```js
const rank = [...districts].sort((a,b) => getPop(b)-getPop(a)).findIndex(x=>x.id===d.id)+1;
```

原代码（`index.html:595`）：
```js
<div class="tooltip-row"><span>常住人口</span><span class="tooltip-val">${d.pop.toFixed(1)} 万人</span></div>
```
改为：
```js
<div class="tooltip-row"><span>常住人口</span><span class="tooltip-val">${getPop(d).toFixed(1)} 万人</span></div>
```

**Step 7: 浏览器验证**

打开 `index.html`，检查：
- 地图颜色正常渲染（与替换前无视觉变化，因 `currentYear = '2024'`）
- 鼠标悬停 Tooltip 显示正确人口数字
- 左侧列表排名正确

**Step 8: 提交**

```bash
git add index.html
git commit -m "refactor: replace d.pop with getPop(d) for year-aware data access"
```

---

### Task 3: 使右侧统计数字动态化

**Files:**
- Modify: `index.html`（HTML 右侧面板 + JS）

当前右侧面板的总人口 `2480`、平均密度 `3912` 是硬编码。改为从数据计算，并在年份切换时更新。

**Step 1: 修改右侧面板 HTML，为统计数字加 id**

原代码（`index.html:367-378`）：
```html
<div class="stat-block">
  <div class="stat-num">2480</div>
  <div class="stat-label">全市常住人口（万人）</div>
</div>
<div class="stat-block">
  <div class="stat-num">3912</div>
  <div class="stat-label">平均人口密度（人/km²）</div>
</div>
```
改为：
```html
<div class="stat-block">
  <div class="stat-num" id="statTotalPop">2480</div>
  <div class="stat-label">全市常住人口（万人）</div>
</div>
<div class="stat-block">
  <div class="stat-num" id="statAvgDensity">3912</div>
  <div class="stat-label">平均人口密度（人/km²）</div>
</div>
```

**Step 2: 新增 `updateStats()` 函数**

在 JS 中（`setMode` 函数之前）添加：

```js
function updateStats() {
  const totalPop = districts.reduce((sum, d) => sum + getPop(d), 0);
  const totalArea = districts.reduce((sum, d) => sum + d.area, 0);
  const avgDensity = totalPop * 10000 / totalArea;
  document.getElementById('statTotalPop').textContent = Math.round(totalPop);
  document.getElementById('statAvgDensity').textContent = Math.round(avgDensity).toLocaleString();
}
```

**Step 3: 在初始化时调用 updateStats()**

找到文件末尾的初始化代码：
```js
// Init
renderMap('population');
renderList('population');
```
改为：
```js
// Init
renderMap('population');
renderList('population');
updateStats();
```

**Step 4: 浏览器验证**

打开页面，确认右侧总人口和平均密度数字与 2024 年数据一致（约 2480 万，3912 人/km²）。

**Step 5: 提交**

```bash
git add index.html
git commit -m "feat: make stats panel dynamic using getPop() aggregation"
```

---

### Task 4: 添加年份切换 UI 与 setYear() 函数

**Files:**
- Modify: `index.html`（HTML 右侧面板 + JS）

**Step 1: 在右侧面板「显示模式」section 前添加年份切换 HTML**

原代码（`index.html:348-354`）：
```html
<div>
  <div class="panel-title">显示模式</div>
  <div class="view-toggle">
    <button class="toggle-btn active" onclick="setMode('population')">常住人口</button>
    <button class="toggle-btn" onclick="setMode('density')">人口密度</button>
  </div>
</div>
```
改为（在其前插入年份切换块）：
```html
<div>
  <div class="panel-title">年份</div>
  <div class="view-toggle" id="yearToggle">
    <button class="toggle-btn active" onclick="setYear('2024')">2024</button>
    <button class="toggle-btn" onclick="setYear('2025')">2025</button>
  </div>
</div>

<div>
  <div class="panel-title">显示模式</div>
  <div class="view-toggle">
    <button class="toggle-btn active" onclick="setMode('population')">常住人口</button>
    <button class="toggle-btn" onclick="setMode('density')">人口密度</button>
  </div>
</div>
```

**Step 2: 添加 `setYear()` 函数**

在 JS 中 `setMode` 函数之后添加：

```js
function setYear(year) {
  currentYear = year;
  // 更新年份按钮状态
  document.querySelectorAll('#yearToggle .toggle-btn').forEach(b => b.classList.remove('active'));
  event.target.classList.add('active');
  // 更新 Header 文本
  document.querySelector('.subtitle').textContent =
    `SHANGHAI POPULATION RESIDENTIAL DENSITY MAP · ${year}`;
  document.querySelector('.data-badge').textContent =
    `数据来源：上海统计年鉴 ${year}`;
  // 重渲染
  renderMap(currentMode);
  renderList(currentMode);
  updateStats();
}
```

**Step 3: 浏览器验证**

1. 打开页面，初始状态显示 2024 年数据，Header 标注 `· 2024`
2. 点击「2025」按钮：
   - 地图颜色随浦东等区人口增减重新着色
   - 左侧列表排名更新（浦东 578.58 万，松江从 208 降到 195.89）
   - 右侧总人口、平均密度数字更新
   - Header 变为 `· 2025`，角标变为 `数据来源：上海统计年鉴 2025`
3. 再点「2024」回到原始状态
4. 同时验证：切换年份后再切模式（常住人口 ↔ 人口密度），功能正常

**Step 4: 提交**

```bash
git add index.html
git commit -m "feat: add year switcher UI and setYear() function for 2024/2025 toggle"
```

---

### Task 5: 更新注释说明并推送

**Files:**
- Modify: `index.html`（右侧底部 note 文字）

**Step 1: 更新底部注释文字，反映双年份数据**

原代码（`index.html:387-391`）：
```html
<div class="note">
  * 数据来自2024年上海市统计年鉴及各区统计公报<br>
  * 热力颜色深浅反映各区人口/密度相对高低<br>
  * 点击区域或列表查看详细数据
</div>
```
改为：
```html
<div class="note">
  * 2024年数据来自上海统计年鉴2024<br>
  * 2025年数据来自各区2024年统计公报<br>
  * 热力颜色深浅反映各区人口/密度相对高低<br>
  * 点击区域或列表查看详细数据
</div>
```

**Step 2: 最终完整验证**

浏览器打开 `index.html`，按顺序测试：
- [ ] 2024 + 常住人口：浦东 568.2万，黄浦 50.3万
- [ ] 2024 + 人口密度：黄浦、虹口、静安应为高密度（偏黄）
- [ ] 2025 + 常住人口：浦东 578.58万，松江 195.89万（低于2024的208）
- [ ] 2025 + 人口密度：排名整体与2025常住人口切换后一致
- [ ] Tooltip 在两年份下均正确显示人口数字
- [ ] 年份 / 模式按钮的激活状态（橙色）正确互不干扰

**Step 3: 提交并推送**

```bash
git add index.html
git commit -m "feat: update data source note for dual-year display"
git push origin main
```
