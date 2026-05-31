# Mundane

*No dragons. No spells. Just Tuesday.*

Two households face off across the kitchen table. You spend **Time** to play **People**,
**Appliances**, and **Habits**, drop **Tasks** onto the stack, and answer them with **Instants** — all
to chip away at your opponent's **Composure** until their household falls apart.

This is the **meta repository** for Mundane: the home of the game's design and specification, and the
index to the repositories that implement it. There is no game code here — just the canonical,
human-readable description of how Mundane works.

## Documentation

- **[Specification](./specs/)** — the tight, normative rules: Composure and Time, the five card types,
  the turn structure, priority, the stack, and the exact legality of every action.
- **[Rulebook](./rulebook/)** — the same rules in plain language, with worked examples and a sample
  turn.

The specification is the human-readable *mirror* of the engine. The engine — in
[`mundane-backend`](https://github.com/letsbuilda/mundane-backend) — is the source of truth: where
the code and the spec disagree, the code wins and the spec is the thing to fix. The card library is
documented with the engine.

## Repositories

| Repository | What it is |
|------------|------------|
| [**mundane**](https://github.com/letsbuilda/mundane) (this repo) | Meta and spec — the game design docs and the project index. |
| [**mundane-backend**](https://github.com/letsbuilda/mundane-backend) | The reference implementation — the rules engine plus a [Litestar](https://litestar.dev) HTTP API over it. |

## License

Released under the [MIT License](./LICENSE).
