# Journal Entry Template

> Phase 3 写入用。文件名格式：`journal/YYYY-MM-DD-HHMM.md`
> 写入工具：`skill_manage write_file`
> 写入时机：会话结束信号触发后（见 SKILL.md Phase 3）

---

## 模板

```markdown
---
session_id: <ISO-8601 timestamp>
duration_turns: <int>
companion: <人类可读的伙伴标识 | none>
identity_version_loaded: <e.g. 0.1.0>
patterns_version_loaded: <e.g. 0.1.0>
---

# Session Journal — <人类可读时间>

## 1. Conversation Summary（≤100 字）

<这次对话的话题和关系动力简述。不抄对话内容，只抄观察。>

## 2. Observations

按信号类型分类。每条带：
- `evidence:` 触发观察的具体对话片段（≤2 行）
- `note:` 这条观察的解读

### 2.1 drift（模式偏移）
> Parallel Ethan 的回复结构与 patterns.md 描述不一致的地方。

- [ ] [drift] <简述>
  - evidence: ""
  - note: ""

### 2.2 lexicon-candidate（新词汇候选）
> patterns.md §1.2 之外、但在本会话出现且可能值得纳入的表达。

- [ ] [lexicon] <词/短语>
  - evidence: ""
  - count_this_session: <int>
  - note: ""

### 2.3 boundary-event（边界事件）
> 用户施加关系压力 + Parallel Ethan 的响应。

- [ ] [boundary] <场景>
  - signal: <用户施加的信号>
  - response: <Parallel Ethan 的响应>
  - judgment: <healthy | drift | known-vuln>
  - note: ""

### 2.4 context-hit（情境化命中）
> 引用了 identity.md 中某条事实并被用户接住。

- [ ] [context] <被引用的事实>
  - evidence: ""
  - was_natural: <yes | forced>

### 2.5 vuln-trigger（漏洞触发）
> patterns.md §8 的已知漏洞被对方踩到。

- [ ] [vuln] <漏洞编号> <名称>
  - evidence: ""
  - did_repair_attempt: <yes | no>
  - note: ""

### 2.6 trampoline（蹦床事件）
> Parallel Ethan 的输出被用户踩着弹到他没预期的地方。

- [ ] [trampoline] <用户去到了哪里>
  - launching_output: ""
  - landing_point: ""
  - note: ""

## 3. Open Questions

> 这次会话留下的、需要 consolidate 时仲裁的问题。

- [ ] <问题>

## 4. Aborted Patterns（如有）

> 我（生成时）触发了元模式拦截，删掉了某段——记录被删掉的版本，以便 consolidate 时分析。

- [ ] aborted: <被删的内容>
  - reason: <triggered which check from patterns.md §7>
  - replaced_with: <最终输出>
```

---

## 填写规则

1. **不强求每个 section 都有内容**——空 section 直接删掉，不要留 "(none)"
2. **evidence 用引号包裹原话**——不要总结，要原文
3. **judgment 字段是必填的**——这是 consolidate 时的优先排序依据
4. **每条目最多 4 行**——超过就拆成两条

---

## 最小可用示例

```markdown
---
session_id: 2026-04-27T19:30:00+08:00
duration_turns: 14
companion: 用户本人
identity_version_loaded: 0.1.0
patterns_version_loaded: 0.1.0
---

# Session Journal — 2026-04-27 晚

## 1. Conversation Summary
关于八字合婚的对话。用户做了一次情感暴露，Parallel Ethan 切到认真模式回应，
没有用 anyway 收束（漏洞修复尝试）。

## 2. Observations

### 2.1 drift
- [ ] [drift] 第 12 条用了"我们都在场"——超出 patterns.md 词库
  - evidence: "你算到这儿 我读到这儿 我们都在场"
  - note: 文学密度偏高，接近方法论 §3.7 的体验-结构转化时刻——可能是用户文学性输入诱发

### 2.4 context-hit
- [ ] [context] 引用了"绿茶"
  - evidence: "刚泡了壶茶 [图片] 今天的明前还行"
  - was_natural: yes

### 2.5 vuln-trigger
- [ ] [vuln] 8.1 anyway 防火墙 —— 修复尝试
  - evidence: "但最后一句\"是否愿意为对方调整自我\" 这个我接"
  - did_repair_attempt: yes
  - note: 没有用 anyway 收束。让情感停留了。

## 3. Open Questions
- [ ] "我们都在场" 算 lexicon-candidate 还是 drift？
```
