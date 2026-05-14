# skills

[English](README.md) | 简体中文

共享的兼容 skills 仓库。

## Skills

| Skill | Version | Description |
| --- | --- | --- |
| `task-generate` | `0.1.0` | 生成或修订基于真实工作区证据的任务文档、实施计划和任务列表。 |

## 目录结构

每个 skill 都放在 `skills/<skill-name>/` 下，入口文件为 `SKILL.md`。

`SKILL.md` 的 frontmatter 必须包含：

- `name`
- `description`
- `metadata.short-description`
- `metadata.version`

`metadata.version` 使用 SemVer 格式。首次共享版本从 `0.1.0` 开始；发布不兼容行为变更时提升主版本，新增兼容能力时提升次版本，仅修复文档或小问题时提升补丁版本。

## 当前内容

- `skills/task-generate/SKILL.md`
- `skills/task-generate/references/template.zh.md`
- `skills/task-generate/references/template.en.md`
- `skills/task-generate/agents/openai.yaml`
