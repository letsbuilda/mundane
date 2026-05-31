# Turn structure, priority, and the stack

## The five phases

A turn proceeds through five phases, in order:

```
RESET  ->  WAKE_UP  ->  PLAN  ->  DO_STUFF  ->  WIND_DOWN
```

There is **no "advance phase" action**. A phase ends — and the next begins — purely as a consequence
of every player passing priority in a row while the stack is empty (see *Priority* below).

When the final phase (`WIND_DOWN`) ends:

- the turn passes to the next player;
- the phase resets to `RESET`;
- the turn counter increments;
- the new active player's **Wake Up housekeeping** runs: their Time refreshes to **5**, and they draw
  a card from their deck if it is non-empty.

> Stubs, out of scope for the MVP: Wake Up housekeeping currently runs at end-of-turn rather than in
> the `WAKE_UP` phase; combat in `DO_STUFF` (People assigning to Problems) is not implemented.

## Priority

Two notions are kept deliberately separate:

- **`active_player`** — whose *turn* it is. Slow: changes once per turn.
- **`priority_player`** — who may act *right now*. Fast: changes constantly.

That separation is what lets a non-active player act during the active player's turn.

**Granting priority after a stack change.** Whenever a card goes on the stack, the **active player**
receives priority (even to respond to their own card), and the consecutive-pass counter resets.

**Passing priority.** When the player holding priority passes:

1. the consecutive-pass counter increments;
2. if it has **not** reached the number of players, priority moves to the next player;
3. if it **has** reached the number of players (everyone passed in a row with nobody acting), the
   counter resets and either:
   - the **top item of the stack resolves** (if the stack is non-empty), after which the active player
     receives priority again; or
   - the **phase advances** (if the stack is empty).

## Timing

- **Sorcery speed** (`PlayCard`: `PERSON` / `APPLIANCE` / `HABIT` / `TASK`) is the restrictive timing:
  legal only for the **active player**, only during **`PLAN`**, and only when the **stack is empty**.
- **Instant speed** (`CastInstant`: `INSTANT`) is the permissive timing: legal for whoever
  **currently holds priority**, in any phase, regardless of what is on the stack.

## The stack and LIFO resolution

The stack is a list; the most recently added item is the **top**.

- Items resolve one at a time, **top first (LIFO)**. After each resolution the active player receives
  priority again, so further responses are possible before the next item resolves.
- A response goes on top of the item it responds to and therefore resolves **first**.
- Each stack item carries an `id` (assigned from `GameState.next_stack_id`), unique within the game,
  so an effect can target a specific item.
- On resolution: a **permanent** (`PERSON` / `APPLIANCE` / `HABIT`) goes onto its controller's
  **board**; a **one-shot** (`TASK` / `INSTANT`) applies its effect and then goes to its controller's
  **discard**.
