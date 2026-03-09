# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指引。

## 项目概述

代码随想录算法刷题仓库，同时作为每日打卡博客使用。每天的打卡记录以 Markdown 文件形式保存。

博客地址：https://kahanat800.github.io/programmercarl-learn-record/

## 构建

- 本地预览：`hugo server -D`
- 构建：`hugo --minify`
- 推送到 main 分支后 GitHub Actions 自动部署

## 项目结构

- `README.md` — GitHub 仓库首页说明
- `hugo.toml` — Hugo 配置文件
- `template.md` — 每日打卡模板
- `content/days/` — 存放每日打卡文件（如 `day01.md`、`day02.md`）
- `drafts/` — 按代码随想录章节预写的题目草稿（不会被 Hugo 构建）
  - `题目模板.md` — 单题模板
  - `01-数组/` ~ `12-图论/` — 12 个章节目录，每个文件以序号前缀排列（如 `01-704-二分查找.md`），顺序与代码随想录课程一致
- `themes/PaperMod/` — Hugo PaperMod 主题（git submodule）
- `layouts/` — Hugo 模板覆盖（RSS 修复、KaTeX/字体引入）
- `assets/css/extended/custom.css` — 自定义样式（内容宽度、代码块样式）
- `.github/workflows/deploy.yml` — GitHub Actions 自动部署

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

## drafts 文件命名规则

- 格式：`序号-题号-题目名.md`，如 `01-704-二分查找.md`
- KamaCoder 题目：`序号-KM题号-题目名.md`，如 `01-KM98-所有可达路径.md`
- 序号按代码随想录课程顺序排列，`ls` 即为学习顺序
