# Mundane — Schemas

Machine-checkable contracts for Mundane data. Today there is one:

| Schema | Describes |
|--------|-----------|
| [`card-set.schema.json`](./card-set.schema.json) | A **card set** — the JSON file format used to publish cards in [`mundane-cards`](https://github.com/letsbuilda/mundane-cards). |

The human-readable description of the set-file format lives in
[`../specs/card-sets.md`](../specs/card-sets.md). This directory holds the normative,
machine-readable version.

## `card-set.schema.json`

- **Dialect:** JSON Schema **Draft 2020-12** (`https://json-schema.org/draft/2020-12/schema`).
- **`$id`:** `https://docs.letsbuilda.dev/mundane/schemas/card-set.schema.json` — a stable identifier
  for the schema. (The canonical identifier need not equal any fetch URL.)
- **Scope:** validates the *structure and bounds* of a set file: the required fields, the five card
  `type` values, id/`set_id` patterns, `cost` bounds, and `additionalProperties: false` so a typo like
  `txet` fails loudly.
- **Out of scope — deliberately:** the schema does **not** check that `effect` names a real effect.
  The effect vocabulary lives in engine *code* (`mundane-backend`) and changes independently; the
  engine rejects unknown effect names and bad `params` at load time. Keeping vocabulary out of the
  schema keeps this contract stable as the engine grows. (`params` is a generic object here for the
  same reason — the engine validates it per effect.)

### Namespacing

Cards carry **bare** ids (`^[a-z0-9_]+$`). The backend loader composes the runtime id as
`set_id:id` (e.g. set `core` + card `throw_a_house_party` → `core:throw_a_house_party`). A card can't
disagree with its own set's prefix, and cross-set id collisions are rejected by the cards-repo CI and
again by the backend.

## Validating locally

The org uses [`check-jsonschema`](https://check-jsonschema.readthedocs.io/) (also what
`mundane-cards` CI runs). With [`uv`](https://docs.astral.sh/uv/):

```sh
# 1. The schema is itself a valid Draft 2020-12 schema:
uvx check-jsonschema --check-metaschema schemas/card-set.schema.json

# 2. A real set validates green:
uvx check-jsonschema --schemafile schemas/card-set.schema.json schemas/examples/core.example.json

# 3. The negative fixtures MUST fail (bad `type`, and an extra/typo'd field):
uvx check-jsonschema --schemafile schemas/card-set.schema.json schemas/examples/invalid/bad-type.json
uvx check-jsonschema --schemafile schemas/card-set.schema.json schemas/examples/invalid/extra-field.json
```

`examples/core.example.json` is a valid set (the Core Set). `examples/invalid/` holds files that are
valid *except* for one deliberate error each, so you can see exactly what the schema rejects.

## Versioning the schema

Consumers pin the schema by version. Bump it deliberately (the same discipline used for pinning
Litestar to a commit):

- Tag the meta repo `schema-v1` once a schema revision lands on `main`. Consumers
  (`mundane-cards` CI, `mundane-backend`) **vendor** a pinned copy of the schema rather than fetching
  it at runtime, so unrelated changes on `main` never break them.
- A breaking change to the contract gets a new tag (`schema-v2`) and a coordinated bump in consumers.
