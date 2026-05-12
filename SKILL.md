---
name: autonomous-problem-solving
description: "自主问题处理方法论 — AI Agent 全程自主闭环，不重复错误，经验积累复用。融合 ReAct + Reflexion + Self-Refine + Plan-and-Solve + AutoGPT 精华。"
version: "2.0"
tags: [agent, autonomous, problem-solving, methodology, self-improvement]
author: "Hermes Agent / Nous Research"
---

## Autonomous Problem-Solving v2.0

### 何时使用

**用这个流程**：修bug、调试、部署、多步任务、不确定解法的问题
**不用这个流程**：简单问答、单步操作、信息查询

判断标准：任务需要 3+ 步骤 或 结果不确定 → 走流程

### 工具映射

| 步骤 | 工具 | 用途 |
|------|------|------|
| 搜经验 | `search_files` 搜 `/root/notes/` | 找历史方案 |
| 跨session回忆 | `session_search` | 找过去对话 |
| 写过程文档 | `write_file` / `patch` | PLAN.md |
| 进度追踪 | `todo` | checklist |
| 子任务 | `delegate_task` | 并行处理 |
| 验证 | `terminal` / `vision_analyze` | 测试结果 |

### 核心流程（精简版）

#### 0. 判断 + 定成功标准
- 这任务需要走流程吗？简单任务直接做
- 什么情况算"做完了"？列出可验证条件
- 写进 PLAN.md 的"成功标准"

#### 1. 搜经验
```
search_files(pattern="关键词", path="/root/notes/")
session_search("类似问题关键词")
```
找到 → 直接复用，跳过已知失败方法
没找到 → 继续

#### 2. 写 PLAN.md
```markdown
# [问题简述]
日期: YYYY-MM-DD

## 成功标准
- [ ] 条件1
- [ ] 条件2

## 环境
- OS/工具版本/路径

## 方案
1. 步骤1（预估 X 分钟）
2. 步骤2（预估 X 分钟）
总预估: X 分钟

## 进度
- [ ] 步骤1
- [ ] 步骤2

## 失败记录
（每次失败记录：做了什么→结果→根因）

## 总结
（完成后填写：什么有效、什么无效、避坑指南）
```

#### 3. 执行循环
每步：读 PLAN.md → 执行 → 验证 → 更新 PLAN.md → 报告进度

#### 4. 失败处理
失败时：
1. 写根因分析（不是"失败了"，是"为什么"）
2. 区分：方法错了？环境不对？执行有误？
3. **绝对不用文档里记录过的失败方法**

#### 5. 失败升级
| 轮次 | 行动 |
|------|------|
| 1-3 | 换参数/细节 |
| 4-6 | 换方法 |
| 7-9 | 换大方向 |
| 10 | 停止，报告用户：已尝试X种方法，建议Y |

#### 6. 质量自检
跑通后：
- [ ] 满足第0步的所有成功标准？
- [ ] 边界情况覆盖？
- [ ] 多余文件清理？
- [ ] 进程杀掉？（端口/后台进程）

#### 7. 总结沉淀
写进 PLAN.md 末尾 + `/root/notes/YYYY-MM-DD-问题关键词.md`：
- 有效方法 + 代码片段
- 无效方法 + 根因
- 避坑指南

### 文件规范

- 经验库：`/root/notes/`
- 命名：`YYYY-MM-DD-简短描述.md`
- 过程文档：`PLAN.md`（放在工作目录）
- **用文档驱动，不用短期记忆**

### v2.0 更新日志
- 删除与 SOUL.md 重复内容
- 增加工具映射表
- 增加失败升级机制（10轮分级）
- 增加"何时使用"判断
- 精简为可执行模板
