---
title: 自动化部署Hexo到GithubPages
abbrlink: 9ffc30a8
date: 2025-05-25 17:34:54
tags:
categories:
keywords:
cover:
---

# 使用 Github Actions 自动化部署 Hexo 到 Github Pages

1\. 创建 GitHub 仓库

1. **操作步骤**：

- 登录你的 GitHub 账号。

- 点击页面右上角的 `New repository` 按钮。

- 在 `Repository name` 处输入仓库名称（例如 `myblog`）。

- 可根据需要勾选 `Initialize this repository with a README`，完成仓库创建。

1. **关键说明**：

- 强烈建议将仓库名称设置为 `username.github.io` 的格式（`username` 为你的 GitHub 用户名），如此配置后，可直接通过 `https://username.github.io` 访问博客，无需进行额外的域名配置。

- 若仓库名称并非 `username.github.io` 格式，则需要在 Hexo 项目的 `_config.yml` 文件中进行如下修改：

```yaml
url: https://username.github.io;
root: /myblog/ # 根据实际仓库名称，明确指定根路径
```

2\. 创建工作流文件

在 Hexo 项目的 `.github` 目录下，新建 `workflows` 文件夹，并在其中创建一个 `.yml` 或 `.yaml` 格式的文件（例如 `pages.yaml`），可通过以下命令在终端完成操作：

```bash
mkdir -p ./.github/workflows
touch pages.yaml
```

3\. 编写工作流

在 `pages.yaml` 文件中编写如下内容，用于定义自动化部署的工作流程：

```yaml
name: 自动化部署Hexo到Github Pages

on:
  push:
    branches: main

permissions:
  contents: write

jobs:
  hexo:
    runs-on: ubuntu-latest
    env:
      # 设置时区为中国标准时间（UTC+8）
      TZ: Asia/Shanghai
    steps:
      - name: 拉取代码
        uses: actions/checkout@v4

      - name: 安装依赖
        run: npm install

      - name: 生成静态文件
        run: npm run build

      - name: 部署
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          branch: gh-pages
          folder: public
```

4\. 提交仓库

完成上述配置后，将修改后的代码提交到 GitHub 仓库，触发 `push` 事件，从而启动自动化部署流程。

5\. 设置 GitHub Pages

1. 进入 GitHub 仓库的 `Settings` 页面。

2. 在 `Pages` 选项卡中，将 `Source` 设置为 `Deploy from a branch`。

3. 选择与工作流中 `branch` 字段一致的分支（如 `gh-pages`）作为存放静态文件的分支，保存设置。

配置完成后，GitHub Actions 会自动监听仓库的 `push` 事件，执行 Hexo 项目的构建与部署操作，最终将博客部署到 GitHub Pages 上。
