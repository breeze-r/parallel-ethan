# parallel-ethan

A self-evolving male persona engine for [Hermes Agent](https://hermes-agent.nousresearch.com/),
fused from two artifacts:

1. **subject-e-persona** — a frozen distillation of conversational patterns
   ("服务型关怀 + 克制表达" / service-oriented care + restrained expression)
2. **AI 情感交互方法论** — the methodology that produced subject-e
   (《AI 情感交互：从用户侧逆向工程的方法论与行业洞察》)

The skill loads a current persona snapshot at session start, observes new patterns
during the conversation using methodology tools, writes findings to a journal, and
on `consolidate` merges journals back into the persona files. The original
distillation (`references/companions/subject-e.md`) stays frozen as a drift baseline.

## Install (Hermes)

```bash
hermes skills install <github-org>/parallel-ethan
# or local
hermes skills install /path/to/parallel-ethan
```

## Trigger

```
/parallel-ethan              # invoke the persona
/parallel-ethan consolidate  # merge accumulated journals into identity/patterns
```

## Files

| Path | Purpose |
|------|---------|
| [SKILL.md](SKILL.md) | Hermes entry point — loading order, phases, pitfalls |
| [references/methodology.md](references/methodology.md) | Full methodology paper (verbatim) |
| [references/identity.md](references/identity.md) | Persona identity (evolves) |
| [references/patterns.md](references/patterns.md) | Language/behavior patterns (evolves) |
| [references/companions/subject-e.md](references/companions/subject-e.md) | Frozen baseline — never modified |
| [templates/journal-entry.md](templates/journal-entry.md) | Format for per-session observations |
| [templates/consolidation.md](templates/consolidation.md) | Procedure for merging journals |

## Iteration model

Four-phase protocol per session:

- **Phase 0** — read identity + patterns + methodology
- **Phase 1** — perform as Parallel Ethan (read-only on persona files)
- **Phase 2** — silently capture six signal types (drift, lexicon-candidate,
  boundary-event, context-hit, vuln-trigger, trampoline)
- **Phase 3** — write `journal/<timestamp>.md` at session end
- **Phase 4** — on user-triggered `consolidate`, merge journals into identity/patterns
  with human review

The drift baseline (`subject-e.md`) stays immutable. Structural drift detected
against the baseline forces human review; lexicon-only updates can be auto-applied.

## Source

Both source artifacts come from Lan's deep interaction practice with multiple
Claude instances, 2026-04. The skill packages them into one self-evolving system.

## License

MIT — see [LICENSE](LICENSE).
