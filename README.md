# Dashboard UI Skill

一个 Claude Code skill — 让 Claude 基于 **Asana 配色** + **AscendOps 风格** 一键生成同款单页数据看板 HTML，或把现有页面刷新成同款。

> ⚡ 风格预览：[examples/ascendops-experience.html](./examples/ascendops-experience.html)（紫青卡片渐变） / [examples/ascendops-experience-warm.html](./examples/ascendops-experience-warm.html)（暖色斜向渐变）

---

## 它能做什么

输入 | 输出
---|---
"用 dashboard-gen 给我生成一个销售看板" | 完整单文件 HTML 看板，含 Hero + Pipeline + 业务卡 + 图表 + 列表
"用 dashboard-gen 把 xxx.html 刷新成同款风格" | 替换 :root token、批量迁移硬编码 hex、清理反模式
"用 dashboard-gen 但 banner 用 Variant B" | 使用整页斜向渐变方案（参考 The Software House）

---

## 安装

把这个仓库 clone 到 Claude Code 的 skills 目录，命名为 `dashboard-gen`：

```bash
cd ~/.claude/skills
git clone https://github.com/yinyucheng0601/dashboard_UI_skill.git dashboard-gen
```

或者只想用一次、不想常驻：clone 到任意位置，把 `SKILL.md` 路径直接发给 Claude。

> Claude Code 启动后，skill 会自动被发现。可以在终端里输入 `/` 查看可用 skill 列表里有没有 `dashboard-gen`。

---

## 使用

进入 Claude Code 会话后：

```
/dashboard-gen
我要做一个销售看板，数据在 ~/sales-data.json，
要展示 Q2 业绩 / 地区分布 / Top10 客户 / 增长趋势。
```

或自然语言触发（关键词命中 SKILL.md 的"触发条件"）：

```
请给我生成一个 dashboard，看团队周报数据
```

Claude 会：
1. 摸清你的需求（业务主题 / 数据来源 / 楼层选择）
2. 给出 ASCII 布局方案让你确认
3. 生成单文件 HTML（继承 Asana token + AscendOps 蓝本）
4. `open` 浏览器让你看效果

---

## 包含什么

```
dashboard_UI_skill/
├── SKILL.md                          # Skill 主文件（Claude 读这个）
├── dashboard-ui-design.md            # 完整设计规范（tokens / 组件 / 反模式）
├── examples/
│   ├── ascendops-experience.html     # Variant A: 紫青卡片 Hero
│   └── ascendops-experience-warm.html# Variant B: 暖色整页斜向渐变
└── README.md
```

| 文件 | 干什么 |
|---|---|
| `SKILL.md` | Agent 指令：触发条件、工作流程、色彩迁移 sed 表、硬规则、楼层模板、自检清单 |
| `dashboard-ui-design.md` | 设计系统 v1：完整 `:root` token、字体阶梯、Spacing/Radius/Grid、15+ 组件 spec、可视化规范、反模式 |
| `examples/*.html` | 两套真实蓝本，可直接复制改业务内容 |

---

## 两种 Banner 风格（开箱可选）

### Variant A · 卡片式 Hero（默认）
紫青渐变独立卡 + 装饰圆 + 大字号标题。适合**严肃数据看板**、内容紧凑、需要明确分区。

![variant-a](./examples/ascendops-experience.html)

### Variant B · 整页斜向渐变（暖色，参考 The Software House）
暖橙→紫→蓝 6 段斜向渐变 + 平行四边形条带 overlay，Hero 去外壳。适合**偏品牌/营销感**看板、希望页面有"氛围"。

![variant-b](./examples/ascendops-experience-warm.html)

切换方式：生成时跟 Claude 说 "用 Variant A" 或 "用 Variant B"。SKILL.md 里有完整 CSS。

---

## 设计原则（硬规则）

- **单文件 HTML**，无外部 JS 库（chart.js / d3 / echarts 都不要）
- **只用 Asana token**（5 色族 × 5 层 + 10 灰阶 + Navy），禁用 Tailwind 默认色
- **卡片不加重阴影**，靠 1px border + bg 分隔
- **不做浏览器 chrome 装饰**（traffic light、URL bar）
- **业务大数字 48–54px / weight 500**（不要 800/900）
- **业务对象色族 ≤ 4 个**
- **字体** Inter + JetBrains Mono（数字 mono）
- **顶栏全宽**，无侧边导航（除非模块 > 6 个）

完整规范见 [`dashboard-ui-design.md`](./dashboard-ui-design.md)。

---

## 业务对象 ↔ 色族 绑定模板

| s | 色族 | bg (Light) | fg (Dark) | 典型语义 |
|---|------|-----------|-----------|---------|
| 1 | Green  | `#EBFCF7` | `#37C597` | 健康 / 已完成 / 成功类对象 |
| 2 | Gold   | `#FFF8E5` | `#FD9A00` | 中性 / 进行中 / 业务流类对象 |
| 3 | Coral  | `#FEEFF0` | `#E63838` | 警示 / 高优先级 / 风险类对象 |
| 4 | Purple | `#F0EFFA` | `#4F4DA7` | 创新 / 探索类 / 品牌核心对象 |

---

## 我没用 Claude Code，能用吗？

可以。`SKILL.md` 本身是个详细的 prompt + 实现指南，把它丢给任何 LLM（Claude API / ChatGPT / Cursor / Cline）+ `dashboard-ui-design.md` 都能生成同款看板。两个 example HTML 也可以直接打开当 starter template。

---

## License

MIT
