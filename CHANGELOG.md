# Changelog

### 新增（CI 门禁：validate 检查 + main 分支 strict 保护）

- **为什么改**：用户 2026-11-03 立规「新建仓库一律必须 strict 模式的 PR CI」，并要求 28 个不合规的存量仓库统一执行最新标准。本项目此前在 PR 上没有门禁——改动合进 main 全靠人工把关，PR 可以拿旧 main 上通过的检查直接合并，回归防护存在缺口。本次补齐团队要求的「三件套」：PR 上真实跑的 CI 检查 + 挂成 required status check + `required_status_checks.strict = true`。
- **改了什么**（2026-11-03）：新增 `.github/workflows/validate.yml`（PR 与 main push 触发，检查名 `validate`）与 `.github/scripts/validate.py`（仓库卫生检查：必备文件存在、敏感文件未被跟踪、被跟踪文件不超过 10 MB、README 相对链接有效、Python / Shell 语法检查）；main 分支保护补挂 required check `validate` 并设 `strict=true`。
- **边界**：检查只做确定性、低误报的项目（required check 一旦误报就会卡住正常 PR，所以宁可少查）；不改动仓库任何原有内容。

## [Unreleased]

### 变更（项目规则文件本体统一改为 `AGENTS.md`，`CLAUDE.md` 弃用，2026-11-05）

- **为什么改**：用户 2026-11-05 立规「所有注册表项目与全局的规则文件本体统一用 `AGENTS.md`，`CLAUDE.md` 弃用」。pi 的上下文文件候选顺序是 `AGENTS.override.md` > `AGENTS.md` > `CLAUDE.md`（同一目录只加载第一个存在的），本体放在 `AGENTS.md` 后 `CLAUDE.md` 根本不参与加载——两份并存只会形成双份内容、随时漂移。
- **改了什么**：项目根 `CLAUDE.md` 改名为 `AGENTS.md`（实体本体，原先的 `AGENTS.md → CLAUDE.md` 软链接已删除）；项目内引用「项目 `CLAUDE.md` / 全局 `CLAUDE.md`」的条文、skill、hook、脚本与 README 同步改为 `AGENTS.md`。`CODEBUDDY.md` 软链接改指向 `AGENTS.md`（原先指向已改名的 `CLAUDE.md`，会悬空）。
- **回归检查**：加载顺序不受影响（`AGENTS.md` 本就排在 `CLAUDE.md` 之前）；子项目根目录的 `AGENTS.md` 软链接仍指向本项目 `AGENTS.md`，目标不变、照常解析。

### 变更（移除访问量徽章，配合停用集中式流量采集）

- **为什么改**：用户 2026-11-04 决定停用 xhqing 仓库的集中式访问量采集，同时不再在各仓库 README 展示访问量徽章。徽章的数据源必须由采集任务每天刷新，采集停掉之后徽章会冻结在旧值，而徽章文案写的是「Visits/day (14d)」——数字不再更新却仍显示成实时数据，留着反而误导读者。
- **改了什么**：`README.md` / `README_cn.md` 顶部徽章区删除那一枚 shields.io 访问量徽章（整行删除，数据源 `https://raw.githubusercontent.com/xhqing/xhqing/main/traffic/badges/<仓库名>.json`）；License / Version / Type 等其它徽章与正文一个字未动。
- **回归检查**：只删这一整行，徽章区所在的 `<p align="center">` / `<div>` 结构与相邻徽章不受影响；README 其余内容零改动。

### 新增（AGENTS.md 软链接入口——智能体专属文件新规统一落位）

- **为什么改**：用户 2026-10-17 修订智能体专属文件归属规则（细则见全局 `~/.claude/docs/agents-registry.md` 2026-10-17 修订节）：实体副本只属于 Agent 项目，子项目改挂 `AGENTS.md` / `.pi` 软链接指回负责 Agent；全队 agent 项目统一补齐 `AGENTS.md` 入口（本仓此前只有 `CLAUDE.md`）。
- **改了什么**（2026-10-17）：新建软链接 `AGENTS.md → CLAUDE.md`（相对路径、随仓库提交；内容唯一源仍是 `CLAUDE.md`，`AGENTS.md` 仅作别名入口）。本仓无子项目，不涉及子项目侧链接；`.claude/` 按新规不纳入链接网络、保持原状。

## [1.0.0] - 2026-10-12

### 新增（接收 Mason→Vendy 阵地运营交接：成交阵地移交）

- **为什么改**：建设期（Mason）完成 Payloadz 上架 + 落地页 + GA4 埋点，流水线 ③→⑤，运营期开工前需正式接收阵地资产与运营入口。
- **改了什么**（2026-09-18）：新建 `docs/handoff.md`（被 gitignore 忽略）：阵地资产与后台入口清单、定价执行项（早鸟双开关机制——Payloadz 限时直降价 + 落地页 earlyBird 配置，两处同步且与 Buzz 首发对齐）、成交补录工具用法（当日升级：批量工具 `ga4-purchase-batch.py` 为主——对账一条命令自动抓订单、去重、按实际价格补录，单笔工具为辅）、运营 SOP（订单监控 / 售后补发 / 三方对账 / 首周后收尾）、平台限制备忘。
- **边界**：仅新增交接输入；未动本仓既有文件。

### 新增（.pi/skills 软链接指向 .cloud/skills）

- **为什么改**：用户指令（2026-09-17）——在 `.pi/` 下建立指向 `.cloud/skills` 的软链接入口。
- **改了什么**（2026-09-17）：新建目录 `.pi/` 与软链接 `.pi/skills`（目标为相对路径 `../.cloud/skills`，仓库整体移动不失效）。注：目标 `.cloud/skills` 当前尚不存在，链接暂为悬空状态，待目标目录创建后自动生效。（后续：该链接其后已改指 `.claude/skills`，见下一条变更。）

### 变更（.pi/skills 软链接改指 .claude/skills）

- **为什么改**：上一条新建的软链接目标 `../.cloud/skills` 并不存在、链接悬空；实际技能源在 `.claude/skills/`，同日改指真实目标（提交 `7a0fe6d`；当时未单独记录，2026-10-12 发布前核对 CHANGELOG 账目时补记）。
- **改了什么**（2026-09-17）：`.pi/skills` 软链接目标 `../.cloud/skills` → `../.claude/skills`。

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
