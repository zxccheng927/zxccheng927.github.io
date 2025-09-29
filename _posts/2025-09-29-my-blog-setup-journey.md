---
title: 测试文章
date: 2025-09-29 16:15:00 +0800
categories:
  - 信息技术
tags:
  - 网站
toc: true
comments: true
published: true
---

## 前言

在数字信息的浪潮中，我一直渴望能有一个完全由自己掌控的角落，用来沉淀思考、记录学习、分享知识。经过一番探索，我最终选择了 **GitHub Pages + Jekyll + Chirpy 主题** 这套备受推崇的技术栈，并结合强大的笔记软件 **Obsidian**，打造了一套属于我自己的、高度自动化的写作与发布工作流。

这不仅仅是一篇技术教程，更像是一本航海日志。它记录了我从一个对 Jekyll 一无所知的新手，如何一步步配置环境、搭建网站、解决部署难题，并最终实现“在 Obsidian 中一键发布”的完整历程。

如果你也想拥有一个免费、美观、稳定且能高效产出的个人博客，希望我的这段经历能为你点亮前行的灯塔。

## 阶段一：奠定基石 - 本地环境搭建 (macOS)

万丈高楼平地起，一个稳定可靠的本地环境是所有工作的基础。

### 1. 安装 Homebrew 与 Git

Homebrew 是 macOS 上的包管理神器，而 Git 则是连接我们本地电脑与云端 GitHub 的桥梁。打开终端 (Terminal)，依次安装和配置它们。

```bash
# 安装 Homebrew (如果尚未安装)
/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"

# 配置 Git 身份，这将作为你提交代码的签名
# 将 "Your-Username" 替换为你的 GitHub 用户名
git config --global user.name "Your-Username"
# 推荐使用 GitHub 提供的隐私邮箱，可以在 GitHub 设置的 Emails 页面找到
git config --global user.email "your-github-privacy-email@users.noreply.github.com"
2. 安装 Ruby 与 Jekyll 环境
Jekyll 是一个基于 Ruby 的静态网站生成器。我们使用 Homebrew 来安装一个纯净的 Ruby 环境。

Bash

# 1. 使用 Homebrew 安装 Ruby
brew install ruby

# 2. 将 Ruby 的路径添加到环境变量中（关键步骤！）
# 请务必执行你终端安装后提示的那条 echo '...' >> ~/.zshrc 命令
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 3. 安装 Jekyll 和 Bundler (Ruby 的依赖管理器)
gem install jekyll bundler
至此，我们的本地开发环境已准备就绪。

阶段二：创建博客 - Chirpy 主题模板
我们不从零开始，而是站在巨人的肩膀上。Chirpy 主题提供了一个完美的“启动器模板”。

访问模板仓库： https://github.com/cotes2020/jekyll-theme-chirpy-starter

使用模板创建新仓库： 点击 Use this template -> Create a new repository。

命名仓库（核心规则）： 仓库名必须是 你的用户名.github.io。这是 GitHub Pages 用户站点的硬性规定。

克隆到本地并安装依赖：

Bash

# 克隆你刚刚创建的仓库
git clone [https://github.com/你的用户名/你的用户名.github.io.git](https://github.com/你的用户名/你的用户名.github.io.git)

# 进入项目目录
cd 你的用户名.github.io

# 安装主题所需的所有依赖包
bundle install
阶段三：个性化配置与部署难题攻克
在本地成功运行 bundle exec jekyll serve 并看到预览后，我开始进行个性化配置，并遇到了第一个、也是最关键的一个部署难题。

1. 核心配置 _config.yml
这是博客的“大脑”，我在这里修改了网站标题、语言、时区等。但有两个配置项至关重要，直接关系到部署的成败：

YAML

# _config.yml

# 网站的完整线上地址，协议头必须是 https，末尾不能有斜杠
url: "[https://你的用户名.github.io](https://你的用户名.github.io)"

# 网站的根路径，对于用户站点，必须是空字符串
baseurl: ""
2. 攻克 htmlproofer 部署错误
在我首次 git push 后，GitHub Actions 部署失败了。日志显示，一个名为 htmlproofer 的链接检查工具无法正确识别我网站的绝对链接。

解决方案是修改自动化工作流文件，让检查工具忽略我的域名。

文件位置：.github/workflows/pages-deploy.yml

修改内容：找到 Test site 步骤，将其 run 命令修改为如下内容，将自己的域名添加到忽略列表中：

YAML

- name: Test site
  run: |
    bundle exec htmlproofer _site \
      --disable-external \
      --ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/,/^https:\/\/你的用户名\.github\.io/"
完成这个修改并再次推送后，我的网站终于成功部署上线了！

阶段四：终极进化 - Obsidian 自动化工作流
网站上线只是开始，如何优雅、高效地产出内容才是关键。为此，我将目光投向了 Obsidian。

1. 基础配置：让 Obsidian 成为博客的一部分
将 _posts 目录作为库 (Vault)：让 Obsidian 只专注于文章内容。

配置附件存放路径：在 设置 -> 文件与链接 中，将附件默认存放路径设置为 assets/img/posts。从此，粘贴图片会自动存放到正确的位置。

2. 强大的文章模板
我创建了一个包含“草稿”功能的模板，每次新建文章时，只需一键插入。

YAML

---
title: 
date: {{date:YYYY-MM-DD HH:mm:ss +0800}}
categories: []
tags: []
toc: true
comments: true
# 只要这行存在，文章就是安全的草稿状态，准备发布时删除即可
published: false 
---