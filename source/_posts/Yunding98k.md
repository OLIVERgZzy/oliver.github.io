---
title: 云顶之弈攻略小程序开发实录
top: false
cover: https://camo.githubusercontent.com/895966d1f8cb49bbf2cbf24266a4ac9f7d7703268f550dee85d71f78fc50cc29/68747470733a2f2f736f6369616c6966792e6769742e63692f4f4c49564552675a7a792f79756e64696e6739386b2f696d6167653f6465736372697074696f6e3d3126666f6e743d496e74657226666f726b733d31266973737565733d31266c616e67756167653d31266f776e65723d31267061747465726e3d5369676e616c2670756c6c733d31267374617267617a6572733d31267468656d653d4461726b
toc: false
mathjax: false
date: 2021-04-11 10:01:45
top_img:
description:
tags:
  - TypeScript
  - Docker Compose
  - Nest.js
  - 微信小程序
  - Nginx
  - 爬虫
  - selenium
categories:
  - 前端
  - 我的项目
keywords:
---

## 小程序截图

微信小程序搜索【云顶 98k】或扫描下方二维码
![二维码](https://cdn.jsdelivr.net/gh/OLIVERgZzy/i-love-auto-chess-blog-cdn@master/css/images/qrcode.jpg)

![小程序截图](https://github.com/OLIVERgZzy/yunding98k/blob/main/miniapp01.jpg?raw=true)
![小程序截图](https://github.com/OLIVERgZzy/yunding98k/blob/main/miniapp02.jpg?raw=true)
![小程序截图](https://github.com/OLIVERgZzy/yunding98k/blob/main/miniapp03.jpg?raw=true)
![小程序截图](https://github.com/OLIVERgZzy/yunding98k/blob/main/miniapp04.jpg?raw=true)
![小程序截图](https://github.com/OLIVERgZzy/yunding98k/blob/main/miniapp05.jpg?raw=true)

## 博客截图

体验地址 [https://www.manito.fun/](https://www.manito.fun/)

![博客截图](./blog01.png)

## 前言

开发这个项目的初衷是因为个人比较喜欢英雄联盟这款游戏中的一个模式————云顶之弈。玩家可以通过收集不同的英雄装备、组合出一支队伍，与其他玩家对战。我个人是被这种新颖的玩法深深的吸引，为了能不断的吃鸡，唯一的方法就是尽快收集到最强的阵容。于是我谷歌，发现国外有一个游戏攻略网站专门收录各种牛逼的阵容，要是能把数据爬下来为我所用那该多好，于是就有了此项目。

## 爬虫

那么开发这款小程序第一步要做的事情，就是如何从网页中把数据爬下来。于是我开始分析目标网站：[https://app.mobalytics.gg/tft/team-comps](https://app.mobalytics.gg/tft/team-comps)。发现这个网站采用动态渲染的方法，一般的爬虫框架就 Over 了。于是我查阅了相关资料，找到了一个 Selenium 框架，它是一个浏览器自动化库，常用于测试 web 应用程序，它可以用于任何需要自动与浏览器交互的任务。在 Node 中使用 Selenium-webdriver 还需根据当前电脑版本选择与当前谷歌浏览器版本一致的驱动，驱动下载链接：[https://chromedriver.chromium.org/downloads](https://chromedriver.chromium.org/downloads)

一些爬虫的细节，这里不就做展示。然后就是对数据做一些清洗，然后数据入库。

## 安装教程

![yunding98k](https://socialify.git.ci/OLIVERgZzy/yunding98k/image?description=1&font=Inter&forks=1&issues=1&language=1&owner=1&pattern=Signal&pulls=1&stargazers=1&theme=Dark)

全栈项目使用 Docker compose 在本地与 Nginx, Hexo, MySQL, Node 运行在 Docker 中。供大家学习与参考。
GitHub: [https://github.com/OLIVERgZzy/yunding98k](https://github.com/OLIVERgZzy/yunding98k)

### 准备

在运行前你需要做一些事情

- api-service 接口服务
  - 修改数据库账户密码，搜索：`你数据库的密码`、`你数据库的账户` 并替换为你自己的数据库账户密码
  - 执行 `i-love-auto-chess.sql` 脚本 初始化数据库和数据
- miniapp 小程序
  - 将 `miniapp/images` 整个文件夹上传到你自己的小程序云开发的 CDN 中，并修改 `app.ts` 中 关于云开发相关配置
  - 修改 `miniapp/utils/http.ts` 中的接口地址 `BASE_URL`
- reverse-proxy 反向代理
  - 将 `nginx.conf` 中的 url 配置成你自己的网站域名
  - 如需开启 https 则需生成对应的网站证书
- web-blog 博客
  - 根据 `/web-blog/themes/hipaper/README.cn.md` 提供的教程修改你自己的博客配置

### 运行

假设您已经安装了 Docker 和 Docker Compose。为了开始，请确保将此项目克隆到 Docker 主机上。在主机上创建目录。

```bash
git clone https://github.com/OLIVERgZzy/yunding98k.git
```

一旦你克隆了项目到你的主机，我们现在可以开始我们的演示项目。轻松！导航到克隆项目所在的目录。从此目录运行以下命令

```bash
sudo docker-compose build
sudo docker-compose up -d
```
