# Cards

## Card types

There are five card types. The first three are **permanents**: on resolution they stay on their
controller's board. The last two are **one-shots**: on resolution they take effect and go to their
controller's discard.

| Type        | Value         | Permanent? | Timing        |
|-------------|---------------|------------|---------------|
| `PERSON`    | `"person"`    | yes        | sorcery speed |
| `APPLIANCE` | `"appliance"` | yes        | sorcery speed |
| `HABIT`     | `"habit"`     | yes        | sorcery speed |
| `TASK`      | `"task"`      | no         | sorcery speed |
| `INSTANT`   | `"instant"`   | no         | instant speed |

Sorcery speed and instant speed are defined in [`turn-priority.md`](./turn-priority.md).

## Card-definition format

A card is **data plus an effect**. Each full definition lives in exactly one place — the card library
— and game state never stores card objects, only their string `id`. Effects are *code*, not *data*,
so they are never serialised; this is what lets the entire state round-trip through JSON.

A card definition has these fields:

| Field    | Type                          | Meaning                                                |
|----------|-------------------------------|--------------------------------------------------------|
| `id`     | `str`                         | Stable identifier; this is what state stores.          |
| `name`   | `str`                         | Display name.                                          |
| `cost`   | `int`                         | Time required to play it.                              |
| `type`   | `CardType`                    | One of the five types above.                           |
| `effect` | `(state, stack_item) -> None` | Mutates state on resolution. Permanents use a no-op.   |
| `text`   | `str`                         | Rules text shown to players.                           |
| `flavor` | `str`                         | Optional flavor text (defaults to empty).             |

When the engine needs a card's type, cost, or effect, it resolves the `id` through the library. Player
collections (`hand`, `board`, `deck`, `discard`) and stack items therefore hold `id`s, never objects.

## Authoring cards as JSON sets

Cards are **published as data**, not written in engine code. Each card lives in a JSON
[card set](./card-sets.md) in [`mundane-cards`](https://github.com/letsbuilda/mundane-cards); the
backend fetches an allowlisted set, validates it against
[`card-set.schema.json`](../schemas/card-set.schema.json), and builds the library at load time.

In a set file `effect` is a **string** naming an effect from the engine's fixed vocabulary (plus a
generic `params` object), not a function — the engine resolves the name to code and rejects unknown
names or invalid params. Permanents use the no-op effect `none`. See
[`card-sets.md`](./card-sets.md) for the full set-file format.
