---
title: 谈谈我的「数字文具盒」 - Obsidian
description: 如何将Obsidian与Docusaurus有机结合，以实现知识管理和网站展示的目标。
keywords:
  - obsidian
  - Docusaurus
tags:
  - 生产力工具/Obsidian
  - 开源项目/数字文具盒
  - 博客/原创
authors:
  - 7Wate
date: 2022-11-30
---

这篇关于 Obsidian 是生产力工具的终篇了，因为目前涉及 Obsidian 的文章特别多，所以我就不啰里啰唆叙述重复的文字了。本文主要涉及到 Obsidian 和 Docusaurus 如何进行有机的结合。

## Obsidian

![screenshot-1.0-hero-combo](https://static.7wate.com/img/2022/11/30/02c7a7cf1ab33.png)

> Obsidian is a powerful and extensible knowledge base that works on top of your local folder of plain text files.

[Obsidian](https://obsidian.md/) 是一款基于本地文件夹保存的 Markdown 文件的强大知识库，旨在打造你的第二大脑！简单来说 Obsidian 提供的功能和语雀、有道笔记等笔记软件的核心功能都是一致的，但是 Obsidian 作为一款开源产品，其插件系统提供了丰富的功能和可拓展性。

Obsidian 的正式 1.0.0 版本于 2022 年 10 月 13 日正式发布。Obsidian 虽然作为一款很新的产品，但是却拥有一众信徒。Google 上关于 Obsidian 的搜索词条高达 2000w 条，同时各种各样的入门教程更是层出不穷。

在此主要介绍几个我认为特别好用的插件：模板、 日记、Git、XMind 思维导图。每个人对于 Obsidian 的需求和用法大同小异，但终归都是为了高效的记录笔记，切勿折腾本末倒置。适合自己的就是最好的！

### 模板

Obsidian 核心插件提供了基础的模板，第三方市场也提供了功能更加丰富的模板插件。模板插件对于文章的元信息，日记的固定模板、周报的固定模板、知识文章的快速生成提供了便捷，同时提高了效率。

#### 文章元信息

```
title: 谈谈我的「数字文具盒」 - Obsidian
description: 谈谈我的「数字文具盒」 - Obsidian
keywords:
- 数字文具盒
- 生产力工具
tags: 
- 数字文具盒
authors:
- 7Wate
date: 2022-11-30
```

#### 日记模板

```
# <% tp.date.now() %>

## Info

| Date           | Weather      | Moon |
| -------------- | ------------ | ---- |
| <% tp.date.now("ddd HH:mm") %> | <% tp.web.daily_weather("郑州","zh") %> | <% tp.web.daily_weather("郑州","zh","?format=%m") %> |

## Daily

<% tp.web.daily_poetry() %>


## Habits

- [ ] 早睡早起 🌃
- [ ] 健康饮食 🥗
- [ ] 多喝热水 ☕️
- [ ] 保持运动 💪

## To-do List

- [ ] 阅读资讯 📺
- [ ] 每日必做 ✨
- [ ] 今日读书 📖
- [ ] 今日计划 ✏
- [ ] 今日分享 📌

## Notes
```

Markdown 文件不同于其他 Word、语雀等提供了元信息，所以这里需要手动维护便于查阅和展示。子弹笔记也可以通过日记模板快速实现，不必拘泥于种种繁琐的细节。我使用 Obsidian 的模板功能主要是方便和 Docusaurus 进行有机的结合。

### 日记

![note](https://static.7wate.com/img/2022/12/12/270218ad42e33.png)

日记功能则是由 Calendar、Periodic Notes、Templater 共同组合实现的。简单来说就是通过 Calendar 插件（右侧日历）点击任意一天，即可通过 Periodic Notes 快速创建至对应文件夹并且生成文件名，然后 Templater 模板插件快速生成今日信息（自定义模板），最后简单写下今日总结即可实现一份完美的日报。

### Git

Obsidian 提供了多种同步插件，但是作为一个 Coder，我还是喜欢 Git。

![image-20221212212342409](https://static.7wate.com/img/2022/12/12/48efe8ded31ec.png)

### XMind 思维导图

Obsidian 的 XMind 思维导图插件在写完文章后可以快速生成思维导图。

![image-20221212212710645](https://static.7wate.com/img/2022/12/12/f5f1f8110c07f.png)

## Docusaurus

![Docusaurus](https://static.7wate.com/img/2022/12/13/e386a7a88d66f.png)

[Docusaurus](https://www.docusaurus.io/) 是 Facebook 的一款开源产品，Docusaurus 能够帮助你快速创建并发布**美观的文档网站**。因为 Obsidian 的开源版本不提供 web 展示，所以我借助 Docusaurus 实现了知识库的 web 展示。

平时在 Obsidian 客户端的帮助下，我可以随时随地快速的记录知识。Obsidian 提供的 Git 插件，方便我同步到远程 Git 仓库，最后使用 Github 等第三方服务进行自动构建部署。

在工作学习中，打造个人的 wiki 知识库是必不可少的，其在经过不断的积累后可以产生质的变化。总的来说 Obsidian 和 Docusaurus 并不是每个人的最优选，我前期刚开始使用的时候也是好一顿折腾，经历了一段时间的使用后才慢慢形成生产力。

## 结语

![字数](https://static.7wate.com/img/2022/12/13/69ea12a01d73f.png)

至此生产力工具篇结束，整个「数字文具盒」系列也算进入了终篇。目前还有两篇文章，关于心得体会和网络环境的。希望这 2w 字的「数字文具盒」对你有所启发和帮助！
