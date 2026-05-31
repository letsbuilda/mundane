# Mundane — Specification

The normative rules of Mundane. These documents are **tight and pedantic by design**: they state what
is legal, what is rejected, and the exact order in which the engine checks. They are the
human-readable *mirror* of the engine, and the engine is the source of truth — where the code and a
spec disagree, the code wins and the spec is wrong and must be fixed.

For the same rules in plain language, with worked examples and a sample turn, read the
[rulebook](../rulebook/) instead. The catalog of individual cards is documented with the engine, not
here.

## Documents

| Document                              | Scope                                                            |
|---------------------------------------|------------------------------------------------------------------|
| [`core.md`](./core.md)                | Execution model, Composure (winning and losing), Time.           |
| [`cards.md`](./cards.md)              | The five card types and the card-definition format.              |
| [`turn-priority.md`](./turn-priority.md) | The five phases, priority, the stack, and timing.             |
| [`actions.md`](./actions.md)          | Every action's preconditions and exact rejection messages.       |

## Conventions

- **"rejected with `X`"** means the engine raises `IllegalAction` carrying the message `X` and leaves
  the state exactly as it was.
- Preconditions are listed **in the order the engine checks them**; the first to fail is the one
  reported.
- Backticked names (`apply_action`, `PLAN`, `time`) refer to engine identifiers.
