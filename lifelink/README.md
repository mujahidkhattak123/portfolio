# LifeLink

**University project, 2021** · Java, pre-Compose Android · **being rebuilt**

A blood donor network. Someone needs a specific blood group in a specific city, and someone nearby is willing to give it — the app is the thing in between.

Originally built as *Blood Bank Management System*. Renamed because the old title described a database, not a product.

---

## What it does

| Screen | Purpose |
|---|---|
| `Sign_Up_Act` / `Sign_in` | Account creation and authentication |
| `Search_Donor` | Find donors by blood group and location |
| `Donait_Blood` | Register yourself as a donor |
| `Donor_Data_contact` | Donor details and contact |
| `User_profile` | Manage your own record |

The same person can be on both sides of it — a donor this month, someone making a request the next.

---

## What is being added

Blood donation is one of the places where AI is usually bolted on for the sake of it. Two things here are not:

- **Read the donor card** — blood group and last donation date come off a card or a lab slip with on-device text recognition, instead of being typed by someone in a hurry. Typed blood groups are a field people get wrong, and getting it wrong here is not a cosmetic bug
- **Eligibility before contact** — a short pre-screen (last donation, weight, recent illness, medication) so a request reaches people who can actually give, rather than everyone with a matching group

Both stay on the device. Health details are not something to post to a server to find out whether someone is eligible.

> Not shipped yet. Listed here because the plan is specific, not because the work is done.

---

## Why it is still listed

Five years old and written in Java. It is here because it was a complete application rather than an exercise — authentication, a search flow over real records, and a two-sided model — and because the problem is worth doing properly.
