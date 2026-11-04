# PlantLens — what to say in an interview

Every claim below is backed by a file in this repository or a number measured on a device. If
you cannot point at the thing, do not say it.

---

## The 60-second version

> PlantLens identifies a plant from a photo. The first answer comes from a TensorFlow Lite
> model on the handset — 2,101 species, 4.8 MB, around 100 milliseconds, no upload and no
> network. When that answer is weak it escalates to PlantNet in the cloud, which has global
> coverage and common names. The decision between the two is a pure function with tests,
> because I have watched that rule get reversed twice in another codebase when it was spread
> across call sites.

Then stop. Let them pick what to ask about.

---

## Numbers you should know cold

| | |
|---|---|
| Model | Google AIY Vision Classifier, Plants V1 |
| Classes | 2,102 — 2,101 species plus a background class |
| Size | 4.8 MB, int8 quantised |
| Input | 224 × 224 × 3, uint8 |
| Cold start | 700 ms to interpreter ready |
| Inference | 72–114 ms on a Galaxy A56 |
| Tests | 27, all host-side, no emulator |

---

## The five questions they will ask

### 1. "Why on-device and not just an API?"

Three reasons, in order of how much they matter:

- **It is free.** An API call costs money per request. Inference on the user's phone costs
  nothing, which is what lets the feature be unlimited instead of metered.
- **It works offline.** Someone in a field looking at a crop has no signal. That is the actual
  use case, not a nice-to-have.
- **Nothing is uploaded.** No photo leaves the handset, so there is no retention policy, no
  disclosure, and nothing to breach.

Then the honest half: on-device is worse at species. That is why there is a cloud tier, and
why the choice between them is a decision rather than a default.

### 2. "How did you pick the threshold?"

`IdentifyPolicy.DEFAULT_ESCALATE_BELOW = 0.45f`

Not 0.5. Plant classifiers spread probability across visually similar species — a correct
answer at 0.47 with its siblings at 0.2 is common and useful. A half-point cutoff would send
most of those to the cloud for nothing.

If they push: it is a starting value, not a tuned one. Tuning it properly needs a labelled set
of real user photos, which does not exist yet. Say that rather than inventing a methodology.

### 3. "What went wrong?"

Two things, and both are better answers than "nothing".

**The background class.** Pointed at a room, the first build reported `top=None conf=0.87`.
`None` is the model's 2,102nd class — its way of saying *this is not a plant*. The model was
right; the app was rendering "None — 87% match" as the answer. The fix moved candidate
selection into `CandidateRanking`, a pure function with eight tests: a winning background class
returns an empty list, which the UI already renders as "I could not place this one".

The point to make: listing the runners-up underneath would have been the app overriding the
model to avoid an empty screen. An empty result is an answer.

**The model is North American.** Photographed real leaves in Peshawar; got coast live oak,
Osage orange, bigtooth aspen, a Texas grape, and an Arizona cactus offered for a maple-shaped
leaf. Top confidences 22–27%.

The pipeline was correct and the latency was good. The species coverage did not match where
the app runs. AIY Plants V1 is trained on data heavily skewed to North America.

This is the strongest thing in the project, so do not rush it: the point is that it was found
in week one by testing on a device with real leaves, not three months later by a user.

### 4. "Why is the engine choice a separate function?"

`IdentifyPolicy.plan()` takes five booleans and returns a plan. No I/O, eleven tests.

In a previous project the same kind of decision — which removal engine runs — lived scattered
across call sites. It was reversed twice, each time plausibly, and each reversal cost a round
of device testing to notice. Five booleans and no I/O can be pinned by a test. A rule spread
across call sites can only be pinned by memory.

The rule itself, in one line: **escalate free to paid, never the reverse.**

- Escalation requires an existing entitlement, so being routed to the better engine can never
  surface a paywall
- A paid result is never quietly downgraded
- Every result carries its `IdentifySource` to the screen, so an on-device 27% is never shown
  as though it came from the better model

### 5. "How would you improve the accuracy?"

Fine-tune on regional data. The gap is known and measured, which makes this concrete rather
than hand-waving:

1. Collect 30–40 species common to the region, 200 images each
2. Transfer-learn from MobileNet or EfficientNet — freeze the base, retrain the head
3. Export to TFLite, quantise float32 to int8
4. Swap the asset

Step 4 costs nothing because `OnDeviceIdentifier` reads input geometry from the model rather
than hard-coding 224 × 224. A better model is a file and a label list.

---

## Where the code is

| Claim | File |
|---|---|
| Engine choice is a pure function | `identify/IdentifyPolicy.kt` + `IdentifyPolicyTest` |
| Background-class handling | `identify/CandidateRanking.kt` + `CandidateRankingTest` |
| On-device inference | `identify/OnDeviceIdentifier.kt` |
| Cloud tier | `identify/CloudIdentifier.kt` |
| Response parsing, tested without a network | `identify/PlantNetResponse.kt` + `PlantNetResponseTest` |
| Escalation flow | `app/IdentifyViewModel.kt` |

---

## Details worth having ready

**Why the TFLite runtime is not bundled.** It comes through Play Services, which keeps about
3 MB out of the download and lets the runtime update without an app release. The cost is that
it must initialise before first use and can fail — which is why `isReady()` means *the
interpreter exists*, not *a probe failed in an encouraging way*.

That distinction is not theoretical. In a sibling project, readiness was inferred by running a
tiny inference and treating any non-specific failure as success. It reported ready in 1198 ms
and the first real request failed two minutes later with `UNAVAILABLE`.

**Why the preprocessing centre-crops.** Squashing a portrait photo into a square distorts leaf
shape and venation, which is most of what the model reads.

**Why the camera rotates the frame.** CameraX reports sensor orientation separately from the
pixels. A photo taken in portrait arrives rotated 90°, and a leaf fed to the classifier
sideways is a different image than the one the user framed.

**Why the API key is not in the repository.** It is read from `local.properties` into
`BuildConfig` at build time. An absent key is a supported state: `CloudIdentifier.isReady()`
returns false, the policy is told `cloudConfigured = false`, and every request stays on the
device. Nothing fails.

**Why the cloud timeout is 25 seconds and not 60.** This tier improves an answer the user may
already be looking at. A minute of spinner is worse than the on-device result they had at
100 ms.

**Why the on-device answer is shown before the cloud call finishes.** The round trip would
otherwise leave the screen blank while a usable answer already existed.

**Why a failed escalation falls back instead of erroring.** `remote.recoverCatching { localResult ?: throw error }`
— a cloud failure must not erase a local answer the user could use. The source chip still says
where it came from.

---

## What is not done, and say so

- The 0.45 threshold is untuned
- No regional fine-tuning yet — the gap is measured, not closed
- Leaf disease detection is designed, not built
- No Flutter port yet

Saying this unprompted is better than being caught by it. It also shows you know what finished
looks like.
