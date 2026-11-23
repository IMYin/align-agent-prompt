# align-agent-prompt

把模糊需求对齐成可直接交给执行 Agent 的任务 Prompt。

这个技能先梳理目标和约束，再针对关键缺口提问；用户明确确认后，输出一份可复制的 Markdown Prompt。它负责需求对齐，**不代替执行 Agent 完成任务**。

## 工作方式

1. 复述需求，区分已知信息和待确认事项。
2. 只追问显著影响执行结果的问题。
3. 整理目标、输入、范围、交付物、验收标准和禁止事项，等待用户明确确认。
4. 确认后，在代码围栏中输出完整 Prompt，方便一键复制。

需求已经清晰时可以跳过不必要的追问，但仍需确认。确认后若仅调整输出格式，不会要求重新确认；若需求发生实质变化，则重新对齐。

## 安装

需要 Git 和支持 Agent Skills 的客户端。以下命令适用于 macOS、Linux 或其他提供 POSIX shell 的环境。先检查目标目录不存在，避免覆盖已有技能；如已存在，请先检查并自行备份或删除。

### GitHub Copilot / Codex：个人安装

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/IMYin/align-agent-prompt.git ~/.agents/skills/align-agent-prompt
```

GitHub Copilot 在 VS Code 的本地会话也支持 `~/.copilot/skills/`。不同客户端和会话类型支持的目录可能不同，请参考各自文档。

### Claude Code：个人安装

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/IMYin/align-agent-prompt.git ~/.claude/skills/align-agent-prompt
```

### 仅安装到当前项目

在目标项目根目录执行以下命令。这里以 GitHub Copilot 为例：

```bash
mkdir -p .github/skills
git clone https://github.com/IMYin/align-agent-prompt.git .github/skills/align-agent-prompt
```

这会创建一个嵌套 Git 仓库；如果希望把技能作为普通文件提交到自己的项目，请在检查内容后仅将其中的 `SKILL.md` 复制到目标项目的 `.github/skills/align-agent-prompt/`，不要提交嵌套的 `.git` 目录。其他客户端请使用其支持的项目技能目录。

安装后，必要时重新启动客户端或开启新会话，让客户端重新发现技能。个人安装通过 `git -C ~/.agents/skills/align-agent-prompt pull --ff-only` 更新；其他安装位置替换对应路径。更新前请先检查本地修改。

## 使用

在 GitHub Copilot 或 Claude Code 的聊天中输入：

```text
/align-agent-prompt 帮我整理一个用于制作电子墨水屏 demo 的执行任务。
```

在 Codex CLI 或 IDE 中，可以输入 `$` 并选择 `align-agent-prompt`，再附上需求。

按提示回答关键问题；确认对齐清单后，复制最终的 Markdown Prompt，发送给执行 Agent。也可以在调用后继续补充材料或修改需求。

技能声明了 `disable-model-invocation: true`，支持该配置的客户端只会在用户手动调用时加载。不同客户端对配置和调用方式的支持可能不同；如果技能未出现，请检查安装位置、开启新会话，并查看客户端文档。

## 文件

- [`SKILL.md`](SKILL.md)：技能元数据和完整工作流程。
- [`LICENSE`](LICENSE)：MIT 许可证。

参考：[VS Code Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)、[Claude Code Skills](https://code.claude.com/docs/en/skills)、[Codex Agent Skills](https://developers.openai.com/codex/skills/)。

## 许可证

[MIT](LICENSE)
