# Plants Nursery

**University project, 2021** · Java, pre-Compose Android · **being rebuilt**

A nursery catalogue and ordering app with two sides to it: customers browse and order, staff manage the stock.

---

## What it does

| Area | Screens |
|---|---|
| Accounts | `Login`, `Signup`, `splashscreen` |
| Customer | `Dashboard`, `Detail` — browse the catalogue, open a plant |
| Staff | `Admin`, `Admin_Dashboard` — manage listings |
| Location | `MapsActivity`, `MapsActivity2` — find the nursery, place a delivery |

Two roles against one dataset, with Google Maps for both finding the shop and setting a delivery point.

---

## What is being added

A nursery app is where plant recognition actually earns its place, rather than being bolted on:

- **Identify a plant from a photo** — point the camera at something and get the listing, instead of scrolling a catalogue for a name you do not know
- **Spot a sick leaf** — the question customers actually ask staff is *"what is wrong with my plant"*, and that is a vision problem with a real answer

Both run on-device, which is the same ground covered in the [AI Photo Editor](../ai-photo-editor/) — ML Kit, model lifecycle, and the Play Services optional-module behaviour that took measuring to get right.

> Not shipped yet. Listed here because the plan is specific, not because the work is done.

---

## Why it is still listed

It is from 2021 and written in Java before Jetpack Compose existed, so the code is not a sample of how I build today. It is here because it was a complete application rather than an exercise — two roles, authentication, a catalogue, and maps — and because it is the next thing getting rebuilt.
