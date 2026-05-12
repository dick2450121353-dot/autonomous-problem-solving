     1|---
     2|name: autonomous-problem-solving
     3|description: "Resource-aware autonomous execution engine — text-driven memory loop + dynamic resource discovery + active research. Search before act when uncertain."
     4|version: "5.0"
     5|tags: [agent, autonomous, memory-loop, resource-aware, research-driven]
     6|author: "Hermes Agent / Nous Research"
     7|---
     8|
     9|## Core: Text-driven memory loop / 核心：文本驱动记忆闭环
    10|
    11|All tasks revolve around **documents**. Documents are memory, not decoration.
    12|所有任务围绕**文档**运转。文档是记忆，不是装饰。
    13|
    14|```
    15|Search experience → Write PLAN → Execute → Update docs → Reflect → Sink
    16|  ↑                                                              ↓
    17|  └──────────── Auto-retrieve next time ←───────────────────────┘
    18|
    19|搜经验 → 写PLAN → 执行 → 更新文档 → 反思 → 沉淀
    20|  ↑                                          ↓
    21|  └────────── 下次任务自动检索 ←──────────────┘
    22|```
    23|
    24|## Step 0: Judge complexity / 预判复杂度
    25|
    26|Decide search depth before acting:
    27|任务进来先判断，决定搜索深度：
    28|
    29|- **Simple** (single file/command, clear direction) → Skip to Step 3
    30|  简单（单文件/单命令/方向明确）→ 跳到步骤3
    31|- **Medium** (multi-file, clear but has dependencies) → Step 1 local + Step 2 skills
    32|  中等（多文件/方向明确但有依赖）→ 步骤1本地 + 步骤2匹配skill
    33|- **Complex** (uncertain/multi-component/unseen) → Full flow: search → research → execute
    34|  复杂（不确定/多组件/没见过的问题）→ 全流程：搜索 → 研究 → 执行
    35|
    36|## Step 1: Search experience (local) / 搜经验（本地）
    37|
    38|```
    39|1. search_files("keywords", path="/root/notes/")   ← Experience base
    40|2. session_search("keywords")                       ← Past sessions
    41|3. search_files("keywords", path="project_root")    ← Project source
    42|```
    43|
    44|Found experience → Read it, skip known failures, reuse success paths.
    45|搜到经验 → 读取，跳过已知失败方法，复用成功路径。
    46|
    47|## Step 2: Search resources (extended) / 搜资源（扩展）
    48|
    49|For medium+ tasks, continue searching available resources:
    50|中等以上任务，继续搜索可用资源：
    51|
    52|```
    53|4. skills_list() → match relevant skills → skill_view() to load
    54|5. tavily-search / web_search("problem keywords")   ← Web solutions
    55|6. GitHub API search related repos/code/issues      ← Open source reference
    56|```
    57|
    58|**Resource evaluation**: Not all useful. Rank by relevance, quality, cost. Pick Top N.
    59|**资源评估**：不全有用，按相关性、质量、成本排序，选Top N。
    60|
    61|**Not enough?** Change keywords, change channels. Don't guess, search.
    62|**搜索不够？** 换关键词、换渠道继续搜。不要猜，要查。
    63|
    64|## Step 3: Write PLAN.md / 写 PLAN.md
    65|
    66|Required for every non-simple task. Five mandatory fields:
    67|每个非简单任务必须写。五项必填：
    68|
    69|```markdown
    70|# PLAN: [Task name]
    71|
    72|## Background / 背景
    73|What user wants, one sentence.
    74|
    75|## Success criteria / 成功标准
    76|What counts as "done". Verifiable conditions, list clearly.
    77|
    78|## Available resources / 可用资源
    79|- Experience: [from Step 1]
    80|- Skills: [from Step 2]
    81|- References: [from web/GitHub]
    82|
    83|## Method / 方法
    84|What technical approach, why this one.
    85|
    86|## Steps (with time tracking) / 方案（含用时）
    87|1. [ ] Step 1 — Est: Xmin — Actual: __min
    88|2. [ ] Step 2 — Est: Xmin — Actual: __min
    89|3. [ ] Step 3 — Est: Xmin — Actual: __min
    90|   Total: __min est — __min actual
    91|
    92|## Iteration count / 迭代计数
    93|Current round __ (max 10 rounds, then change direction)
    94|
    95|## Progress/feedback / 进度/反馈
    96|(Updated during execution)
    97|```
    98|
    99|## Step 4: Execute / 执行
   100|
   101|Default: execute immediately after PLAN. Only wait for user confirmation if user explicitly asks to review the plan first.
   102|默认：PLAN写完直接执行。仅当用户主动要求先看PLAN时才等待。
   103|
   104|Core: Use docs to correct yourself, not short-term memory.
   105|核心：用文档纠正自己，不是靠短期记忆。
   106|
   107|Each step follows:
   108|每步执行遵循：
   109|```
   110|Read PLAN for direction → ACT → VERIFY → Update PLAN progress
   111|读PLAN确认方向 → ACT → VERIFY → 更新PLAN进度
   112|```
   113|
   114|- Update PLAN.md progress immediately after each step
   115|  每步完成后立刻更新 PLAN.md 的进度
   116|- Long operations: report every minute — current step, what was done, result, next step
   117|  长时间操作每分钟反馈：当前步骤、做了什么、结果、下一步
   118|- Verify failed → Record failure, go to Step 5
   119|  验证失败 → 记录失败现象，进入步骤5
   120|
   121|## Step 5: Failure handling / 失败处理
   122|
   123|**Reflection (Reflexion)**: After failure, write clearly:
   124|**反思**：失败后写清楚：
   125|
   126|- **Symptom**: Exact error message/output
   127|  现象：具体错误信息/输出
   128|- **Root cause**: Wrong method? Wrong environment? Wrong execution?
   129|  根因：方法本身错？环境不对？执行有误？
   130|- **Distinguish**: Change method vs. same method, different params
   131|  区分：换个方法 vs 同一方法换参数
   132|
   133|**Escalation (10-round grading)** / 升级（10轮分级）：
   134|```
   135|Rounds 1-3:  Same method, tune params        同方法微调参数
   136|Rounds 4-6:  Completely different method       换完全不同的方法
   137|Rounds 7-9:  Different stack/framework/tool    换技术栈/框架/工具
   138|Round 10:    Stop. Write experience doc.       停止，写经验文档，报告用户
   139|```
   140|
   141|**Iron rule**: Failed methods recorded in docs must NEVER be retried. Catch yourself reusing one → stop immediately.
   142|**铁律**：文档里记录过的失败方法，绝对不再尝试。发现又在用同一个方法，立刻停下来。
   143|
   144|## Step 6: Verification gate / 验证门控
   145|
   146|**Iron rule: No evidence, no claim of completion.**
   147|**铁律：没有验证证据禁止声称完成。**
   148|
   149|Self-check before claiming done:
   150|验证前的自问：
   151|1. What command/operation proves success? / 什么命令/操作能证明成功？
   152|2. Did I run it? See full output? / 我运行了吗？看到完整输出了吗？
   153|3. Does output actually prove success? (Not "should work") / 输出确实证明成功了吗？
   154|
   155|Verification types / 验证类型：
   156|- Code: Run command + check output + exit code
   157|  代码：运行命令 + 检查输出 + exit code
   158|- File: stat + read back content
   159|  文件：stat + read back 内容
   160|- Network: fetch URL + check status code
   161|  网络：fetch URL + 检查状态码
   162|- Process: Check alive + must kill after test
   163|  进程：检查存活 + 测试完必须杀掉
   164|- External: Must verify side effects (HTTP status, file exists)
   165|  外部操作：必须验证副作用（HTTP状态码、文件存在）
   166|
   167|Report with evidence (command output/file content/status code). Never say "it should work".
   168|验证后报告：带证据（命令输出/文件内容/状态码），不说"应该成功了"。
   169|
   170|Banned words / 禁止词：应该、可能、大概、probably、should work
   171|
   172|## Step 7: Experience sink / 经验沉淀
   173|
   174|After task ends, write to `/root/notes/YYYY-MM-DD-keywords.md`:
   175|任务结束，写入 `/root/notes/YYYY-MM-DD-关键词.md`：
   176|
   177|```markdown
   178|## Summary / 总结
   179|- What worked / 什么方法有效
   180|- What didn't (+ root cause) / 什么方法无效（附根因）
   181|- Key pitfalls / 关键陷阱和避坑指南
   182|- Reusable code/command templates / 可复用的代码/命令模板
   183|- New resources discovered / 发现的新资源（skill/工具/参考）
   184|```
   185|
   186|For complex tasks that succeeded, consider saving as skill (`skill_manage`).
   187|复杂任务成功后，考虑保存为 skill（`skill_manage`）。
   188|
   189|## Tool mapping / 工具映射
   190|
   191|| Step | Tool | Purpose |
   192||------|------|---------|
   193|| Search experience | `search_files` | Search local experience base and project source |
   194|| Search sessions | `session_search` | Search past conversations |
   195|| Search skills | `skills_list` / `skill_view` | Discover and load skills |
   196|| Search web | `tavily-search` / `web_search` | Search web solutions |
   197|| Search code | GitHub API | Search open source references |
   198|| Write PLAN | `write_file` | Create process document |
   199|| Update PLAN | `patch` | Incremental doc updates |
   200|| Execute | `terminal` / `execute_code` | Run commands and scripts |
   201|| Task tracking | `todo` | Checklist tracking |
   202|| Parallel | `delegate_task` | Multiple independent subtasks |
   203|| Sink | `write_file` / `skill_manage` | Save experience / create skill |
   204|