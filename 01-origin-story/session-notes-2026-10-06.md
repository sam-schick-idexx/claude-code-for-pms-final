# Session notes: what went wrong with Dispatch 4.2

Session date: 6 October 2026. Rook Industries is a fictional course scenario.
The data in the Rook database runs from 29 June to 6 September 2026 (pings and
callouts) and 7 September (tickets).

## The short version

Release 4.2 (12 Aug) cut the ping wait from 90s to 60s and reweighted ranking
toward proximity. Missed pings jumped nine-fold overnight. Because a miss
scores the same as a turn-down and scores never recover, four reliable
responders were effectively cut off: Farlight, The Undertow, Meteor Mite and
Vesper. By September the averages look recovered, but only because routing
works around those four. The support tickets point at the wrong people, and
the hardest-hit handlers never complained. Priya's "it's seasonal" explains
the August dip in callout volume, nothing more.

## Sources used

| Source | What it gave us |
|---|---|
| `00-rook/company/notes/handoff-from-priya.docx` | Priya's view of 4.2, open items, roles |
| `00-rook/code/dispatch-routing/` | How ranking, offers and scoring actually work |
| Rook database: `support_tickets` (147) | What handlers complained about |
| Rook database: `pings`, `callouts`, `responders` | What actually happened |
| Rook wiki: Customer interviews (4) | Handlers' own accounts, 2–5 Sep |
| Rook wiki: 4.0/4.1/4.2 release pages, Q3 roadmap, Team directory, Glossary, Dispatch page, briefs | Names, commitments, definitions, what was meant to ship |

## People (wiki Team directory)

| Name | Role | Notes |
|---|---|---|
| Helen Achebe | Director of Product | Owns roadmap and commitments |
| Marcus Oyelaran | Engineering Manager, Dispatch | Asked on 14 Aug whether the ranking change was "a decision or… just fell out that way"; no answer yet |
| Wen Li | Staff Engineer, Dispatch | Built routing; away 14–24 Aug; 2019 TODO in `history.py` |
| Nadia Hoffmann | Support Lead | Owns tickets; flagged 3x ticket volume on 18 Aug |
| Sofia Marino | Product Designer | Console and phone app; ran the September interviews |
| Ravi Menon | Data Analyst | Weekly acceptance reports (not in the handoff) |
| Priya Raghunathan | Former PM | Left 21 Aug |

## 1. Support tickets (147)

| Group | Before 4.2 | After 4.2 | Still open |
|---|---|---|---|
| Ping moved on before the responder could answer | 0 | 15 | 15 |
| Responder has gone quiet | 0 | 30 | 30 |
| Saved console filters | 2 | 14 | 6 |
| Other notification glitches | 6 | 0 | 0 |
| Gear / Supply | 10 | 16 | 9 |
| Other console, account, feature requests | 22 | 32 | 23 |

- The 30 "quiet" tickets cover only four responders: Farlight, The Undertow,
  Corporal Ashgrove and Halfmoon.
- Many tickets are repeats or templated: only 104 of 147 have unique text.
  Count people, not tickets.
- Filter tickets are mostly minor. The exception is silent resets that left
  handlers on the wrong list (#3064, #3072).

## 2. Pings data

| Week of | % taken | % missed |
|---|---|---|
| 29 Jun – 3 Aug | 77% | 2.3% |
| 10 Aug (4.2 ships on the 12th) | 54% | 21.5% |
| 17 Aug | 66% | 17.7% |
| 24 Aug | 67% | 14.8% |
| 31 Aug | 73% | 12.7% |

- Missed pings ran at 0–1 a day until 11 Aug, then 7 on the 12th and 12 on
  the 13th. It was a sudden jump, not a drift.
- Unfilled callouts went from 5.5% to 11.1%.

**Pings per week, before → after 4.2:**

| Responder (handler) | Before | After | 1–6 Sep | Tickets filed? |
|---|---|---|---|---|
| Farlight (Linda Pruitt) | 11.9 | 3.0 | 0 | Yes |
| The Undertow (Desmond Okafor) | 11.9 | 3.8 | 1 (missed) | Yes |
| Meteor Mite (Kip) | 11.1 | 3.8 | 1 (missed) | No |
| Vesper (Aunt Dot) | 13.7 | 4.6 | 1 (missed) | No |
| Corporal Ashgrove (Yusuf Demir) | 10.0 | 7.8 | 7 | Yes |
| Halfmoon (Simone Fischer) | 11.0 | 8.9 | 8 | Yes |
| Other 10 combined | 103 | 133 | — | |

- The four hardest-hit responders turned down nothing. They took everything,
  missed a few pings in release week, and then the pings stopped.

## 3. Early September

| | Before 4.2 | 12–31 Aug | 1–6 Sep |
|---|---|---|---|
| Callouts per day | 20.0 | 16.3 | 19.0 |
| Pings taken | 76.6% | 60.9% | 74.0% |
| Pings missed | 2.3% | 19.6% | 13.0% |
| Tickets per day | 0.9 | 4.0 | 4.0 |

- **Recovering:** callout volume and the share of pings taken.
- **Not recovering:** misses, ticket volume, and the four responders.
- The Gale, Nightwell and Stormwrack took 18–20 pings each in six days.

## 4. Customer interviews (wiki, 2–5 Sep)

These were for console redesign research, so pings came up only in passing.

| Handler (responder) | What happened | What they did | Suspected cause |
|---|---|---|---|
| Aunt Dot (Vesper) | Pings vanish while he runs downstairs; quiet stretches too | Told him to keep the phone in his pocket; no ticket | "I couldn't tell you why" |
| Kip (Meteor Mite and The Gale) | Mite dead quiet, Gale swamped | Told Mite "hang in there"; no ticket | "Whatever's underneath it" |
| Mr. Ambrose (Captain Vantage) | Moved on while suiting up | Filed #3043 | "It felt fast" |
| Halloran (Sgt. Bulwark) | Went to someone else before boots on | Nothing; "these things happen" | None |

- **They agree with the tickets** on the facts and on the console wishlist.
- **They disagree on who's affected.** The worst-hit handlers didn't file
  tickets, and they downplay the problem.
- **Only Aunt Dot links the two symptoms**: the vanishing pings and the quiet
  weeks. The data shows they're one sequence.
- **The headline asks are cosmetic**: dark mode, bigger text. They would bury
  the ping problem if you summarized the interviews by their asks alone.
- **Design clue:** handlers learn after the fact. Aunt Dot wants to be told
  too.

## 5. 4.2 defect fixes

| Fix | Tickets before | After |
|---|---|---|
| Duplicate push notification | #3007, #3016, #3040 | None |
| Capability tag ordering | #3019 | None |
| Coverage export time zone | #3025 | None |

All three appear to have worked. #3050 and #3116 are a separate time-zone bug
in availability hours.

## 6. Contradictions and gaps across 00-rook

**Contradictions**

1. **Bulk callout (4.1)** is listed as shipped, but `offer.py` only offers a
   callout one person at a time. The brief says "nobody's picked it up yet",
   and ticket #3006 can't find the button.
2. **The routing override audit log (4.0)** is listed as shipped, but there's
   no trace of overrides in the code, and the brief says the team "can pick
   this up".
3. **The README says ranking only decides the order.** In practice, low rank
   means never being asked.
4. **Missed = turn-down is documented** (Glossary and `history.py`). It was
   harmless at 90s and harmful at 60s.
5. **The 4.2 weight change should have made misses matter less** (acceptance
   weight went from 0.40 to 0.25). The collapse must come from miss volume
   plus the proximity boost. Unverified, because scores aren't stored.
6. **Priya's handoff is wrong in places:** "mobile stable" (#3034, #3040);
   "August soft every year" (no data); "ask the EM for numbers" (it never
   mentions Ravi).
7. **The Glossary says availability is a ranking input.** The code only uses
   it to decide who's on the list.

**Missing**

- No recovery for a score (Wen's 2019 TODO).
- The clock starts when the ping is sent, not when it arrives, so a late push
  or no signal still counts as a miss.
- No alert to the handler on a missed or withdrawn ping.
- No limit on load per responder.
- Scores are held in memory (`_scores = {}`). Do they reset on deploy?
- No rationale for 60s anywhere.
- Time-to-accept is a stated metric, but answer times aren't stored.
  Acceptance is reported "weekly, in aggregate", which is how the four
  disappeared.
- Availability Confidence (committed for 4.2) didn't ship. The roadmap hasn't
  been reviewed since 30 Jun, and the item may touch the Supply contract
  (`availability.py`).
- The 4.2 defect fixes are in the wiki release notes but not the CHANGELOG.
- `00-rook/feedback/` is empty, and `00-rook/company/` holds only the handoff.

## 7. Open questions and next steps

- **Marcus and Wen:**
  - Is missed = declined intended?
  - Is there any score recovery?
  - Can we get scores or answer times?
  - Do bulk callout and the audit log exist?
- **Helen:** what's the status of Availability Confidence and the other Q3
  items, now that Q3 is over?
- **Ravi:** where are the weekly reports?
- **Rook as a whole:** how would it spot the next quiet, non-complaining
  responder?
- **Me:** write the "how ranking works" doc Priya asked for.
- Hold the fix proposal for Helen's Module 5 request.

## Appendix: practice drafts (not sent; the people are fictional)

**To Marcus Oyelaran, cc Wen Li**

> Hi Marcus, picking up your 14 Aug question on the 4.2 page about whether
> the ranking change was meant to apply to people who've been turning jobs
> down. I think there's a related issue underneath it, and I'd like you and
> Wen to check my reading. In `history.py`, a missed ping takes the same
> penalty as a turn-down. Combined with the 60s timeout, the pings table
> shows a loop. Four responders (Farlight, The Undertow, Meteor Mite and
> Vesper) turned nothing down, missed a few pings in release week, and
> dropped from about 12 pings a week to about 1. Three questions: (1) Is
> missed = declined intentional? (2) Is there anything that lets a score
> recover if someone isn't being pinged? (3) Can we pull scores or answer
> times? Not proposing a fix yet. 20 minutes this week?

**To Helen Achebe**

> Hi Helen, Priya's handover flagged that we never had the conversation about
> which items were squeezed out of 4.2, and now that Q3 has closed I'd like
> to have it. The roadmap hasn't been reviewed since 30 June. Availability
> Confidence is still marked committed for 4.2, but it didn't ship.
> Requisition approval chains (4.3) and the two Q4 items are probably worth
> checking too. Could we agree which are still commitments, which have moved
> and which are dropped? Separately, I have early findings on why 4.2 landed
> badly (it isn't seasonal), and I'd like to walk you through them once
> Marcus confirms one piece.
