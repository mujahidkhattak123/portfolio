# Boat Escape

**Live on Google Play** · [Store listing](https://play.google.com/store/apps/details?id=com.boat_puzzle.boatescape)

A logic puzzle game. Boats are jammed in a harbour; slide them out of each other's way to clear a path to open water.

`Kotlin` · `Jetpack Compose` · `AdMob` · `UMP`

---

## What was actually hard

Not the puzzle. The ads.

A free puzzle game lives or dies on whether its ad placement is tolerable, and the App Open format is the easiest one to get wrong — it is the first thing a player sees, before they have had any value from the session.

What shipped:

- **Startup-only surface** — the loading screen exists so the ad has somewhere to live that is not on top of gameplay
- **Ready-only fallback** — if nothing is loaded, the game starts. An ad never blocks first play
- **Four-hour cache** with expiry checks, so a stale ad is discarded rather than shown
- **Two per day cap**, plus a shared 180-second pacing window across every full-screen format, so App Open and interstitials cannot stack
- **Third-session onboarding** — a new player sees no full-screen ad at all until their third session
- **UMP consent** before any personalised request

## Packaging

The app shipped under `com.softosolu.boatescape` and later moved to `com.boat_puzzle.boatescape`. A package change is a one-way door on Android — player data does not migrate, and a build from the old archives installs as a separate app. Worth recording because it is the kind of decision that looks free until it is made.

## What is being added

### A solver, which is the whole thing

Levels are hand-authored today, and that does not scale — every new level is someone sitting
down with a grid. The fix is not to generate boards faster; a random board is usually either
trivial or impossible, and shipping an unsolvable level is the one bug a puzzle game cannot
survive.

So the generator needs a solver underneath it. The board is a state, a move is an edge, and
breadth-first search over that graph answers both questions at once: **is this solvable**, and
**what is the shortest solution**. Minimum move count is a difficulty measure you can trust,
because it is a property of the puzzle rather than a guess.

That buys three things from one piece of work:

- **Infinite levels**, each one proven solvable before it ships
- **Difficulty that means something** — bucket by solution length, not by eyeballing it
- **Hints** — the same search returns the next optimal move, which is also the most natural
  thing in the game to attach a rewarded ad to

Classic search, not a model. Worth saying plainly: a transformer would be the wrong tool here,
and the right one is a queue and a visited set.

> Not shipped yet. Listed here because the plan is specific, not because the work is done.

---

## Testing

1,574 Kotlin host-side checks, including 21 groups covering App Open eligibility, caching, pacing and consent state.
