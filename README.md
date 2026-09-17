<div align="center">

<img src="assets/logo.svg" width="640" alt="Vendy logo" />

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Stars](https://img.shields.io/github/stars/xhqing/DigiVendAgent?style=social)
![Last Commit](https://img.shields.io/github/last-commit/xhqing/DigiVendAgent)
![AI Agent](https://img.shields.io/badge/Type-AI%20Agent-FF1493)
<img src="https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/xhqing/xhqing/main/traffic/badges/DigiVendAgent.json" alt="Visits/day (14d)" />

</div>

# DigiVendAgent

> **Codename: Vendy** — the sales & conversion agent of the team.
>
> "Digi (digital) + Vend (sell) + Agent" — Vendy is step ⑤ of a six-agent pipeline. She takes a finished digital product and turns it into money: list → price → fulfill → collect → withdraw, looping until the target is met. (Research = Scout, production = Wright, build = Mason, traffic = Buzz, analysis = Echo.)

Vendy's objective is the **sales side of passive income**. She doesn't take hourly freelance work or offer courses / managed services / consulting. Given a "produce-once, sell-many" digital product, she autonomously lists, prices, fulfills, collects payment, handles after-sales, and withdraws.

[中文文档 / Chinese](README_cn.md)

---

## 🎯 Positioning

| Dimension | Detail |
|-----------|--------|
| **What** | Autonomously sell digital goods online for profit (team step ⑤ — sales / conversion) |
| **What not** | No time-billed freelance; no human-intensive active services |
| **Market** | Global (English-first) + China (Xiaohongshu) |
| **Payout** | Withdraw to HK / CN bank (Payloadz + PayPal main route) |
| **Mode** | Continuous trial loop until the target is met |

---

## 🧠 Core Capabilities

> 🔎 **Trend research (`/hot-trend`) now lives in Scout** — [ProductStrategistAgent](https://github.com/xhqing/ProductStrategistAgent). Vendy is the **conversion / sales** agent: give her a product and she turns it into money.

### Vending Engine · `/vend` (the sales loop)

Vendy's "hands". Reads the target from `docs/config.json` and runs the **sales loop**: list → price → fulfill → collect → check metrics → adjust. Producing the product is **Wright**'s job ([ProductProducerAgent](https://github.com/xhqing/ProductProducerAgent)); driving traffic is **Buzz**'s ([GrowthMarketerAgent](https://github.com/xhqing/GrowthMarketerAgent)). **She only stops for three special cases**; otherwise she runs autonomously until the target is met.

- Trigger: `/vend`
- Listing & payment: Payloadz → PayPal (overseas); Xiaohongshu (China)
- Fulfillment & after-sales: delivery, refunds, disputes
- Three cases that require stopping to ask: account registration / permissions, password / verification input, spending money

---

## 🔁 Typical Workflow

Vendy is step **⑤** of a six-agent team:

```
① Scout (research) → ② Wright (produce) → ③ Mason (build site) → ④ Buzz (traffic) → ⑤ Vendy (sell / ops) → ⑥ Echo (analyze)
```

Run the sales loop:

```
/vend   →   list → price → promote → collect → withdraw (loop until target met)
```

---

## 📦 What's in This Repo

This open-source repo ships **the agent's design and skills** — not runtime data.

```
DigiVendAgent/
├── README.md                 ← this file (English)
├── README_cn.md              ← Chinese README
├── LICENSE.md                ← MIT
├── CLAUDE.md                 ← project-level agent instructions
└── .claude/
    ├── skills/
    │   ├── vend/             ← Vending Engine skill (sales loop)
    │   └── anysearch/        ← bundled web-search skill (third-party, its own license)
    └── rules/                ← project work rules
```

> Runtime data (products, scripts, logs, `config.json` with credentials) lives in `docs/`, which is **gitignored and not distributed**. Each user keeps their own locally.

---

## ⚙️ Configuration (runtime, local only)

Runtime data is centralized under `docs/`. Core config: `docs/config.json`.

> ⚠️ `docs/config.json` contains plaintext credentials / API keys. It is excluded by `.gitignore` and **never committed**. Structure only (values placeholder):

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

- `target`: money target (amount, currency, crypto acceptance, withdrawable accounts)
- `speed`: earning-speed target (internal efficiency KPI, **not** hourly freelance)
- `budget`: budget (require ROI estimate before spending)
- `assets`: local asset directories
- `accounts`: platform credentials
- `llm_api_keys`: LLM API keys

---

## 🚀 Quick Start

1. Create `docs/config.json` locally with your target, budget, and platform accounts (never committed).
2. (Optional) Get a validated product opportunity from **Scout** — [ProductStrategistAgent](https://github.com/xhqing/ProductStrategistAgent) — or supply your own product.
3. Start the sales loop:
   ```
   /vend
   ```
4. Vendy only stops for three reasons: account registration, password / verification, or spending money. Otherwise she runs autonomously until the target is met.

---

## 🛡️ Safety & Boundaries

- Only lawful, compliant money-making attempts; no exploits, no fake orders, no fraud.
- Credentials live **only** in local `config.json`; never written back to skills or logs.
- Never spends money without explicit user consent; every spend carries an ROI estimate.
- No traffic from politics, disasters, tragedies, or heavy controversy; no copyrighted / trademarked material.
- Earnings estimates are always given as ranges with uncertainty noted; never "guaranteed X per month".

---

## 📝 Design Principles

- **Passive income first**: produce once, sell many.
- **Speed first**: take the fastest legal path to revenue (internal KPI = 200 HKD/hour).
- **Keep trying**: if one channel fails, immediately move to the next.
- **Reuse existing assets**: consume inventory and lessons in `product/` and `history/` first.
- **Reuse skills, don't reinvent**: actively `/find-skill`, `/install-skill`.

---

## 📄 License

MIT — see [LICENSE.md](LICENSE.md). The bundled `anysearch` skill under `.claude/skills/anysearch/` is third-party and retains its own license and notice.

## 🏷️ Attribution

If you use, fork, or build on this project, please credit **DigiVendAgent (Vendy)** and link back to the project repository: `https://github.com/xhqing/DigiVendAgent`.

*Vendy keeps running until the money is made.*
