# skills

English | [简体中文](README.zh.md)

A shared repository for compatible skills.

## Skills

| Skill | Version | Description |
| --- | --- | --- |
| `task-generate` | `0.1.0` | Generates or revises evidence-grounded task documents, implementation plans, and task lists. |

## Directory Structure

Each skill lives under `skills/<skill-name>/`, with `SKILL.md` as its entry file.

The `SKILL.md` frontmatter must include:

- `name`
- `description`
- `metadata.short-description`
- `metadata.version`

`metadata.version` uses SemVer. The first shared version starts at `0.1.0`; increment the major version for incompatible behavior changes, the minor version for compatible new capabilities, and the patch version for documentation or small fixes.

## Current Contents

- `skills/task-generate/SKILL.md`
- `skills/task-generate/references/template.zh.md`
- `skills/task-generate/references/template.en.md`
- `skills/task-generate/agents/openai.yaml`
