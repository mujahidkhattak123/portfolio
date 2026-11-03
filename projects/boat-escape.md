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

## Testing

1,574 Kotlin host-side checks, including 21 groups covering App Open eligibility, caching, pacing and consent state.
