# FRESHMAN GRINDSET — Campaign Bible: "RISE & GRIND"

> Design spec for the story campaign. Directed for: **4-act semester · goal = own the whole school · tone = goofy throughout · spine = Reputation ("Clout") + branching story choices with multiple endings.**
>
> This is the authoritative design doc. The implementation subagent builds to this. Keep it self-contained in `index.html`, zero external requests, mobile-first — same constraints as the existing game.

---

## 1. THE ONE-LINER
You start as a freshman nobody whose college fund evaporated in a crypto rug pull. Over one chaotic semester you rise from selling one cart in one bathroom to running the entire underground economy of Grindset High — and how you get there (and who you become) is up to you.

The whole game is played for laughs. The "story" is an escalating excuse for dumber and dumber high-stakes scenarios. There is no real darkness — just bigger stupidity and funnier consequences.

---

## 2. THE SPINE: CLOUT & SCHOOL CONTROL

A single meta-progression stat carries the campaign. Two faces of the same number:

- **CLOUT** — your reputation. Earned every day; never resets within a campaign.
- **SCHOOL CONTROL %** — CLOUT rendered as "how much of the school you run." Shown in the HUD as `CONTROL 24%` and on the act-transition screens as a filling bar.

### Earning Clout (per day, folds into daily cash summary)
| Action | Clout |
|---|---|
| Close a deal | +2 (exact-product match +3) |
| Win a class activity (quiz/called-on/awake) | +3 |
| Out-earn Chadwick that day | +10 |
| Complete the act's story mission | +25 (big) |
| Beat a day without getting busted | +5 |
| Get busted | -6 |
| Shove a teacher / caught doing something heinous | -10 |

### Control gates
Each act requires a **Control threshold** to unlock its finale mission and advance. Falling short = "run it back" (replay days in the act). This is the pacing governor.

- End of Act 1 → need **25%**
- End of Act 2 → need **50%**
- End of Act 3 → need **75%**
- End of Act 4 → **100%** (the takeover)

Persist `fg_campaign` in localStorage: `{act, day, control, brand, flags:{}, endings_seen:[]}`.

---

## 3. STRUCTURE — 4 ACTS / ~20 DAYS

Each act = ~5 days. Each act has: a **theme**, an **antagonist**, a **new wrinkle** (mechanic or area), a scripted **story-mission day**, and ends on a **CROSSROADS** — a full-screen decision that sets a persistent flag and colors everything after.

Reuse the existing `story` screen infrastructure for act intros and crossroads. Crossroads = the story card with 2–3 tappable choice buttons instead of one "First Bell" button.

---

### ACT 1 — "NEW KID ENERGY" (Days 1–5) · Freshman Fall
- **Theme:** learning the ropes; you're a nobody with a backpack and a dream.
- **Antagonist:** the Hall Monitor + your own incompetence.
- **Wrinkle:** none new — this act teaches the existing loop. Clout is introduced.
- **Story mission (Day 5): "THE BAKE SALE WAR."** The student council bake sale is a front for Chadwick moving product under a "gluten-free" label. Objective: out-sell the bake sale before final bell (hit a cash target during the day).
- **CROSSROADS — "How do you make your name?"** Sets your **BRAND** flag (permanent, flavors all later dialogue & the ending):
  - **A) MUSCLE** 👊 — "They'll respect you or they'll limp." Shoves intimidate customers into paying more; teachers wary of you.
  - **B) CHARM** 😎 — "Undercut, out-talk, out-hustle." Better deal prices; customers text you more.
  - **C) GHOST** 👻 — "They can't tax what they can't catch." Lower heat gain; the monitor loses you faster.

---

### ACT 2 — "COLD WAR" (Days 6–10) · Winter
- **Theme:** Chadwick stops being a nuisance and becomes a real rival with a crew.
- **Antagonist:** **Chadwick**, actively contesting deals, running sabotage events (spreads rumors that spike your heat; plants a fake customer).
- **Wrinkle:** **Cafeteria unlocks** as a high-volume sales floor (if a full new map area is too heavy for v1, represent as a schedule period "LUNCH RUSH" with 2× customers + a Chadwick presence; flag real map build as follow-up).
- **Story mission (Day 10): "THE SHOWDOWN."** A scripted head-to-head sales-off against Chadwick in the cafeteria. Whoever sells more by the bell wins the act.
- **CROSSROADS — Chadwick's offer:** he proposes a merger.
  - **A) MERGE** 🤝 — partner up. Chadwick becomes an **ally** in Acts 3–4 (shows up to help in the finale). Flag `chad=ally`.
  - **B) CRUSH** 💥 — refuse, total war. He gets desperate and dangerous. Flag `chad=nemesis`.
  - **C) ABSORB** 🪤 — poach his crew out from under him. He's humiliated, goes lone-wolf villain. Flag `chad=broken`.

---

### ACT 3 — "THE ADMINISTRATION" (Days 11–15) · Spring
- **Theme:** you're too big to ignore. The faculty and **Principal Krunk** come for their cut.
- **Antagonist:** the administration. Patrols intensify; a recurring **"VIBES AUDIT"** and **locker sweeps** raise the stakes.
- **Wrinkle:** **NOTORIETY** — a slow campaign-long heat track (separate from daily heat). High notoriety = more monitors, surprise sweeps. The Admin/Teacher's Lounge wing becomes a high-risk/high-reward zone.
- **Story mission (Day 15): "THE LOCKER SWEEP."** Survive a day where the admin searches lockers — stash your product (choose lockers/allies to hide it) and don't get caught holding.
- **REVEAL (goofy):** Krunk isn't a criminal mastermind — he's a bumbling, corrupt administrator who runs a **trophy-case embezzlement scam** and wants in on your money.
- **CROSSROADS — Krunk's ultimatum:**
  - **A) PAY HIM OFF** 💵 — lose a cash cut, gain "administrative protection" (lower notoriety Act 4). Flag `krunk=paid`.
  - **B) BLACKMAIL HIM** 🗄️ — you found the trophy-case books; he works for YOU now. Flag `krunk=owned`.
  - **C) DECLARE WAR** ⚔️ — hardest path, biggest finale payoff, admin comes at you full force. Flag `krunk=war`.

---

### ACT 4 — "FINALS WEEK" (Days 16–20) · The Takeover
- **Theme:** everything converges. Your Act 1–3 choices cash out — allies show up (or don't), enemies escalate per your flags.
- **Objective:** push Control to **100%** — own every wing.
- **Finale (Day 20): "THE GRINDSET GAMES."** A school-wide chaos day. Final showdown against whoever your choices made the last boss (Chadwick-as-nemesis, Krunk-at-war, or a desperate coalition). The finale scene is assembled from your flags.
- **MULTIPLE ENDINGS** (determined by `brand` + crossroads flags + stats):
  - **THE KINGPIN** 👑 — crushed everyone (`chad=nemesis/broken` + `krunk=war/owned`, high control). "You own the halls. The halls own you. Beautiful."
  - **THE CEO** 💼 — took the merger/legit path (`chad=ally`, `krunk=paid`). You "pivot to a real business," Chadwick as COO, IPO on the horizon.
  - **THE LEGEND / PHANTOM** 👻 — brand=GHOST + very few busts. Nobody ever proved anything. You graduate a myth.
  - **THE CAUTIONARY TALE** 🎤 — too many busts across the campaign. You're now a paid motivational speaker warning kids about the grindset. (Comedic "bad" ending — still funny, not a fail screen.)
  - **CERTIFIED HUSTLER (secret S-rank)** 🏆 — 100% control + beat Chadwick every act + never shoved a teacher. "See the guidance counselor immediately."

---

## 4. BRANCHING DIALOGUE
Lines vary by `brand` and crossroads flags. Implementer: keep a small helper that picks a line variant from a flag, e.g. `brandLine({MUSCLE, CHARM, GHOST})`. Every act intro, Chadwick encounter, Krunk scene, and the finale reads differently based on flags. Volume of variant lines = the delegated content-writing job (Haiku/Sonnet).

**Tone rules (unchanged):** goofy-cartoonish only; parody vape brands, "carts"/💨 only, no real brands, no other substances, comedic violence only, no minors depicted in any inappropriate way. Every line is a joke.

---

## 5. IMPLEMENTATION SCOPE — V4 (this pass)
Build the **narrative + systems layer on the existing map** (don't block on new map wings — flag those as follow-up):
1. **CLOUT / Control %** meta-stat: earn rules above, HUD readout, persisted in `fg_campaign`.
2. **Act engine**: replace the loose day-arc with the 4-act structure; each act gates its finale by Control threshold; act intro cards.
3. **Crossroads screens**: 3 of them (end of Acts 1/2/3) — decision card with tappable options, sets flags. End of Act 4 = ending screen.
4. **Story-mission days**: Bake Sale War, Showdown, Locker Sweep, Grindset Games — implement at least as flavored objective days (special toast banner + a win/lose check) even if the set-piece is light in v1.
5. **Branching dialogue** hooks driven by `brand` + flags.
6. **Multiple endings** screen keyed off flags + stats.
7. **NOTORIETY** long-term track (Act 3+).

Keep the daily loop (class / sell / gym / dodge monitor / Chadwick) exactly as-is underneath — the campaign wraps around it.

## 6. FOLLOW-UP (later passes, not this one)
- Real map wings: Cafeteria, Admin/Teacher's Lounge.
- Crew/recruiting + turf systems (chosen against for v1, but the fiction leaves room).
- Higgsfield art pass: act splash cards, character portraits (Chadwick, Krunk, Big Toe, teachers), product packaging.
