# syb-skills

Claude Code 与 subagent harness 的 agent skill 集合。

## Skills

### `subagent-orchestration`

以主 Agent（协调者）身份拆解并向 subagent 派发多步骤工程任务：拆段防失控、文件白名单互斥并行、证据抽验、失败三选一处置、变异验证。

两套 harness 的版本内容一致，只有「子代理选型 / 派发方式 / 超时现实」三节按各自机制改写：

| 目录 | 适用 | 说明 |
|---|---|---|
| [`subagent-orchestration/claude-code/`](subagent-orchestration/claude-code/SKILL.md) | Claude Code | 使用 `Agent` 工具；按任务形态选 `fork` / `general-purpose` / `Explore` / `Plan` |
| [`subagent-orchestration/agents-harness/`](subagent-orchestration/agents-harness/SKILL.md) | 其他 subagent harness | 使用固定 `worker` 角色、`timeoutMs` 与派发脚本 |

## 安装

**Claude Code** —— 把目录放到 skill 搜索路径之一：

```bash
# 项目级（仅当前仓库）
git clone https://github.com/Syb-coder/syb-skills.git /tmp/syb-skills
cp -r /tmp/syb-skills/subagent-orchestration/claude-code ~/.claude/skills/subagent-orchestration

# 或用户级为全局可用：~/.claude/skills/
```

重启会话后 skill 出现在列表中，可用 `/subagent-orchestration` 调用。

**其他 harness** —— 复制 `subagent-orchestration/agents-harness/SKILL.md` 到该 harness 的 skill 目录，保持 frontmatter 的 `name` 与目录名一致。

## 核心主张

- **拆段**：单次派发 ≈ 1 个新文件 + 其守卫测试 + 1-2 条定向命令。叠加「改全部调用方」或「跑全量门禁」是失控主因。
- **白名单互斥**：并行写入任务按文件白名单隔离；派发前跑四类检索（守卫 / 类型 / 夹具 / 连带死代码）补齐白名单——这是最贵的返工源。
- **抽验**：子代理的总结是意图不是事实，采纳前读 diff。
- **变异验证**：守卫/测试是否真在拦，只能靠彻底移除匹配目标来证伪。加后缀、删冗余判定都是无效变异。
- **fail-closed**：把「无法验证」当「不安全」，把「无法确认」当「未完成」，不用猜测填补证据缺口。

## License

MIT —— 见 [LICENSE](LICENSE)。
