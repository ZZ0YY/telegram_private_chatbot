# 🛡️ TeleGuard (v5.4)

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/jikssha/telegram_private_chatbot)
![GitHub stars](https://img.shields.io/github/stars/jikssha/telegram_private_chatbot?style=social)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
[![Telegram](https://img.shields.io/badge/Telegram-DM-blue?style=social&logo=telegram)](https://t.me/vaghr_wegram_bot)

[🇺🇸 English](README_EN.md) | [🇨🇳 简体中文](README.md)

**Telegram Private Chatbot** is a high-performance, two-way private messaging bot based on **Cloudflare Workers**. It is designed to solve the problem of spam harassment on Telegram, featuring a zero-latency local CAPTCHA verification system, a powerful set of administrator commands, and a seamless message forwarding experience.

Deploy a free, enterprise-grade customer service system utilizing Cloudflare's powerful edge computing network without purchasing any servers.

---

<details>
<summary>📢 <b>v5.4 Update Highlights (2026-05-16)</b></summary>
   
### New Features:
- **Keyword Filtering System**: Added a dual-layer filtering system (hardcoded + KV dynamic word list) supporting text and media caption interception.
- **Dynamic Management**: Added admin commands (`/addword`, `/delword`, `/listwords`) for real-time blacklist maintenance.
- **Performance**: Introduced in-memory caching for high-speed keyword lookups.
</details>

---

## 📑 Table of Contents

* [✨ Key Features](#-key-features)
* [🛠️ Administrator Commands](#-administrator-commands)
* [🚀 Deployment Tutorial](#-deployment-tutorial)
    *[Method 1: One-Click Deploy via GitHub (Recommended)](#method-1-one-click-deploy-via-github-recommended-)
    * [Method 2: Manual Deployment](#method-2-manual-deployment-simple--direct)
    * [Final Step: Activate Webhook](#final-step-activate-webhook-crucial)
*[❓ FAQ](#-faq)
* [📈 Star History](#-star-history)

---

## ✨ Key Features

| Feature | Description |
| :--- | :--- |
| **⚡ Zero-Latency Verification** | Uses a **local curated trivia database**. Verifies instantly, with a 100% success rate. |
| **🛡️ Smart Anti-Spam** | **Short ID mechanism** provides a **30-day disturbance-free period** after verification. |
| **💬 Topic Group Management** | Utilizes **Telegram Forum Topics** to automatically create separate topics for each user for organized management. |
| **👮 Invisible Command System** | Automatically **intercepts** `/` commands sent by users. Admin commands are only effective within the admin group. |
| **🔒 Permission Control** | Powerful command set: Supports **Ban (/ban)**, **Unban (/unban)**, **Close Ticket (/close)**, and **Trust (/trust)**. |
| **☁️ Serverless** | Runs entirely on Cloudflare Workers. **Zero cost**, server-free, and handles high concurrency. |
| **📸 Multimedia Support** | Perfectly supports two-way forwarding of text, images, videos, and files. |

---

## 🛠️ Administrator Commands

> **Note**: The following commands are only effective within **topics in the administrator group**.

| Command | Action | Scenario |
| :--- | :--- | :--- |
| `/close` | **Force Close Chat** | Ticket resolved; politely ending the consultation. |
| `/open` | **Reopen Chat** | Resumes message forwarding for the user. |
| `/ban` | **Ban User** | Malicious spamming, ad bots. |
| `/unban` | **Unban User** | Giving a second chance. |
| `/trust` | **Permanent Trust** | VIP clients or partners (no CAPTCHA). |
| `/reset` | **Reset Verification** | Testing flow or suspected compromise. |
| `/info` | **View Info** | Checking user UID and topic link. |
| `/cleanup` | **Batch Cleanup** | Scans and removes data for deleted topics. |
| `/addword <word>` | **Add Keyword** | Block new spam/ads in real-time. |
| `/delword <word>` | **Remove Keyword** | Remove an incorrect or unnecessary block rule. |
| `/listwords` | **List Keywords** | Display all active blocked words (Hardcoded + Dynamic). |

#### 🚫 Keyword Filtering Description
- Added intelligent keyword interception, supporting text and media `caption` detection.
- Keywords added via commands are stored in KV and take effect globally within 60 seconds.

---

## 🚀 Deployment Tutorial

### Prerequisites
1.  **Telegram Bot**: Apply for a bot from[@BotFather](https://t.me/BotFather) and get the `Token`. *Important: Turn off Group Privacy.*
2.  **Administrator Group**: Create a group, enable Topics, add the bot, and grant "Manage Topics" permission.

### Method 1: One-Click Deploy via GitHub (Recommended ★)
1.  **Fork this repository** to your GitHub.
2.  In Cloudflare Workers & Pages, select **Connect to Git** to link this repository.
3.  **Configure Binding & Variables**:
    *   In **Settings -> Variables**, bind a KV Namespace (Variable name **must** be `TOPIC_MAP`).
    *   Add Environment Variables `BOT_TOKEN` and `SUPERGROUP_ID` (starts with `-100`).
4.  **Final Step**: In the **Deployments** tab, click **Retry deployment**.

### Method 2: Manual Deployment (Simple & Direct)
1.  Create a Worker in Cloudflare.
2.  Click **Edit code**, paste the `worker.js` content.
3.  Click **Deploy**.
4.  In **Settings -> Variables**, add the `TOPIC_MAP` binding and required environment variables.

---

### Final Step: Activate Webhook (Crucial)
Visit the following URL in your browser (replace Token and Worker URL):
`https://api.telegram.org/bot<YOUR_TOKEN>/setWebhook?url=https://<YOUR_WORKER_URL>`

---

## ❓ FAQ

**Q: Why does clicking the verification button do nothing?**
A: Ensure the Webhook is set correctly. You can try deleting the old Webhook first and setting it again.

**Q: Why isn't keyword filtering working?**
A: Ensure you sent the command in the admin group topic. The system has a 60-second memory cache, so wait a moment for changes to propagate.

**Q: Why can't the bot create topics?**
A: Ensure the bot has "Manage Topics" permission and that the group has Topics enabled.

---

## 🔒 Security Note

>[!IMPORTANT]
> Please keep your `BOT_TOKEN` safe. Never share it or commit it to public repositories.

---
**If this project helps you, please give it a Star ⭐️!**