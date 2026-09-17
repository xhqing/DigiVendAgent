<div align="center">

<img src="assets/logo.svg" width="640" alt="Vendy logo" />

![License: MIT](https://img.shields.io/badge/License-MIT-yellow)
![Version](https://img.shields.io/badge/Version-1.0.0-blue)
![AI Agent](https://img.shields.io/badge/Type-AI%20Agent-FF1493)
<img src="https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/xhqing/xhqing/main/traffic/badges/DigiVendAgent.json" alt="Visits/day (14d)" />

</div>

# DigiVendAgent

> **拟人化名字：Vendy** —— 团队里的成交 / 电商运营智能体。
>
> 「Digi（数字）+ Vend（售卖）+ Agent（智能体）」—— Vendy 是六智能体流水线的第 ⑤ 步：拿到成品数字产品，把它变成钱（上架 → 定价 → 履约 → 收款 → 提现），循环到达成目标。（研判 = Scout，生产 = Wright，建站 = Mason，引流 = Buzz，复盘 = Echo。）

Vendy 的目标是**被动收入里的「成交」环节**。她不接时薪、日薪类外包，也不做课程、代运营、咨询等需要持续投入人力的主动服务——拿到「一次产出、反复销售」的数字产品后，她自动上架、定价、履约、收款、处理售后、提现。

[English](README.md)

<img src="https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/xhqing/xhqing/main/traffic/badges/DigiVendAgent.json" alt="Visits/day (14d)" />

---

## 🎯 定位

| 维度 | 说明 |
|------|------|
| **做什么** | 自主在线售卖数字商品赚钱（团队第 ⑤ 步：成交 / 转化）|
| **不做什么** | 不按时间计费的外包；不做需要持续人力的主动服务 |
| **目标市场** | 全球（英文为主）+ 国内（小红书）|
| **收款** | 提现到 HK / CN 银行（Payloadz + PayPal 主链路）|
| **运行模式** | 持续尝试循环，直到达成目标金额 |

---

## 🧠 核心能力

> 🔎 **热点选品（`/hot-trend`）已迁至 Scout** —— [ProductStrategistAgent](https://github.com/xhqing/ProductStrategistAgent)。Vendy 是团队里的**成交 / 销售智能体**：给她一个产品，她把它变成钱。

### 自动售卖引擎 · `/vend`（成交循环）

Vendy 的「手脚」。读取 `docs/config.json` 的目标后，跑**成交循环**：上架 → 定价 → 履约 → 收款 → 检查指标 → 调整策略。生产成品是 **Wright**（[ProductProducerAgent](https://github.com/xhqing/ProductProducerAgent)）的活，引流是 **Buzz**（[GrowthMarketerAgent](https://github.com/xhqing/GrowthMarketerAgent)）的活。**除非遇到 3 种特殊情况，否则不停下来汇报或询问**，一直跑到达成目标。

- 触发：`/vend`、`帮我搞钱`、`帮我赚钱`、`卖货赚钱`……
- 上架收款：Payloadz → PayPal（海外）；小红书（国内）
- 履约与售后：发货、退款、纠纷
- 3 种必须停下来问用户的特殊情况：需要注册账号 / 获取权限、需要输入密码 / 验证、需要花钱投入

---

## 🔁 典型工作流

Vendy 是六智能体团队的第 **⑤** 步：

```
① Scout（研判）→ ② Wright（生产）→ ③ Mason（建阵地）→ ④ Buzz（引流）→ ⑤ Vendy（成交运营）→ ⑥ Echo（复盘）
```

跑成交循环：

```
/vend   →   上架 → 定价 → 履约 → 收款 → 提现（循环到达成目标）
```

---

## 📦 本仓库包含什么

本开源仓库发布的是 **Agent 的设计与技能**，不含运行时数据。

```
DigiVendAgent/
├── README.md                 ← 英文 README
├── README_cn.md              ← 本文件（中文）
├── LICENSE.md                ← MIT
├── CLAUDE.md / CODEBUDDY.md  ← 项目级 Agent 指令（CODEBUDDY.md → CLAUDE.md）
├── CHANGELOG.md              ← 版本记录
├── assets/                   ← LOGO 与视觉资源
└── .codebuddy/skills/        ← Agent 技能（vend 等）
```

> 运行时数据（产品、脚本、日志、含凭证的 `config.json`）位于 `docs/` 目录，**已 gitignore，不随仓库分发**，每个用户本地自备。

---

## ⚙️ 配置（运行时，仅本地）

所有运行时数据集中在 `docs/` 目录。核心配置文件：`docs/config.json`。

> ⚠️ `docs/config.json` 含明文账号密码 / API Key，已在 `.gitignore` 中排除，**不会提交到仓库**。下面只列结构（值为占位）：

```json
{
  "target":   { "amount": 30000, "currency": "HKD", "accept_crypto": true, "withdrawable_to": ["HK_bank", "CN_bank"] },
  "speed":    { "amount": 200, "currency": "HKD", "period": "hour" },
  "budget":   { "amount": 100, "currency": "HKD", "require_roi_before_spend": true },
  "assets":   { "product_dir": "docs/product", "history_dir": "docs/history", "log_dir": "docs/log" },
  "accounts": {
    "payloadz":   { "email": "", "password": "" },
    "PayPal":     { "email": "", "password": "" },
    "twitter":    { "email": "", "username": "", "password": "", "api_key": "", "api_secret": "", "access_token": "", "access_token_secret": "" },
    "instagram":  { "username": "", "email": "", "password": "" },
    "youtube":    { "channel_id": "", "email": "", "password": "" },
    "xiaohongshu":{ "username": "", "email": "", "password": "" }
  },
  "llm_api_keys": { "openai": "", "anthropic": "", "gemini": "", "openrouter": "", "Agnes": "", "devto_api_key": "" }
}
```

- `target`：金额目标（金额、货币、是否接受加密货币、可提现账户）
- `speed`：挣钱速度目标（单位时间收益，内部效率 KPI，**非**时薪外包）
- `budget`：可用预算（花钱前要求预估 ROI）
- `assets`：本地资产目录（产品 / 历史 / 日志）
- `accounts`：各平台账号凭证
- `llm_api_keys`：大模型 API Key

---

## 🚀 快速开始

1. 本地创建 `docs/config.json`，填好目标金额、预算、平台账号（不提交）。
2.（可选）从 **Scout** —— [ProductStrategistAgent](https://github.com/xhqing/ProductStrategistAgent) —— 拿到一个经过验证的产品机会，或自行准备好产品。
3. 让 Vendy 开始成交循环：
   ```
   /vend
   ```
4. 期间 Vendy 只在三种情况下会停下来找你：要注册账号 / 要你输密码或验证 / 要花钱。其余时间自主推进，直到达成目标。

---

## 🛡️ 安全与边界

- 只做合法合规的赚钱尝试，不利用漏洞、不刷单、不欺诈。
- 账号、密码、API Key 等敏感信息**只存本地** `config.json`，绝不写回 SKILL 或日志。
- 未经用户明确同意，不花一分钱；花钱必附 ROI 预估。
- 不蹭涉政、灾害、悲剧、严重争议类流量；不搬运受版权 / 商标保护的素材。
- 预估收益一律给区间并标注不确定性，不写「保证月入 X 万」。

---

## 📝 设计原则

- **被动收入优先**：一次产出，反复销售。
- **速度优先**：在合法合规前提下走最快产生收入的路径（内部效率 KPI = 200 HKD/小时）。
- **不断尝试**：失败一个渠道立即换下一个，不长时间停顿。
- **优先复用已有资产**：先吃 `product/` 和 `history/` 里的存货与经验。
- **能用现成 Skill 就不重造**：遇难题主动 `/find-skill`、`/install-skill`。

---

## 📄 协议

MIT，详见 [LICENSE.md](LICENSE.md)。

Copyright (c) 2026 All Contributors.

## 🏷️ 署名

使用、二次开发本项目时，请署名 **DigiVendAgent (Vendy)** 并引用项目地址：`https://github.com/xhqing/DigiVendAgent`。

*Vendy 会一直跑，直到把钱赚到手。*
