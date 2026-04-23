---
description: 使用AI工具开发时，必须要遵循的规则
version: 1.0
author: zengxt
globs:
  - "**/*.java"
  - "**/*.sh"
  - "**/*.sql"
  - "**/*.yml"
  - "**/*.yaml"
  - "**/*.env"
  - "**/*.xml"
alwaysApply: true
---

# AI工具约束规则

> **核心原则**：推荐的可以主动做，不推荐不要自动去做，其它的需要申请。

## 适用场景

- 场景一：git 操作代码仓库
- 场景二：Database 操作

## 规范内容

### DO（推荐做法）
- 拉取（pull）代码
- select 操作

### DON'T（禁止做法）
- git
	- 推送（push）代码
- Database
	- UPDATE
	- REPLACE
	- MERGE
	- ALTER
	- DELETE
	- TRUNCATE
	- DROP