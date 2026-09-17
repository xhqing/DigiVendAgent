# Changelog

## [Unreleased]

### 新增（.py/skills 软链接指向 .cloud/skills）

- **为什么改**：用户指令（2026-09-17）——在 `.py/` 下建立指向 `.cloud/skills` 的软链接入口。
- **改了什么**（2026-09-17）：新建目录 `.py/` 与软链接 `.py/skills`（目标为相对路径 `../.cloud/skills`，仓库整体移动不失效）。注：目标 `.cloud/skills` 当前尚不存在，链接暂为悬空状态，待目标目录创建后自动生效。

### 变更（CLAUDE.md 删去「经验提炼」整节）

- **为什么改**：Claude 的 Auto Memory 功能已关闭，`.claude/memory/` 不复存在，「把 Auto Memory 的教训固化为项目规则」的整套工作流（识别 → 提炼 → 留针）失去前提；与 SiteBuilderAgent 同步清理（2026-09-13 全量扫描后仅本项目残留此节）。
- **改了什么**（2026-09-13）：`CLAUDE.md` 删除「## 经验提炼：把 Auto Memory 的教训固化为项目规则」整节（含触发条件、为什么、怎么做、不必提炼的情况四个小节）。溯源核查：该节为早期同步的模板内容，不承担任何修复逻辑，删除无回归风险。

### 变更（CLAUDE.md 删去「由 Claude Code 自动加载」说明句）

- **为什么改**：用户 2026-09-12 要求 CLAUDE.md 不再强调本文由 Claude Code 加载，团队全部项目的 CLAUDE.md 统一清理此类语句。
- **改了什么**（2026-09-12）：`CLAUDE.md` 开头角色定位行删去句尾「本文件由 Claude Code 在每次会话开头自动加载。」，角色描述本身保留。

### 变更（find-skill 相关内容清理）

- **为什么改**：全局 find-skill skill 已被用户删除（实际使用中从未用到），项目内「find-skill skill 同步」专节与相关提及全部失效，2026-09-12 联动清理。
- **改了什么**：`CLAUDE.md`：①「像 anysearch、find-skill 这类通用 skill」→「像 anysearch 这类通用 skill」；②删除「## find-skill skill 同步（全局为权威副本）」整节（项目内 `.claude/skills/find-skill/` 副本此前已随全局删除，无目录残留）；③判断标准句「按下方规则双向同步」改「按上方规则双向同步」（原指向被删的 find-skill 节，现指上方的 anysearch 同步节）。

### 变更（assets/logo.svg 副标题去中文）

- **为什么改**：全局规则新增「Logo / 图标资产文字一律用英文」（2026-09-12 用户立，起因 Swing 仓库 logo 副标题混入中文被指出）：logo 是面向全球读者的视觉标识，中文受众已有 README_cn.md 双语通道；且 SVG 中文依赖查看环境的字体回退，渲染不可控。本次为按新规批量清理存量。
- **改了什么**：`assets/logo.svg` 副标题「Sales Ops · 电商运营」→「E-commerce Sales Ops」（与注册表「电商运营」对齐）。

### 变更（措辞统一 fleet → team / 舰队 → 团队 + 流水线数字修正：README 中英双语 + vend skill）

- **为什么改**：用户 2026-08-16 已把 xhqing 主页 README 的自称从「舰队 / fleet」改为「团队 / team」，但本仓 README 中英两版与 vend skill 仍是 fleet 旧措辞；且 README 写的「五智能体流水线 / five-agent pipeline、Vendy 第 ④ 步」是 Mason 立项前的旧口径——现行流水线为六步（Scout → Wright → Mason → Buzz → Vendy → Echo）、Vendy 是第 ⑤ 步。2026-08-21 用户裁定全量存量一次清零，顺手修正数字。
- **改了什么**：`README.md` / `README_cn.md`——fleet / 舰队 → team / 团队（引言、定位表、典型工作流段共 3 处 / 版）；「五智能体 / five-agent」→「六智能体 / six-agent」、Vendy 步骤号 ④ → ⑤、流水线图补 ③ Mason（建阵地）节点、引言括号补「建站 = Mason / build = Mason」；`.claude/skills/vend/SKILL.md`——description「agent of the fleet」→「of the team」、「角色范围（舰队分工）」→「（团队分工）」。职责、徽章、结构均不变。

### 变更（Visitors 徽章更名 Visits/day (14d)：alt 文本与 xhqing 集中统计新 label 对齐）

- **为什么改**：用户要求（2026-08-17）访问量徽章名需表达「最近半月日均访问量」口径——xhqing 集中统计侧的 badge JSON label 已从 `Visitors` 改为 `Visits/day (14d)`（`Visits/day` 是 shields.io 表达日均的惯例写法、`(14d)` 标注 14 天滚动窗口），各仓 README 的徽章 alt 文本同步对齐，避免 alt 与徽章实际显示文字脱节。
- **改了什么**：README 徽章区 `alt="Visitors"` → `alt="Visits/day (14d)"`，仅改 alt 文本，endpoint URL、数据源、徽章口径均不变（口径改动记 xhqing 仓库 CHANGELOG，本仓只改 alt）。

### 变更（Visitors 徽章 alt 文本首字母大写：README 访问量徽章命名统一）

- **为什么改**：用户指令（2026-08-16）「Visitors 徽章全局统一，首字母大写」——配合全局 `~/.claude/CLAUDE.md`「徽章英文首字母必须大写」新规，集中统计上线时挂的访问量徽章 `alt="visitors"` 为小写存量，与 badge JSON label（`Visits/day`）及大写规范不一致，本次一次收口。
- **改了什么**：README（EN/CN）徽章区 visitors 徽章 `alt="visitors"` → `alt="Visitors"`，仅改 alt 显示文本，endpoint URL 与数据源不变。

### 新增（README 访问量徽章——舰队集中式访问统计）

- **为什么改**：全舰队上线集中式「真去重」访问统计（图片徽章方案无法去重，走官方 Traffic API 路线）：统计集中部署在 xhqing 仓库（`scripts/update_traffic.py` + 每日 GitHub Action），各 fleet 仓库只需在 README 挂徽章、零运行负担。
- **改了什么**：README（EN/CN）徽章区新增 visitors 徽章（shields.io endpoint 指向 `xhqing/xhqing` 仓库 `traffic/badges/<repo>.json`，由每日采集的官方 Traffic API 数据更新）。徽章数字含义：按日去重访客的累计（GitHub 只提供每日 uniques，跨天不去重），自 2026-08-16 起累计。
