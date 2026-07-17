---
name: "work-daily-report"
description: "Creates structured work daily reports. Invoke when user asks to write work daily report, daily log, or daily summary."
---

# Work Daily Report

This skill helps users create structured work daily reports, including:
- Daily work content (tasks completed, progress, achievements)
- Learning content (new knowledge, skills acquired)
- Future outlook (plans, goals, next steps)

## Usage

When user asks to write a work daily report, follow this structure:

### Report Format

```
📅 Date: [YYYY-MM-DD]

## 今日工作内容
- [Task 1]: [Description and progress]
- [Task 2]: [Description and progress]
- [Task 3]: [Description and progress]

## 今日学习内容
- [Learning item 1]: [What was learned]
- [Learning item 2]: [What was learned]

## 未来展望
- [Plan 1]: [Future task or goal]
- [Plan 2]: [Future task or goal]
- [Plan 3]: [Future task or goal]
```

### Instructions

1. Prompt user for today's work content if not provided
2. Prompt user for today's learning content if not provided
3. Prompt user for future plans if not provided
4. Organize information into the structured format above
5. Add date automatically
6. Keep each section concise and actionable

### Example

```
📅 Date: 2026-07-17

## 今日工作内容
- 完成用户认证模块开发：实现 JWT token 生成和验证逻辑
- 修复订单页面响应式布局问题：调整媒体查询断点
- 代码审查：review 了 3 个 PR，提出 5 个改进建议

## 今日学习内容
- 学习 React Server Components 概念：了解 RSC 与传统组件的区别
- 阅读《代码整洁之道》第 3 章：关于命名的最佳实践

## 未来展望
- 明天继续开发用户权限管理功能
- 下周计划学习 Next.js App Router
- 长期目标：提升系统性能，优化数据库查询
```
