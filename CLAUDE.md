# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指引。

## 项目概述

「木人的CS学习笔记」——算法刷题打卡 & CS 学习笔记博客。包含代码随想录每日打卡、算法深度笔记、操作系统八股深入等内容。

博客地址：https://kahanat800.github.io/programmercarl-learn-record/

## 构建

- 本地预览：`hugo server -D`
- 构建：`hugo --minify`
- 推送到 main 分支后 GitHub Actions 自动部署

## 项目结构

- `README.md` — GitHub 仓库首页说明
- `hugo.toml` — Hugo 配置文件
- `template.md` — 每日打卡模板
- `content/` — 博客内容
  - `days/` — 每日打卡文件（如 `day01.md`、`day02.md`）
  - `notes/algo/` — 算法深度笔记（如 KMP 详解、Floyd 判环）
  - `notes/os/` — OS 八股深入笔记（序号前缀排列，如 `01-操作系统的本质认识.md`、`02-从一切皆文件到IO多路复用.md`）
- `drafts/` — 按代码随想录章节预写的题目草稿（不会被 Hugo 构建）
  - `题目模板.md` — 单题模板
  - `01-数组/` ~ `12-图论/` — 12 个章节目录，每个文件以序号前缀排列（如 `01-704-二分查找.md`），顺序与代码随想录课程一致
- `themes/PaperMod/` — Hugo PaperMod 主题（git submodule）
- `layouts/` — Hugo 模板覆盖
  - `index.html` — 自定义首页（按板块分栏展示）
  - `_default/terms.html` — 标签页（按数据结构/算法技巧/其他分组）
  - `_default/rss.xml` — RSS 修复
  - `partials/extend_head.html` — KaTeX 公式、字体引入
- `assets/css/extended/custom.css` — 自定义样式（卡片式条目、首页布局、标签分组、代码块美化）
- `.github/workflows/deploy.yml` — GitHub Actions 自动部署（含旧 deployment 清理）

## 导航结构

- 打卡记录 → `/days/`
- 算法笔记 → `/notes/algo/`
- OS 八股深入 → `/notes/os/`
- 标签 → `/tags/`（按数据结构、算法技巧、其他分组）

## 标签体系

- **数据结构**：数组、链表、哈希表、字符串
- **算法技巧**：二分查找、双指针、滑动窗口、前缀和、快慢指针、KMP
- **其他**：操作系统
- 新增标签时需同步更新 `layouts/_default/terms.html` 中的分组列表

## 编程语言

主要语言：C++

## 约定

- 打卡文件标题格式：`代码随想录打卡第X天 | 题目1、题目2`
- 每道题包含：力扣/KamaCoder 链接、代码随想录文章链接、B站视频链接、完成状态
- 完成状态标记：✅ 独立完成 / ⚠️ 看题解后完成 / ❌ 未完成
- 每道题记录：第一想法、看完题解后的想法、实现中遇到的困难、代码
- 末尾写今日收获
- 所有说明和注释使用中文

## 新建打卡流程

1. 复制 `template.md` 到 `content/days/dayXX.md`
2. 填写 front matter 中的 title、date、tags
3. 从 `drafts/` 对应章节中粘贴已预写的题目内容
4. 推送后 GitHub Actions 自动构建部署

## 预写题目流程

1. 在 `drafts/` 对应章节目录中找到题目文件（已按课程顺序预建，含力扣链接、文章链接、视频链接）
2. 填写想法和代码
3. 等老师布置当天任务后，将内容粘贴到 `content/days/dayXX.md` 中

## 笔记流程

1. 在 `content/notes/<学科>/` 下新建 `.md` 文件，填写 front matter 和正文
2. 新增学科直接加子目录（如 `content/notes/network/`），并建 `_index.md`
3. 深度分析可从 drafts 中提取为独立博客文章（如 KMP 详解从 strStr 草稿中提取）

## 笔记约定

- 语气平易近人，像在对话，不要教科书式叙述
- 关键论断附权威参考链接（kernel.org、man pages、RFC、LKML/LWN 等）
- OS 笔记写作时需主动搜索网上资源（特别是 Linux 内核源码、man pages、LKML 邮件列表、LWN 文章）确保技术细节准确
- 展示映射/对照关系用 Markdown 表格，不要用代码块（代码块等宽字体渲染中文很丑）
- 代码块只用于真正的代码

## drafts 文件命名规则

- 格式：`序号-题号-题目名.md`，如 `01-704-二分查找.md`
- KamaCoder 题目：`序号-KM题号-题目名.md`，如 `01-KM98-所有可达路径.md`
- 序号按代码随想录课程顺序排列，`ls` 即为学习顺序
