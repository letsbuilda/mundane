# Core model

## Execution model

The engine is a **referee, not a player**. The whole game is a fold over a stream of *actions*:

```
final_state = reduce(apply_action, actions, initial_state)
```

`apply_action(state, action)` is the **only** function that mutates the game. It validates the action
against the current state, then transitions it.

- Validation happens **before** any mutation. An action that fails any precondition is **rejected**:
  the engine raises `IllegalAction` and the state is left exactly as it was.
- There is no other door: nothing changes the game except a successful `apply_action`.

## Composure — winning and losing

`Composure` is a household's life total. Each player starts at **20**.

- When any player's Composure is **0 or below**, their opponent **wins**.
- Once a winner is decided the game is over, and every subsequent action is rejected with
  **"the game is over"**.

## Time — the resource

`Time` is spent to play cards.

- Every card has an integer `cost`. Playing a card spends that much Time.
- A player MUST have `time >= cost`; overspending is rejected with **"not enough Time (have/cost)"**.
- The active player's Time refreshes to **5** during Wake Up housekeeping (see
  [`turn-priority.md`](./turn-priority.md)).
