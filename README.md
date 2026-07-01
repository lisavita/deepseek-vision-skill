#ltysb
# deepseek-vision-skill

DeepSeek 作为 Claude Code CLI 的模型供应商时只有纯文本能力，这个 skill 通过 Playwright 操控 DeepSeek 网页版聊天（chat.deepseek.com），让文本模型的 Claude 也能用上 DeepSeek 的识图功能。

DeepSeek is text-only as a model provider for Claude Code CLI. This skill bridges the gap by driving DeepSeek's web chat via Playwright, giving Claude access to DeepSeek's vision capabilities.

## 能做什么

- OCR：提取图片中的文字、表格数据
- 排版审查：检查 LaTeX/PPT/设计稿的布局、对齐、重叠
- 图表分析：验证报表数据是否对得上、图表与数据是否一致
- 设计评审：配色、信息层级、视觉一致性
- 多轮追问：同一张图片上持续细化分析，直到问题彻底解决

## 安装

复制到 Claude Code 的 skills 目录：

```bash
mkdir -p ~/.claude/skills/deepseek-vision
cp SKILL.md ~/.claude/skills/deepseek-vision/
```

## 依赖

- [Playwright](https://playwright.dev/) (`pip install playwright`)
- 一个已登录 DeepSeek 的浏览器 profile（首次运行时会提示手动登录）

## 用法

直接跟 Claude 说需要分析哪张图片，Claude 会自动判断需不需要路由到 DeepSeek。不需要手动指定——如果图够清晰、Claude 自己能处理，就不会绕路。

```
"帮我看看这张报表的数据有没有对不上的地方"
"检查一下这个UI截图里的按钮对齐和间距"
"这张LaTeX编译出来的PDF截图，有没有文字和图片重叠？"
```

## 工作原理

```
Claude 读取图片 → 看不清/排版/图表/OCR？→ Playwright 打开 DeepSeek 网页 → 上传图片 → 精确提问 → 多轮追问 → 返回结果
                  ↓
              看得清 → Claude 直接回答，不绕路
```
