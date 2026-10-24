---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot 应用配置 Pages 工作流、评审拉取请求，并验证太空问答网站已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 为太空问答部署一个公开访问地址。Copilot 可以准备工作流并引导部署，但你仍需负责评审更改并确认网站已上线。

在本课中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前评审工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

太空问答已经位于 GitHub 存储库中，但其他人还没有便捷的体验方式。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖即可通过公开 URL 访问。

## 让 Copilot 部署网站

在 `space-quiz` 的 Copilot 应用会话中发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；GitHub Actions 中成功运行的 Deploy website 工作流，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及已发布到 yourname.github.io/space-quiz/ 的太空问答网站。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求代理创建并合并拉取请求，以及更改存储库的 Pages 设置。继续之前，请评审差异和部署权限，并检查 GitHub 上的审批提示。

## 评审工作流并验证部署

1. 确认仅当存储库中尚无 README 时才添加该文件。
2. 评审拉取请求中的更改，包括工作流的构建和部署步骤。
3. 仅在所需的拉取请求检查通过后合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认太空问答正常加载。

如果代理无法启用 Pages 或合并拉取请求，请按其提供的具体说明操作。只有在部署成功且网站可以正常访问后，才能将任务视为完成。

## 总结和后续步骤

太空问答现已作为网站上线，你也亲自验证了部署，而非只依赖代理的报告。接下来继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
