# Consolidation Procedure

> Phase 4 用。触发命令：`/parallel-ethan consolidate`
> 使用工具：`skill_manage patch`（增量）+ `skill_manage write_file`（归档）

---

## 0. Pre-flight

读取以下文件，构建工作集：

- `journal/*.md` 所有未归档的日志
- `references/patterns.md`（当前模式）
- `references/identity.md`（当前身份）
- `references/companions/subject-e.md`（漂移检测基线）

如果 journal 数量 < `consolidation_threshold_journals`（默认 5），
**询问用户是否继续**——可能数据不足。

---

## 1. drift 仲裁

每条 `[drift]` 标记走以下决策树：

```
是否在多个 journal 中都出现？
├── 否（单次）→ 标记为偶发，不动 patterns.md
└── 是（≥3 次）→
    └── 与 subject-e.md 比较：
        ├── subject-e 也有这个倾向 → 模式确认，更新 patterns.md（patch）
        └── subject-e 没有 → 这是真演化，进入"演化候选"区
            └── 询问用户：保留还是回退？
```

**关键：** 任何**结构层面**的 drift（不只是词汇）都需要用户审。
词汇增量可以自动，结构变化必须人审。

---

## 2. lexicon 升级

每条 `[lexicon-candidate]` 走计数：

```
跨 journal 累计 count ≥ 3？
├── 否 → 留在 patterns.md §10（候选区），等下次
└── 是 → 升级到 patterns.md §1.2，需要标注：
    - 情境化程度（按方法论 §4.2 测）
    - 频率分级（极高/高/中/低）
    - 首次出现的 journal 引用
```

**补充检查：** 升级前用方法论 §4.3（情境扎根测试）验证一遍。
把这个词放到一个完全不同的对话上下文里——还成立 → 泛化；不成立 → 情境化。

---

## 3. identity 增量

每条 `[context-hit]` 走以下流程：

```
引用的事实是否已在 identity.md？
├── 是 → 不动，记一笔"§五 伙伴 anchor 命中"
└── 否 →
    └── 跨 journal 累计命中 ≥ 3？
        ├── 否 → 写入 identity.md §三（待验证区）
        └── 是 → 提议升级到 §一/§二（基底事实）
            └── 必须人审。打印 diff 给用户。
```

---

## 4. boundary-event 评估

逐条检查 `judgment` 字段：

- `healthy` → 无操作，归档
- `drift` → 升级到第 1 步的 drift 流程
- `known-vuln` → 检查是否触发了修复尝试：
  - `did_repair_attempt: yes` → 加一条到 patterns.md §8 末尾，标"修复尝试 N 次"
  - `did_repair_attempt: no` → 提醒用户：漏洞复发未处理

---

## 5. trampoline 累积

`[trampoline]` 事件不进 patterns.md 也不进 identity.md。
单独写入 `references/trampoline-log.md`（如果不存在则创建）。

这个文件是 Parallel Ethan 的"输出价值"档案——它记录了 Ethan 的输出
被对方踩着去了哪些地方。Phase 0 不读这个文件，但用户可以人工查阅。

---

## 6. 归档

把已处理的 journal 文件移到 `journal/archive/`：

```
现在用 write_file + remove_file 模拟 mv：
1. write_file: journal/archive/<filename>.md（内容 = 原文件 + 归档标头）
2. remove_file: journal/<filename>.md
```

归档标头：
```markdown
> archived at: <ISO-8601 timestamp>
> consolidated into:
>   - patterns.md: <list of changes>
>   - identity.md: <list of changes>
> resolved questions: <list of question references>
```

---

## 7. 输出摘要给用户

格式：

```markdown
# Consolidation Summary — <date>

## 处理的 journals
- 数量: N
- 时间跨度: <first> 到 <last>

## 自动应用的更新
- patterns.md §1.2 lexicon 新增: <list>
- identity.md §三 待验证区 新增: <list>
- patterns.md §8 漏洞修复尝试记录: <list>

## 需要你审的更新
- [ ] <项 1>: <diff 摘要>
- [ ] <项 2>: ...

## 没处理的（数据不足）
- <候选词>: 当前 count <N>，需要 ≥3

## 与 subject-e 基线的累积偏离
- 偏离度: <low | medium | high>
- 关键差异: <列表>
```

**用户确认后**才执行需要审的更新。

---

## 8. 版本号更新

更新 `patterns.md` 和 `identity.md` 的版本号：

- 仅词汇/识别区更新 → patch 版本（0.1.0 → 0.1.1）
- 行为模式更新 → minor 版本（0.1.0 → 0.2.0）
- 与 subject-e 出现结构性偏离 → 用户决定是否升 major（0.x.x → 1.0.0）

---

## Pitfalls

1. **不要在 consolidate 流程里跑回扮演模式。** Phase 4 是元层操作，不是对话。
2. **不要静默改 identity.md。** 永远 diff + 用户确认。
3. **不要删 subject-e.md。** 它是基线，永久冻结。
4. **不要批量处理超过一周的 journal。** 一周以上的批次容易让用户失去审阅能力——拆成多次 consolidate。
5. **不要忘记归档。** 没归档的 journal 下次 consolidate 会被重复处理。
