# Card sets

Card **content** is published as JSON **sets** in
[`mundane-cards`](https://github.com/letsbuilda/mundane-cards). Card **behavior** (effects) stays in
engine code. A set file is a list of card definitions plus a little header. This document is the
human-readable description of that format; the machine-checkable contract is
[`schemas/card-set.schema.json`](../schemas/card-set.schema.json) (JSON Schema Draft 2020-12).

The backend only loads sets from the allowlisted `mundane-cards` raw origin, validates each against
the schema, and **snapshots** the resolved cards into the game (with a content hash) so logs and
replays stay reproducible.

## Set file

A set file is a JSON object:

| Field     | Type            | Meaning                                                              |
|-----------|-----------------|---------------------------------------------------------------------|
| `set_id`  | `str`           | Namespace for the set. Pattern `^[a-z0-9_]+$`.                       |
| `name`    | `str`           | Human-readable set name.                                            |
| `version` | `str`           | Set version: semver (`1.0.0`) or an ISO-8601 date (`2026-05-31`).   |
| `cards`   | `array`         | One or more card definitions (see below). Must be non-empty.        |

A set may also carry a top-level `"$schema"` string — an editor/tooling hint. No other top-level
fields are allowed (`additionalProperties: false`).

## Card definition

Each entry in `cards` is an object:

| Field    | Type     | Required | Meaning                                                                       |
|----------|----------|----------|-------------------------------------------------------------------------------|
| `id`     | `str`    | yes      | Bare card id, unique within the set. Pattern `^[a-z0-9_]+$`.                   |
| `name`   | `str`    | yes      | Display name.                                                                  |
| `cost`   | `int`    | yes      | Time required to play it. `minimum: 0`.                                        |
| `type`   | `enum`   | yes      | One of `person`, `appliance`, `habit`, `task`, `instant`.                      |
| `effect` | `str`    | yes      | Name of an effect in the engine's fixed vocabulary (see below).               |
| `params` | `object` | no       | Generic parameters for the effect; defaults to `{}`. The engine validates it. |
| `text`   | `str`    | yes      | Rules text shown to players.                                                   |
| `flavor` | `str`    | no       | Optional flavor text.                                                          |

No other card fields are allowed, so a typo like `txet` fails validation loudly.

### Effects are names, not code

`effect` is a **string** naming an effect from the engine's fixed vocabulary; a set cannot define new
behavior. At load time the engine resolves the name to code, binds `params`, and rejects any unknown
name or invalid params. The schema deliberately does **not** check the vocabulary or the shape of
`params` — that contract lives in the engine and changes independently. Permanents (`person`,
`appliance`, `habit`) have no on-resolve effect and use the no-op effect `none`.

### Namespacing

Cards carry **bare** ids. The backend loader composes the runtime id as `set_id:id` — e.g. the card
`throw_a_house_party` in set `core` becomes `core:throw_a_house_party`. Ids must be unique once
composed across all loaded sets; collisions are rejected.

### Scope

The format mirrors the engine's current `Card` fields. There are **no** combat stats (Effort,
Patience) yet because combat is still stubbed — they will be added to the schema when combat lands,
not before. As everywhere in Mundane, the code is the source of truth and the schema mirrors it.

## Example

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "set_id": "core",
  "name": "Core Set",
  "version": "1.0.0",
  "cards": [
    {
      "id": "throw_a_house_party",
      "name": "Throw a House Party",
      "cost": 3,
      "type": "task",
      "effect": "damage_composure",
      "params": { "amount": 3 },
      "text": "Deal 3 chaos to your opponent's Composure."
    },
    {
      "id": "helpful_roommate",
      "name": "Helpful Roommate",
      "cost": 2,
      "type": "person",
      "effect": "none",
      "params": {},
      "text": "A dependable body around the house. Resolves onto your board."
    }
  ]
}
```

See [`mundane-cards`](https://github.com/letsbuilda/mundane-cards) for the published sets and the PR
flow for contributing a card.
