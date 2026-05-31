# Worked examples

Two plays, traced step by step, to show how priority and the stack actually flow. The mechanics behind
them live in [how to play](./how-to-play.md) and, exactly, in the [specification](../specs/). The cards
named here — *Helpful Roommate*, *Throw a House Party*, *Noise Complaint* — are just convenient
examples; the full card library is documented with the engine.

We'll call the players **A** (the active player, whose turn it is) and **B** (the opponent).

## Example 1 — a quiet turn: playing a permanent

It's A's turn. A has just woken up: Time is back to **5** and A has drawn a card.

1. The turn walks `Reset -> Wake Up -> Plan`. (Each of those handoffs is just both players passing
   with an empty stack — nothing dramatic happens.)
2. **Plan.** A plays **Helpful Roommate** (a Person, cost 2). A's Time drops from 5 to **3**. The
   Roommate goes onto the stack, and — because the stack just changed — A gets priority back.
3. A has nothing else to do, so A **passes**. Priority moves to B.
4. B doesn't want to interfere, so B **passes** too. That's everyone in a row, and the stack is not
   empty — so its top item resolves. Helpful Roommate is a permanent, so it simply lands on **A's
   board** and stays there. A gets priority again.
5. The stack is now empty. A passes, B passes — and with nothing waiting, **Plan ends** and Do Stuff
   begins. The same quiet pass-pass walks the turn through Do Stuff and Wind Down, after which the turn
   hands off to B (whose Time resets to 5, and who draws).

Net result: A spent 2 Time and now has a Helpful Roommate on the board.

## Example 2 — the counter: responding on the stack

This is the play the whole priority system exists for. Same setup: A's turn, Plan phase, empty stack,
A holding priority with **5** Time. A is holding **Throw a House Party** (a Task, cost 3, which would
deal 3 to B's Composure). B is holding **Noise Complaint** (an Instant, cost 1, which counters a task
on the stack) and has at least 1 Time to spare.

1. A plays **Throw a House Party**. A's Time drops 5 → **2**. The Task goes on the stack, and A gets
   priority back.

   ```
   stack (top -> bottom):
     [0] Throw a House Party   (A)
   ```

2. A doesn't respond to their own Task, so A **passes**. Priority moves to B.
3. **B responds.** Instants can be cast whenever you hold priority — even on the opponent's turn. B
   casts **Noise Complaint**, targeting the House Party. B's Time drops by 1. Because it was cast
   *after* the House Party, it lands **on top** of it. The stack changed, so priority snaps back to A.

   ```
   stack (top -> bottom):
     [1] Noise Complaint       (B)   <- resolves first
     [0] Throw a House Party   (A)
   ```

4. A has no answer, so A **passes**. Priority moves to B. B **passes** too. Everyone has passed in a
   row and the stack is not empty, so the **top item resolves first**: Noise Complaint. It counters the
   House Party — lifting it off the stack and sending it straight to A's discard, *unresolved*. Noise
   Complaint, having done its job, then goes to B's discard.

   ```
   stack: empty
   ```

5. The House Party never resolved, so it **never dealt its 3**. B's Composure is untouched. With the
   stack empty, A and B pass once more and Plan ends.

Net result: A spent 3 Time for nothing; B spent 1 Time to make sure of it. That's the payoff of holding
an Instant and waiting — last onto the stack, first to resolve.
