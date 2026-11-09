---
title: "Configuring Claude Code"
date: 2026-11-07T11:45:31+08:00
draft: false
tags: []
categories: []
---

## 配置 claude code

### node.js 和 npm

Node.js 和 npm 是配套使用的一对工具，可以这样理解它们的关系：**Node.js 是运行 JavaScript 的环境，npm 是管理 JavaScript 包的工具**。两者通常一起安装，装 Node.js 时 npm 会自动带上。

- Node.js 是什么

Node.js 是一个**让 JavaScript 脱离浏览器运行的环境**。JavaScript 最初只能在浏览器里跑，用来做网页交互；Node.js 把它带到了服务器端和本地终端，让你可以用 JavaScript 写命令行工具、后端服务、构建脚本等。

它基于 Chrome 的 V8 引擎，执行效率很高。你之前要装的 Claude Code 就是一个用 Node.js 写的命令行工具，所以必须先有 Node.js 才能运行它。

Node.js 目前有两个发布版本线：**LTS（长期支持版）** 和 **Current（最新版）**。日常使用建议选 LTS，稳定、维护周期长；官方下载页默认推荐的也是 LTS。

- npm 是什么

npm 全称 Node Package Manager，是 Node.js 的**包管理器**，也是世界上最大的开源代码仓库之一。它负责三件事：

- **安装依赖**：一条命令就能把别人写好的代码库下载到你的项目里。
- **管理版本**：记录你的项目依赖了哪些包、分别是什么版本。
- **运行脚本**：在 `package.json` 里定义快捷命令，用 `npm run xxx` 执行。

### 安装 node.js

- 先安装 homebrew...

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

Homebrew 是 macOS（以及 Linux/WSL）上的**包管理器**。它能用一行命令就能安装、更新和卸载开发工具或普通应用，不用再去官网下载 `.dmg` 或 `.pkg` 文件手动拖拽。

macOS 自带了一些命令行工具，但开发者常用的很多工具（比如 `wget`、`node`、`git` 的新版本）并不在其中。Homebrew 就是用来补上这些“系统缺的软件”，并且统一管理它们的安装和升级。


- 接着安装 node.js

```bash
brew install node
# 运行了大概十五分钟

node -v && npm -v
# v26.13.0
# 11.20.0
```


### 安装 claude code

```bash
npm install -g @anthropic-ai/claude-code
```



### 配置 deepseek 大模型到 CC

- [下载 cc swtich](https://github.com/farion1231/cc-switch) 并安装

- 在应用内修改配置并应用



接下来就能愉快地使用 claude code 了。
