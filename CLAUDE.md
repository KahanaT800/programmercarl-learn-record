# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在此仓库中工作时提供指引。

## 项目概述

代码随想录算法刷题仓库，同时作为每日打卡博客使用。每天的打卡记录以 Markdown 文件形式保存。

## 项目结构

- `README.md` — GitHub 仓库首页说明
- `hugo.toml` — Hugo 配置文件
- `template.md` — 每日打卡模板
- `content/days/` — 存放每日打卡文件（如 `day01.md`、`day02.md`）
- `themes/PaperMod/` — Hugo PaperMod 主题（git submodule）
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
3. 填写题目信息
4. 推送后 GitHub Actions 自动构建部署
