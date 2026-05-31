# Action legality

Every action is validated *before* anything is mutated, so a rejected action changes nothing. The
messages below are the **exact** messages the engine raises with `IllegalAction`, and the preconditions
are listed in the order they are checked.

## Global — every action

Before any action-specific check: the game MUST NOT be over (`winner is None`), else
**"the game is over"**.

## `PlayCard(player, hand_index)` — sorcery speed

1. The player holds priority — **"you don't have priority"**.
2. The player is the active player — **"only the active player may play sorcery-speed cards"**.
3. The phase is `PLAN` — **"sorcery-speed cards only during Plan"**.
4. The stack is empty — **"the stack must be empty for sorcery-speed cards"**.
5. `hand_index` is a valid index into the player's hand — **"no such card in hand"**.
6. The card is not an `INSTANT` — **"use CastInstant for instants"**.
7. The player has enough Time (`time >= cost`) — **"not enough Time (have/cost)"**.

On success: the card leaves the hand, its cost is paid in Time, it goes on the stack, and the active
player receives priority.

## `CastInstant(player, hand_index, target_id=None)` — instant speed

1. The player holds priority — **"you don't have priority"**.
2. `hand_index` is a valid index into the player's hand — **"no such card in hand"**.
3. The card is an `INSTANT` — **"that card isn't an instant"**.
4. The player has enough Time (`time >= cost`) — **"not enough Time (have/cost)"**.

On success: as above, and the optional `target_id` is recorded on the new stack item.

## `PassPriority(player)`

1. The player holds priority — **"you don't have priority to pass"**.

On success: priority passes — resolve the top of the stack, advance the phase, or hand priority to the
next player (see [`turn-priority.md`](./turn-priority.md)).

## Anything else

An unrecognised action is rejected with **"unknown action: ..."**.
