# Expense Tracker

**In development**

Offline-first personal finance tracking for Android.

`Kotlin` · `Jetpack Compose` · `Room`

---

## Approach

Built in numbered phases, each closed out with its own audit and validation pass before the next begins, rather than one long branch. Every phase carries a device-safety backup step — a deliberate habit after learning that `connectedAndroidTest` uninstalls the app under test and takes its database with it.

Offline-first means the local database is the source of truth, not a cache. There is no spinner waiting on a network call to show someone what they spent this morning.

## Status

Phase 5. Not yet published.
