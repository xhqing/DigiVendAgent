<div align="center">

<img src="assets/logo.svg" width="640" alt="Vendy logo" />

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Stars](https://img.shields.io/github/stars/xhqing/DigiVendAgent?style=social)
![Last Commit](https://img.shields.io/github/last-commit/xhqing/DigiVendAgent)
![Built with Claude Code](https://img.shields.io/badge/Built%20with-Claude%20Code-19C37D)
![AI Agent](https://img.shields.io/badge/Type-AI%20Agent-FF1493)

</div>

# DigiVendAgent

> **Codename: Vendy** — your autonomous digital-goods vendor.
>
> "Digi (digital) + Vend (sell) + Agent" — purpose-built to produce digital / virtual / SaaS / MaaS / software services automatically, sell them across the global internet, and collect payouts until a configured money target is met.

Vendy is an autonomous agent whose sole objective is **passive income**. She doesn't take hourly or daily freelance work, nor does she offer courses, managed services, or consulting that require ongoing human effort. She only builds "produce-once, sell-many" digital products, then autonomously lists, promotes, collects payment, and withdraws — looping until the target amount is reached.

[中文文档 / Chinese](README_cn.md)

---

## 🎯 Positioning

| Dimension | Detail |
|-----------|--------|
| **What** | Autonomously produce and sell digital goods online for profit |
| **What not** | No time-billed freelance; no human-intensive active services |
| **Market** | Global (English-first) + China (Xiaohongshu) |
| **Payout** | Withdraw to HK / CN bank (Payloadz + PayPal main route) |
| **Mode** | Continuous trial loop until the target is met |

---

## 🧠 Core Capabilities (Two Skills)

### 1. Trend Radar · `/hot-trend` (upstream selection)

Vendy's "eyes". Scans global English-language trend boards in parallel (Google Trends, Exploding Topics, X, Reddit, TikTok, Product Hunt, Hacker News, Gumroad, etc.), filters with a five-dimension scorecard, and converges on the **single** trend best suited to become a shippable digital product. Outputs a complete executable plan, then stops and waits for your confirmation.

- Trigger: `/hot-trend`
- Iron rule: output exactly one trend, never a list; decisions must be data-backed and actionable; time window ≥ 2 weeks.
- Output: `docs/product/hot-trend-<slug>.md` (runtime data, not shipped with this repo)

### 2. Vending Engine · `/vend` (production + sales)

Vendy's "hands". Reads the target from `docs/config.json` and enters a continuous loop: produce → list → promote → collect → check metrics → adjust. **She only stops for three special cases**; otherwise she runs autonomously until the target is met.

- Trigger: `/vend`
- Listing & payment: Payloadz → PayPal (overseas); Xiaohongshu (China)
- Promotion: X, Instagram, YouTube, Xiaohongshu
- Three cases that require stopping to ask: account registration / permissions, password / verification input, spending money

---

## 🔁 Typical Workflow

```
/hot-trend   →   lock onto one trend + actionable plan (stops, awaits confirmation)
     │
     ▼
  /vend       →   produce → list → promote → collect → loop until target met
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
    │   ├── hot-trend/        ← Trend Radar skill
    │   ├── vend/             ← Vending Engine skill
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
2. Have Vendy pick a sellable trend:
   ```
   /hot-trend
   ```
3. Once you confirm the plan, start the produce-and-sell loop:
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
