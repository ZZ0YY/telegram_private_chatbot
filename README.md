# 🤖 Telegram Private Chatbot (v5.3) 

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/jikssha/telegram_private_chatbot)
![GitHub stars](https://img.shields.io/github/stars/jikssha/telegram_private_chatbot?style=social)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
[![Telegram](https://img.shields.io/badge/Telegram-DM-blue?style=social&logo=telegram)](https://t.me/vaghr_wegram_bot)
[🇺🇸 English](README_EN.md) | [🇨🇳 简体中文](README.md)

**Telegram Private Chatbot** 是一个基于 **Cloudflare Workers** 的高性能 Telegram 双向私聊机器人。它专为解决 Telegram 上的垃圾广告骚扰而生，拥有 0 延迟的本地人机验证系统、强大的管理员指令集以及无缝的消息转发体验。

无需购买服务器，利用 Cloudflare 强大的边缘计算网络，即可免费部署一套企业级的客户服务系统。

---

<details>
<summary>📢 <b>v5.3 版本更新公告 (2026-05-16)</b></summary>
   
### 新增功能：
- **关键词过滤系统**：新增硬编码+KV动态词库，支持拦截文本及媒体描述 (Caption)。
- **实时管理**：支持 `/addword`、`/delword`、`/listwords` 等管理员指令，无需重启即可动态更新屏蔽列表。
- **性能优化**：引入内存缓存机制，降低 KV 读取频率，保证高并发下的响应速度。
   
### 历史修复：
- 自动话题修复、消息路由规范化、并发验证加固、多媒体转发优化等。
</details>

---

## 📑 目录 (Table of Contents)

* [✨ 核心特性](#-核心特性)
* [🛠️ 管理员指令](#-管理员指令)
* [🚫 关键词过滤系统](#-关键词过滤系统)
* [🚀 部署教程](#-部署教程)
* [❓ 常见问题 (FAQ)](#-常见问题-faq)

---

## ✨ 核心特性

| 特性 | 描述 |
| :--- | :--- |
| **⚡ 0 延迟验证** | 采用本地精选常识题库。秒开秒验，验证成功率 100%。 |
| **🚫 关键词过滤** | 内置硬编码与 KV 动态双层词库，支持过滤 `Caption`，管理员可实时管理词库。 |
| **🛡️ 智能防骚扰** | 验证通过后提供 30 天免打扰期，兼容安全与用户体验。 |
| **💬 话题群组管理** | 利用 Forum Topics 自动为用户创建独立话题，消息管理井井有条。 |
| **👮 隐形指令系统** | 自动拦截用户端发送的 `/` 开头指令，防止骚扰，管理指令仅群内生效。 |
| **☁️ Serverless** | 完全基于 Cloudflare Workers，0 成本、无需运维、抗高并发。 |

---

## 🛠️ 管理员指令

> **注意**：以下指令仅在 **管理员群组的话题内** 有效。

| 指令 | 作用 | 适用场景 |
| :--- | :--- | :--- |
| `/close` | 强制关闭对话 | 工单处理完成，礼貌结束咨询。 |
| `/open` | 重新开启对话 | 恢复对该用户的消息转发。 |
| `/ban` | 封禁用户 | 遇到恶意刷屏、广告机器人。 |
| `/unban` | 解封用户 | 恢复该用户的正常通讯权限。 |
| `/trust` | 永久信任 | 熟人、VIP 客户、长期合作伙伴。 |
| `/reset` | 重置验证 | 测试流程或怀疑账号被盗。 |
| `/info` | 查看用户信息 | 查询用户的 UID 及话题链接。 |
| `/cleanup` | 批量清理 | 扫描并清理已删除话题的残留数据。 |

---

## 🚫 关键词过滤系统

为了有效阻断垃圾信息，本项目内置了关键词拦截功能：

*   **双层校验**：程序内置基础屏蔽词库（硬编码），同时支持通过命令添加动态屏蔽词。
*   **全方位拦截**：支持文本消息及图片/视频的 `caption`（描述文字）检测。
*   **指令管理**：
    *   `/addword <关键词>`：将词汇加入动态词库。
    *   `/delword <关键词>`：从动态词库中移除词汇。
    *   `/listwords`：查看当前已生效的全部屏蔽词列表。

*示例：在管理员话题内发送 `/addword 赌博`，机器人将自动拦截后续所有包含“赌博”的消息。*

---

## 🚀 部署教程

### 1. 前置准备
*   **Telegram Bot**：找 [@BotFather](https://t.me/BotFather) 申请 Token。*注意：请在 BotFather 中关闭 Group Privacy (Turn off)。*
*   **管理员群组**：创建群组并开启话题功能 (Topics)，将机器人拉入并给予管理权限。

### 2. 部署步骤
1.  **Fork 本仓库** 到您的 GitHub。
2.  在 Cloudflare Workers & Pages 中选择 **Connect to Git** 关联此仓库。
3.  **配置绑定与变量**：
    *   在 **Settings -> Variables** 中绑定 KV Namespace（名称必须为 `TOPIC_MAP`）。
    *   添加环境变量 `BOT_TOKEN` 和 `SUPERGROUP_ID` (群组 ID 需以 `-100` 开头)。
4.  **激活 Webhook**：
    在浏览器访问以下链接（替换 Token 和域名）：
    `https://api.telegram.org/bot<YOUR_TOKEN>/setWebhook?url=https://<YOUR_WORKER_URL>`

---

## ❓ 常见问题 (FAQ)

**Q: 为什么点击验证按钮没有反应？**
A: 请确保 Webhook 设置正确。若不成功，可先访问 `deleteWebhook` 清理旧设置，再重新设置。

**Q: 为什么关键词过滤没有拦截成功？**
A: 请检查是否在管理员群组发送的指令生效了，并通过 `/listwords` 确认词库是否已更新。动态词库有 60 秒缓存，稍等片刻即可生效。

**Q: 为什么无法自定义域名？**
A: 若自定义域名设置不成功，请先使用 `workers.dev` 默认域名进行调试，确保网络环境无误。

---

## 🔒 安全说明
请妥善保管您的 `BOT_TOKEN`，不要上传到公开仓库或分享给他人。

---
**如果这个项目对你有帮助，请给个 Star ⭐️ 吧！**