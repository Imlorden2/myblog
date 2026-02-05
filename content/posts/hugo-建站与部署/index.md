---
title: "Hugo 建站与部署"
date: 2025-10-10T14:59:28+08:00
draft: false
tags: ["网站", "Hugo"]
categories: ["技术"]
---

折腾了两个学生特惠的服务器，虽然没能做出一个传播广泛的网站，但在这个过程中倒是积累了不少的建站知识。现在回过头来看，[Github Pages](#github-pages) 不仅建站简单，而且维护方便。

基于本网站的建站过程，分享一下这个简单的过程。


> **环境信息**
> - **操作系统**: Linux (Kubuntu 24.04 LTS)
> - **Hugo 版本**: v0.151.0 extended
> - **其他**: Git、GitHub 账号





## 1. hugo 安装
> [hugo 是什么？](#hugo)
```bash
# bash commands !

# 安装 hugo
# sudo apt install hugo
# apt 安装的 hugo 版本太旧，与一些主题无法兼容，使用 snap 安装
sudo snap install hugo

# 安装 gh
sudo apt install gh

hugo version && gh --version

# 返回结果：
# hugo v0.151.0-c70ab27ceb841fc9404eab5d2c985ff7595034b7+extended linux/amd64 BuildDate=2025-10-02T13:30:36Z VendorInfo=snap:0.151.0
# gh version 2.45.0 (2025-07-18 Ubuntu 2.45.0-1ubuntu0.3)
# https://github.com/cli/cli/releases/tag/v2.45.0
```
> 环境简单是 hugo 的优势（hexo 还需要安装 node.js 环境）



## 2. 创建项目

```bash
# bash commands !

hugo new site myblog
tree

# 结果
.
├── archetypes
│   └── default.md
├── assets
├── content
├── data
├── hugo.toml
├── i18n
├── layouts
├── static
└── themes
```



## 3. 主题下载
> blowfish 官方文档在[这里](https://blowfish.page/)

```bash
# bash commands !

git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
mkdir -p config/_default  # 创建 hugo 项目的配置文件夹
cp -r themes/blowfish/config/_default/* config/_default/
rm hugo.toml  # 删除默认的配置文件
```



## 4. 创建文章

> 写文章注意[文章命名要求](#文章命名要求)

```bash
# bash commands !

hugo new content posts/Hello-Hugo.md
```

## 5. GitHub Actions 配置
```bash
# bash commands !

mkdir -p .github/workflows
gh auth login
  - GitHub.com
  - HTTPS
  - Yes (authenticate Git)
  - Login with a web browser（推荐）

git add .
git commit -m "Initial commit: Hugo blog with Blowfish theme"
gh repo create myblog --public --source=. --remote=origin --push
gh repo view --web
```

1. 访问仓库
   - 打开创建的仓库
   - 点击 Settings（设置）
   - 左侧菜单找到 Pages
   - Source：选择 GitHub Actions


2. 设置
   - 点击 Settings → Actions → General
   - 找到 Workflow permissions
   - 确保选择了 Read and write permissions 
   - 勾选 Allow GitHub Actions to create and approve pull requests
   - 点击 Save

3. 等待部署
   - 回到仓库首页，点击 Actions 标签
   - 查看 workflow 运行状态
   - 等待绿色✓（大约 1-2 分钟）

4. 访问你的网站
   - 部署成功后访问：https://username.github.io/myblog/

## 6. 后续操作
创建新文章：
`hugo new content posts/文章名.md`

本地预览：
`hugo server -D`

发布文章：
1. 编辑文章，设置 `draft: false`
2. `git add .`
3. `git commit -m "Add new post"`
4. `git push`
5. GitHub Actions 会自动部署

访问网站： https://username.github.io/myblog/

---
**后言**：

至此，就可以访问本 hugo 项目的 github page。部署在此无需租用服务器、不必担心服务器到期。开始愉快的写博客之旅吧～

另外，还有众多主题供你探索。但众多 Blogger 的体会都是：大多数人的热情都是搭站和配置主题，而忘记了博客的初衷以及重点都是：文章和内容。



---
## 注释部分
### Github Pages
github.io 是 GitHub 提供的免费静态网站托管服务域名。

**基本概念**

- 服务名称：GitHub Pages
- 域名格式：username.github.io 或 username.github.io/repository
- 用途：托管[静态网站](#静态网站)（HTML、CSS、JavaScript）
- 费用：完全免费

**两种使用方式**

1. 用户/组织站点
- 仓库名必须是：username.github.io
- 访问地址：https://username.github.io
- 每个账号只能有一个
2. 项目站点
- 任意仓库名
- 访问地址：https://username.github.io/repository-name
- 可以有无数个

**主要特点**

- ✓ 支持自定义域名
- ✓ 免费 HTTPS
- ✓ 支持 Jekyll 静态网站生成器
- ✓ 自动从仓库部署
- ✓ 支持 GitHub Actions 自动化构建
- ✗ 不支持服务端代码（PHP、Python后端等）
- ✗ 仓库大小限制 1GB
- ✗ 月流量建议不超过 100GB

**常见用途**

- 个人博客
- 项目文档
- 在线简历
- 开源项目演示页面


### 静态网站
> 静态网站 (Static Website)

**定义**

静态网站是由固定内容的文件（HTML、CSS、JavaScript）组成的网站，服务器直接把这些文件发送给浏览器，不需要实时生成内容。

**与动态网站的对比**

| 特性    | 静态网站     | 动态网站                    |
|-------|----------|-------------------------|
| 内容生成  | 预先生成好的文件 | 服务器实时生成                 |
| 数据库   | 不需要      | 通常需要                    |
| 服务端语言 | 不需要      | 需要（PHP、Python、Node.js等） |
| 速度    | 很快       | 相对慢                     |
| 成本    | 低（可免费托管） | 高（需要服务器）                |
| 安全性   | 更安全      | 相对复杂                    |

**静态网站的工作方式**

用户请求 → 服务器 → 直接返回 HTML 文件 → 浏览器显示

**动态网站的工作方式**

用户请求 → 服务器 → 执行代码 → 查询数据库 → 生成 HTML → 返回 → 浏览器显示

**静态网站示例**

- 个人博客
- 公司介绍页面
- 文档网站
- 作品集网站

**静态网站生成器**

- Jekyll（GitHub Pages 默认）
- Hugo
- Hexo
- VuePress / VitePress
- Gatsby

**优点**

✓ 加载速度快
✓ 托管便宜/免费
✓ 安全性高（没有服务端代码）
✓ 容易备份和迁移

**缺点**

✗ 不能有用户登录、评论等动态功能（除非借助第三方服务）
✗ 内容更新需要重新生成和部署
✗ 不适合需要实时数据的应用


### Hugo
**什么是 Hugo**

Hugo 是一个用 Go 语言编写的开源静态网站生成器，由 Steve Francia 于 2013 年创建。

**核心特点**

1. **极速构建**

- 号称"世界上最快的网站构建工具"
- 1 毫秒内可以生成一个页面
- 适合包含数千页面的大型网站

2. **零依赖**

- 单个二进制文件
- 无需安装 Ruby、Python、Node.js 等运行时
- 跨平台支持（Windows、macOS、Linux）

3. **易于使用**

- 简单的命令行界面
- 实时预览（热重载）
- Markdown 编写内容


**常用命令**

```bash
# bash commands !

# 创建新站点
hugo new site 网站名称

# 创建新文章
hugo new posts/文章名.md

# 本地预览（带草稿）
hugo server -D

# 构建生成静态文件
hugo

# 查看版本
hugo version
```
**主题系统**

- 支持主题切换
- 主题可以通过 Git submodule 管理
- 热门主题：
    - PaperMod：简洁现代的博客主题
    - Ananke：官方推荐主题
    - Stack：卡片式设计
    - **blowfish：本站使用的主题**

**模板引擎**

使用 Go 的模板语法：
- {{ .Title }}              // 文章标题
- {{ .Content }}            // 文章内容
- {{ range .Pages }}        // 遍历页面
- {{ .Site.Title }}         // 网站标题

**配置文件**

支持多种格式：
- hugo.toml（TOML，推荐）
- hugo.yaml（YAML）
- hugo.json（JSON）

**优势**
- 性能卓越：构建速度极快
- 简单安装：单文件，无依赖
- 灵活强大：支持复杂的内容组织
- 主题丰富：社区活跃
- SEO 友好：自动生成 sitemap、RSS
- 多语言支持：内置国际化功能
- 图片处理：内置图片优化功能

**劣势**
- Go 模板语法有学习曲线
- 不如 Jekyll 与 GitHub Pages 集成紧密（需要额外配置）
- 文档虽然详细但组织略显混乱

**与其他工具对比**

| 特性           | Hugo  | Jekyll | Hexo    |
|--------------|-------|--------|---------|
| 语言           | Go    | Ruby   | Node.js |
| 速度           | ⭐⭐⭐⭐⭐ | ⭐⭐     | ⭐⭐⭐     |
| 安装难度         | 简单    | 较难     | 中等      |
| GitHub Pages | 需配置   | 原生支持   | 需配置     |
| 社区           | 活跃    | 非常活跃   | 活跃（中文多） |

**学习资源**

- 官方文档：https://gohugo.io/documentation/
- 主题市场：https://themes.gohugo.io/

### 文章命名规范

#### 一. 存放位置
在运行 `hugo new content posts/Test1.md` 命令之后，Test1.md 会自动创建在 `～/content/posts/Test1.md` 下
- 如果是**纯文本文章**，可以直接书写，编辑上传；
- 如果文章包含图片等非文本文件，建议创建文件夹 `~/content/posts/Test1`，然后修改文章文件名字为 `index.md`。

这是 Hugo 的规范页面组织模式：
```
content/posts/
└── hugo-建站与部署/
   ├── index.md      # 文章内容
   ├── cover.jpg     # 封面图
   └── images/       # 相关图片
         └── pic1.png
```
好处有：
1. 资源管理集中：图片、附件等资源与文章在同一目录
2. 相对路径引用：可直接使用 `![](cover.jpg)` 引用同目录资源
3. 便于迁移：整个文件夹就是一个完整的文章单元
4. cover.jpg 将自动成为 index.md 的封面图

#### 二、构建系列

> 开启：`config/_default/hugo.toml` 中配置 `series = "series"  # 启用系列功能`

想要制作系列文章，需在文章 Front Matter 中添加 series 字段

```
---
title: "系列文章第一篇"
series: ["系列名"]  # 声明属于哪个系列
weight: 1  # 系列内排序权重（可选）
---
```


#### 三、标签和分类

1. 配置方法：在文章 Front Matter 中添加字段

```
---
tags: ["RPG", "BG3"]
categories: ["游戏"]
---
```

2. 区别

分类（Categories）
- 层级性：通常用于主要分类
- 数量少：一篇文章通常1-2个分类
- 范围广：如"技术"、"生活"、"随笔"
- 例子：编程、数据分析、读书笔记

标签（Tags）
- 扁平化：更细粒度的关键词
- 数量多：一篇文章可以有多个标签
- 具体化：描述文章具体内容
- 例子：Python、Hugo、Docker、算法

3. 建议
- Categories：1-2个，表示文章大类
- Tags：3-5个，描述具体技术点
- Series：用于连载或主题文章