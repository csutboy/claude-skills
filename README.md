# Claude Skills 收藏

我的 Claude Code 自定义技能（Skills）合集。每个 skill 是一个结构化的提示词框架，安装后可以让 Claude 在特定场景下按照固定流程工作。

---

## 技能列表

### 🔮 bazi-ai-consultant — 八字 AI 命理顾问

用"强化学习"的反馈校准思路，让 AI 算八字算得更准。

**核心思路**：八字符号是多义的（比如"财星"可以代表金钱、配偶或父亲），AI 第一次直接算通常很泛。通过三轮反馈校准，让 AI 先学会"在你这张命盘里，这些符号具体指什么"，再做预测。

**三阶段流程**：

| 阶段 | 内容 |
|------|------|
| 第一阶段：排盘 | 生成四柱八字、大运表、流年，并交叉验证起运年龄 |
| 第二阶段：校准 | 三轮反馈（过去事件 → 当前特征 → 感情历史），用 ✓/✗ 纠正 AI 的理解 |
| 第三阶段：预测 | 基于校准后的理解，预测未来配偶特征、十年大运走势、关键年份节点 |

**包含内容**：
- 每个阶段的完整中文提示词（可直接复制到 DeepSeek / Claude / GPT）
- 起运年龄交叉验证方法（推荐 3 个专业排盘网站）
- 反馈格式模板

---

## 如何安装使用

### 方式一：通过 Claude Code 安装

```bash
# 克隆本仓库
git clone https://github.com/csutboy/claude-skills.git

# 将 skill 文件夹复制到 Claude Code skills 目录
cp -r claude-skills/bazi-ai-consultant ~/.claude/skills/
```

### 方式二：手动安装

1. 下载 `bazi-ai-consultant/SKILL.md`
2. 放入 `~/.claude/skills/bazi-ai-consultant/SKILL.md`
3. 重启 Claude Code，输入「帮我算八字」即可触发

---

## 使用示例

安装后，在 Claude Code 中直接说：

- `帮我算八字，我是1990年3月15日早上8点生的，男，上海`
- `用 AI 分析我的命盘`
- `我想做一个完整的八字推算`

Claude 会自动启用这个 skill，引导你走完完整的三阶段流程。

---

## 关于 Skills

Claude Code 的 Skills 功能允许你把常用的复杂工作流打包成可复用的模块。Claude 会根据你说的话自动判断是否需要调用某个 skill。

更多信息参考：[Claude Code 文档](https://docs.anthropic.com/claude-code)
