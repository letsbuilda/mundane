# How to play

Two households face off across the kitchen table. Everything below is the gentle version of the
[specification](../specs/) — read that when you need the exact wording.

## The goal: drain their Composure

**Composure** is your household's patience with the world. You both start at **20**. Knock your
opponent's Composure to **0 or below** and their household falls apart — you win. From that moment the
game is over and nothing else can happen.

## The budget: Time

You pay for everything with **Time**. Every card costs some whole number of Time to play, and you
simply can't spend Time you don't have. At the start of each of your turns your Time resets to **5**
and you draw a card (if your deck still has cards in it). Time does not carry over — use it or lose it.

## The cards

There are five kinds of card. Three of them *stick around*; two of them happen *once* and are gone.

**Stays on your board (permanents):**

- **People** — the dependable bodies around the house.
- **Appliances** — the gadgets you've talked yourself into buying.
- **Habits** — the routines, wholesome or otherwise.

When one of these resolves, it lands on your side of the table and stays there.

**Happens once, then into the discard (one-shots):**

- **Tasks** — chores and schemes you carry out. A Task goes onto *the stack* (see below), does its
  thing, and is discarded.
- **Instants** — quick reactions. Like a Task, but you can play one almost any time, even on your
  opponent's turn.

That last difference is the heart of the game, so it has a name.

## Timing: sorcery speed vs instant speed

- **Sorcery speed** is the slow, polite timing. People, Appliances, Habits, and Tasks can only be
  played on **your own turn**, during the **Plan** phase, and only when nothing else is waiting to
  happen. This is the restrictive timing.
- **Instant speed** is the quick timing. Instants can be played whenever it's your moment to act —
  even in the middle of your opponent's turn, even in response to something they just did.

## A turn, phase by phase

Every turn walks through five phases, always in this order:

```
Reset  ->  Wake Up  ->  Plan  ->  Do Stuff  ->  Wind Down
```

- **Reset / Wake Up** — housekeeping. Your Time goes back to 5 and you draw.
- **Plan** — the phase where you actually play your People, Appliances, Habits, and Tasks.
- **Do Stuff** — where combat will eventually live. *(Combat isn't built yet — for now this phase is a
  quiet one.)*
- **Wind Down** — the wrap-up before the turn passes to your opponent.

Here's the part people find surprising: **there is no "next phase" button.** You never declare that a
phase is over. A phase ends only when *everyone* has passed in a row with nothing waiting to resolve.
Which brings us to priority.

## Priority: whose moment is it?

Keep two ideas separate:

- The **active player** is whose *turn* it is. This changes once per turn.
- The player with **priority** is whoever may act *this instant*. This changes constantly.

They are not the same thing, and that's the whole point: because priority can be held by the
non-active player, you get to do things on your opponent's turn.

When you have priority you may either play a card you're allowed to play, or **pass**. Each time
someone plays a card, priority snaps back to the active player (so they can respond first), and the
count of "passes in a row" resets to zero. When players just keep passing:

- Once everyone has passed in a row, the game looks at the stack.
- If something is waiting on the stack, the top item resolves — then the active player gets priority
  again, in case anyone wants to react to *that*.
- If nothing is on the stack, the phase finally ends and the next one begins.

So phases don't advance because someone says so; they advance because the table has gone quiet.

## The stack: things wait their turn

When you play a Task or an Instant, it doesn't take effect immediately. It goes onto **the stack** — a
pile where the **most recent card sits on top**. The stack resolves **top first**: last in, first out.

This is why reacting works. If your opponent plays a Task and you respond with an Instant, your Instant
lands *on top* of their Task — so your Instant resolves **first**, before their Task ever gets the
chance. Permanents skip all this drama: when a Person, Appliance, or Habit resolves, it just goes onto
your board.

Want to see all of this in motion? Head to the [worked examples](./examples.md).
