# 🤖 Telegram Private Chatbot (v5.4) 

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/jikssha/telegram_private_chatbot)
![GitHub stars](https://img.shields.io/github/stars/jikssha/telegram_private_chatbot?style=social)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
[![Telegram](https://img.shields.io/badge/Telegram-DM-blue?style=social&logo=telegram)](https://t.me/vaghr_wegram_bot)
[🇺🇸 English](README_EN.md) | [🇨🇳 简体中文](README.md)

**Telegram Private Chatbot** 是一个基于 **Cloudflare Workers** 的高性能 Telegram 双向私聊机器人。它专为解决 Telegram 上的垃圾广告骚扰而生，拥有 0 延迟的本地人机验证系统、强大的管理员指令集以及无缝的消息转发体验。

无需购买服务器，利用 Cloudflare 强大的边缘计算网络，即可免费部署一套企业级的客户服务系统。

---

<details>
<summary>📢 <b>v5.4 版本更新公告 (2026-05-16)</b></summary>
   
### 新增功能：
- **关键词过滤系统**：新增硬编码+KV动态词库，支持文本及媒体描述 (Caption) 拦截。
- **动态管理指令**：新增 `/addword`, `/delword`, `/listwords`，无需重启即可实时维护屏蔽词库。
- **性能优化**：引入内存缓存，保证高并发下的词库查询速度。
   
### 历史修复：
- 自动话题修复、消息路由规范化、并发验证加固、多媒体转发优化等。
</details>

---

## 📑 目录 (Table of Contents)

*[✨ 核心特性](#-核心特性)
* [🛠️ 管理员指令](#-管理员指令)
* [🚀 部署教程](#-部署教程)
    * [方法一：GitHub 一键连接 (推荐)](#方法一github-一键连接部署-推荐-)
    * [方法二：手动复制部署](#方法二手动复制部署-简单直接)
    * [最后一步：激活 Webhook](#最后一步激活-webhook-至关重要)
* [❓ 常见问题 (FAQ)](#-常见问题-faq)
* [📈 Star History](#-star-history)

---

## ✨ 核心特性

v4.0 版本移除了所有不稳定的外部 API 依赖，专注于**极致的速度**与**绝对的稳定性**。

| 特性 | 描述 |
| :--- | :--- |
| **⚡ 0 延迟验证** | 采用**本地精选常识题库**。秒开秒验，彻底告别网络超时与接口报错，验证成功率 100%。 |
| **🛡️ 智能防骚扰** | **短 ID 机制**修复了 Telegram 按钮点击失效的 Bug。验证通过后提供 **30 天免打扰期**，兼顾安全与用户体验。 |
| **💬 话题群组管理** | 利用 **Telegram Forum Topics** 功能，自动为每位私聊用户创建一个独立的话题，消息隔离，管理井井有条。 |
| **👮 隐形指令系统** | 自动**拦截**用户端发送的 `/` 开头指令，防止普通用户骚扰管理员。管理指令仅在管理员群组内生效。 |
| **🔒 权限控制** | 强大的指令集：支持 **封禁 (/ban)**、**解封 (/unban)**、**结单 (/close)** 和 **永久信任 (/trust)** 等操作。 |
| **☁️ Serverless** | 完全基于 Cloudflare Workers 运行。**0 成本**、无需服务器、无需运维、抗高并发。 |
| **📸 多媒体支持** | 完美支持文本、图片、视频、文件等多种消息格式的双向转发，不丢失任何细节。 |

---

## 🛠️ 管理员指令

> **注意**：以下指令仅在 **管理员群组的话题内** 有效。用户在私聊窗口发送指令会被静默拦截，不会对管理员造成骚扰。

| 指令 | 作用 | 适用场景 |
| :--- | :--- | :--- |
| `/close` | **强制关闭对话** | 工单处理完成，礼貌结束咨询。 |
| `/open` | **重新开启对话** | 恢复对该用户的消息转发。 |
| `/ban` | **封禁用户** | 遇到恶意刷屏、广告机器人。 |
| `/unban` | **解封用户** | 恢复该用户的正常通讯权限。 |
| `/trust` | **永久信任** | 熟人、VIP 客户、长期合作伙伴。 |
| `/reset` | **重置验证** | 测试验证流程，或怀疑账号被盗。 |
| `/info` | **查看信息** | 查询用户资料。 |
| `/cleanup` | **批量清理** | 扫描并清理已删除话题的用户数据。 |
| `/addword <词>` | **添加屏蔽词** | 将词汇加入动态词库，实时拦截。 |
| `/delword <词>` | **删除屏蔽词** | 从动态词库中移除已添加的屏蔽词。 |
| `/listwords` | **查看词库** | 查看当前生效的全部屏蔽词列表。 |

#### 🚫 关键词过滤功能说明
- 本版本新增了关键词智能拦截功能，支持拦截文本及图片/视频描述 (Caption)。
- 通过命令添加的词汇会存入 KV，修改后 60 秒内全局生效。

---

## 🚀 部署教程

### 前置准备
1.  **Telegram Bot**：找 [@BotFather](https://t.me/BotFather) 申请一个机器人，获取 `Token`。
    * *重要设置*：在 BotFather 中关闭 **Group Privacy** (`/mybots` > Settings > Group Privacy > Turn off)。
2.  **管理员群组**：创建一个 Telegram 群组，并**开启话题功能 (Topics)**。
    * 将机器人拉入群组，并设为**管理员**（给予管理话题权限）。
    * 获取群组 ID（通常以 `-100` 开头）。

### 方法一：GitHub 一键连接部署 (推荐 ★)

1.  **Fork 本仓库** 到您的 GitHub 账户。
2.  登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)。
3.  导航到 **Workers & Pages** -> **Create Application**。
4.  点击 **Connect to Git** 标签页，选择刚才 Fork 的 `telegram_private_chatbot` 仓库。
5.  **配置部署设置**：点击 **Save and Deploy**。
6.  **⚠️ 关键步骤：绑定数据库与变量**
    * 进入 Worker 的 **Settings** -> **Variables**。
    * **绑定 KV 数据库**：添加 binding，名称填写 `TOPIC_MAP`，Namespace 选择你的 KV 数据库。
    * **添加环境变量**：`BOT_TOKEN` 和 `SUPERGROUP_ID`。
7.  **最后一步**：在 **Deployments** 页面点击 **Retry deployment**。

### 方法二：手动复制部署

1.  在 Cloudflare 创建一个 Worker。
2.  点击 **Edit code**，将 `worker.js` 代码全部粘贴进去。
3.  点击 **Deploy**。
4.  在 **Settings** -> **Variables** 中添加 `TOPIC_MAP` 绑定以及 `BOT_TOKEN`、`SUPERGROUP_ID` 变量。

---

### 最后一步：激活 Webhook (至关重要)

在浏览器中访问以下 URL（请替换 `<YOUR_TOKEN>` 和 `<YOUR_WORKER_URL>`）：
`https://api.telegram.org/bot<YOUR_TOKEN>/setWebhook?url=<YOUR_WORKER_URL>`

---

## ❓ 常见问题 (FAQ)

**Q1: 为什么点击验证按钮没有反应？**
A: 请确保 Webhook 设置正确，Telegram 必须能向你的 Worker 发送回调请求。

**Q2: 为什么关键词过滤没有拦截成功？**
A: 请确保在管理员群组的话题内执行指令。系统有 60 秒的内存缓存，添加指令后稍等片刻即可。

**Q3: 为什么机器人无法在群里创建话题？**
A: 确保机器人拥有群组的 "Manage Topics" 权限，且群组已开启话题功能。

---

## 📈 Star History

[![Star History Chart](https://api.star-history.com/svg?repos=jikssha/telegram_private_chatbot&type=date&legend=top-left)](https://star-history.com/#jikssha/telegram_private_chatbot&type=date&legend=top-left)

---
**如果这个项目对你有帮助，请给个 Star ⭐️ 吧！**