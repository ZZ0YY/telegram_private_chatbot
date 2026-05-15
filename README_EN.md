# 🛡️ TeleGuard (v5.3)

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/jikssha/telegram_private_chatbot)
![GitHub stars](https://img.shields.io/github/stars/jikssha/telegram_private_chatbot?style=social)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
[![Telegram](https://img.shields.io/badge/Telegram-DM-blue?style=social&logo=telegram)](https://t.me/vaghr_wegram_bot)[🇺🇸 English](README_EN.md) | [🇨🇳 简体中文](README.md)

**Telegram Private Chatbot** is a high-performance, two-way private messaging bot based on **Cloudflare Workers**. It is designed to solve the problem of spam harassment on Telegram, featuring a zero-latency local CAPTCHA verification system, a powerful set of administrator commands, and a seamless message forwarding experience.

Deploy a free, enterprise-grade customer service system utilizing Cloudflare's powerful edge computing network without purchasing any servers.

---

<details>
<summary>📢 <b>v5.3 Update Highlights (2026-05-16)</b></summary>
   
### New Features:
- **Keyword Filtering System**: Added a dual-layer filtering system (hardcoded + KV dynamic word list) that supports intercepting text messages and media captions.
- **Real-time Management**: Added admin commands (`/addword`, `/delword`, `/listwords`) to update the blacklist dynamically without redeploying.
- **Performance Optimization**: Introduced an in-memory caching mechanism to minimize KV read operations, ensuring high-speed responses under load.
</details>

---

## 📑 Table of Contents

* [✨ Key Features](#-key-features)
* [🛠️ Administrator Commands](#-administrator-commands)
* [🚫 Keyword Filtering System](#-keyword-filtering-system)
* [🚀 Deployment Tutorial](#-deployment-tutorial)
* [❓ FAQ](#-faq)
* [📈 Star History](#-star-history)

---

## ✨ Key Features

| Feature | Description |
| :--- | :--- |
| **⚡ Zero-Latency Verification** | Uses a local curated trivia database. Verifies instantly with a 100% success rate. |
| **🚫 Keyword Filtering** | Dual-layer filtering (hardcoded + dynamic KV) with support for `Caption` filtering. Manage rules via chat commands. |
| **🛡️ Smart Anti-Spam** | Short ID mechanism for reliable callback handling. 30-day disturbance-free period after verification. |
| **💬 Topic Group Management** | Automatically creates separate topics for private chat users for organized management. |
| **👮 Invisible Command System** | Automatically intercepts `/` commands sent by users. Admin commands work only in the admin group. |
| **☁️ Serverless** | Runs entirely on Cloudflare Workers. Zero cost, server-free, and handles high concurrency. |

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
| `/addword <word>` | **Add Keyword** | Block new types of spam/ads in real-time. |
| `/delword <word>` | **Remove Keyword** | Remove an incorrect or unnecessary block rule. |
| `/listwords` | **List Keywords** | Display all active blocked words (Hardcoded + Dynamic). |

---

## 🚫 Keyword Filtering System

To effectively block spam, this project includes a powerful keyword interception engine:

*   **Dual-Layer Validation**: Built-in hardcoded blacklist (cannot be deleted via command) + customizable dynamic KV word list.
*   **Comprehensive Interception**: Detects both standard text messages and the `caption` (description) of photos/videos.
*   **Command Management**:
    *   `/addword <word>`: Adds a keyword to the dynamic blacklist.
    *   `/delword <word>`: Removes a keyword from the dynamic blacklist.
    *   `/listwords`: View the full list of currently active blocked words.

*Example: Send `/addword gambling` in the admin topic to automatically block all messages containing "gambling".*

---

## 🚀 Deployment Tutorial

### 1. Prerequisites
*   **Telegram Bot**: Apply for a bot from [@BotFather](https://t.me/BotFather) and get the `Token`. *Important: Turn off Group Privacy.*
*   **Administrator Group**: Create a group, enable Topics, add the bot, and grant "Manage Topics" permission.

### 2. Deployment Steps
1.  **Fork this repository** to your GitHub.
2.  In Cloudflare Workers & Pages, select **Connect to Git** to link this repository.
3.  **Configure Binding & Variables**:
    *   In **Settings -> Variables**, bind a KV Namespace (Variable name **must** be `TOPIC_MAP`).
    *   Add Environment Variables `BOT_TOKEN` and `SUPERGROUP_ID` (starts with `-100`).
4.  **Activate Webhook**:
    Visit the following URL in your browser (replace Token and Worker URL):
    `https://api.telegram.org/bot<YOUR_TOKEN>/setWebhook?url=https://<YOUR_WORKER_URL>`

---

## ❓ FAQ

**Q: Why does clicking the verification button do nothing?**
A: Ensure the Webhook is set correctly. You can try deleting the old Webhook first and setting it again.

**Q: Why isn't keyword filtering working?**
A: Ensure you sent the command in the admin group topic. The system has a 60-second memory cache for keywords, so please wait a moment for changes to propagate.

**Q: Can I use a custom domain?**
A: If custom domain setup fails, please use the default `workers.dev` domain for initial debugging to ensure your network environment is correct.

---

## 🔒 Security Note

> [!IMPORTANT]
> Please keep your `BOT_TOKEN` safe. Never share it or commit it to public repositories.

---
**If this project helps you, please give it a Star ⭐️!**