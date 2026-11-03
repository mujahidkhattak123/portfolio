# Portfolio — Mujahid Khattak

AI engineer, Android and backend. CEO & Founder at Tackle Bytes, Peshawar.

One folder per project. Open any of them for the write-up and the screenshots that go with it. Source for the commercial work stays private.

📧 [mujahidmskhan112@gmail.com](mailto:mujahidmskhan112@gmail.com)

---

## Shipped

### [AI Photo Editor — Background Eraser Photo Cutout](ai-photo-editor/)

![Background Eraser Photo Cutout](ai-photo-editor/hero.png)

Live on Google Play. One-tap background removal running ML Kit subject segmentation **on the device**, with a FastAPI gateway behind it fronting ten model providers for generative replace, inpainting, upscale and text-to-image. Self-hosted BiRefNet on a RunPod GPU worker.

`Kotlin` `Jetpack Compose` `ML Kit` `FastAPI` `Firebase` `Play Billing` `AdMob`

[Play Store](https://play.google.com/store/apps/details?id=com.photostudio.aiphotoeditor) · [Read the case study →](ai-photo-editor/)

---

### [Boat Escape](boat-escape/)

Live on Google Play. Logic puzzle game — unblock the boats to clear the harbour. Full ad lifecycle: App Open with session pacing and frequency caps, UMP consent, and a startup surface that never blocks first play.

`Kotlin` `Jetpack Compose` `AdMob` `UMP`

[Play Store](https://play.google.com/store/apps/details?id=com.boat_puzzle.boatescape) · [Read more →](boat-escape/)

---

## In development

### [Expense Tracker](expense-tracker/)

Offline-first personal finance tracking. Built in phases, each with its own audit and validation pass.

`Kotlin` `Jetpack Compose` `Room`

---

## Earlier work, being rebuilt

Two complete applications from 2021 — Java, before Jetpack Compose. The code is not a sample of
how I build today; both are getting a rebuild with on-device vision doing something the app
actually needed.

### [LifeLink](lifelink/) — blood donor network

Find a donor by blood group and location, or register as one. **Being added:** blood group and
last-donation date read off a donor card with on-device text recognition, and an eligibility
pre-screen so a request reaches people who can actually give.

### [Plants Nursery](plants-nursery/) — catalogue and ordering

Customer and admin sides over one catalogue, with Maps for the shop and for delivery.
**Being added:** identify a plant from a photo, and spot a sick leaf — the question customers
actually ask staff.

### Other 2021 work

IVAP COVID-19 and an on-demand mechanic booking app, plus focused practice on RecyclerView,
Retrofit + Glide, SQLite, Services, BroadcastReceivers, Google Maps and AdMob.

---

## Stack

**On-device ML** — ML Kit (subject segmentation, face detection), Play Services optional-module lifecycle
**Model serving** — FastAPI, provider abstraction over BiRefNet / Replicate / fal / OpenAI Images / Bria / PhotoRoom, self-hosted GPU inference, async job orchestration
**Android** — Kotlin, Jetpack Compose, Coroutines/Flow, Room, CameraX, Play Billing, Play App Signing, AdMob, UMP
**Backend & infra** — Python, Docker, Railway, Firebase Auth, Firestore, GitHub Actions
**Practice** — measure on the device before changing behaviour, pin defects with tests that carry a number, feature flags over branches
