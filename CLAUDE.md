# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Git workflow
- No branches. Always commit directly to `main`.

## Working context

Full notes, numbers and sources: `01-origin-story/session-notes-2026-10-06.md`.

### Me, the products, the people
- I'm the new PM on **Rook Dispatch**. I started after 4.2 shipped on
  12 Aug 2026 and had no overlap with Priya Raghunathan, the previous PM,
  who left 21 Aug. Her handover (`00-rook/company/notes/handoff-from-priya.docx`)
  is one person's view. She admits she "made calls faster than I checked them".
- **Dispatch:** an incident comes in, available responders are ranked, and
  pings go out one at a time until someone takes it. It has three parts:
  console (handlers), phone app (responders) and routing (the risk).
  **Rook Supply** handles gear; it reads Dispatch's availability record.
- **Team** (wiki Team directory):
  - Helen Achebe, Director of Product (roadmap)
  - Marcus Oyelaran, Engineering Manager
  - Wen Li, Staff Engineer (built routing, wrote the 2019 TODO in `history.py`)
  - Nadia Hoffmann, Support Lead
  - Sofia Marino, Product Designer (ran the September interviews)
  - Ravi Menon, Data Analyst (weekly acceptance reports)
- **Vocabulary:**
  - **Ping**: one callout offered to one responder.
  - **Outcomes**: taken, turned down or missed (no answer within the ping
    wait).
  - **Acceptance rate**: the headline metric, reported weekly in aggregate.
  - **Handlers** look after responders and use the console.

### What we've found (as of 6 Oct 2026; data ends 6–7 Sep)
- **4.2 changed two things:** proximity weighted up and recent acceptance
  down, plus the ping wait cut from 90s to 60s. The timeout is the trigger:
  missed pings jumped from 2.3% to about 20% on 12 Aug and are still 13% in
  early September. Unfilled callouts doubled, from 5.5% to 11% (13–14% a
  week for the three weeks after 4.2), but the week of 31 Aug was back to
  5.5% (7 of 127 callouts), inside the pre-4.2 weekly range of 2.9–8.3%.
  One small week; confirm with data after 6 Sep before calling it recovered.
- **A miss scores the same as a turn-down** (`history.py`; the Glossary says
  so too). Scores never recover on their own. Result: four responders
  (Farlight, The Undertow, Meteor Mite, Vesper) fell from about 12 pings a
  week to about 1, and still about 0 in September. Before 4.2 they turned
  down about 20% of pings, like everyone else, and missed about 3%; after
  4.2 they missed 59% (the other twelve, 14%). Their misses came first, then
  their pings fell. The others absorbed the work (The Gale is "exhausted").
  Inferred from code plus data; scores aren't stored, so this is unconfirmed
  with engineering.
- **The tickets are a late, partial signal.** 147 tickets, many repeated or
  templated; weekly volume went from 5–8 to 20–32 after 12 Aug, but filter
  and unrelated tickets rose too. "Gone before I could answer" tickets start
  on 12 Aug (6 that week) and fade to 0 by 31 Aug, though missed pings are
  still 12.7%. "Quiet responder" tickets (35) start 17 Aug, about a week
  after the data moved, and peak 24 Aug. They name Farlight (11) and The
  Undertow (11), plus Ashgrove (5) and Halfmoon (5), who dipped only about
  30%. None name Meteor Mite or Vesper; Kip and Aunt Dot never filed. The
  interviews were for console research, so the ping problem came up only in
  passing. The worst-hit handlers complain least.
- **Priya's seasonal theory explains callout volume** (about 16 per day in
  August, back to 19 in September), not the misses or the four collapses.
  There's no prior-year data.
- **Early September looks recovered on average** (74% taken), but only
  because routing worked around the four.
- **Other 4.2 changes:** filter persistence is mostly fine, except that a
  silent reset leaves handlers on the wrong list (a safety risk). The three
  defect fixes worked.
- **Design clue (Module 5):** handlers find out about a lost ping from the
  responder, after the fact. Aunt Dot wants to be told too.

### Contradictions and gaps across 00-rook
- **Bulk callout (4.1) and the routing override audit log (4.0)** are listed
  as shipped, but neither appears in the code, and their wiki briefs say
  "nobody's picked it up" / "can pick this up". Ask whether they exist.
- The **README says ranking only sets the order**. In practice, low rank
  means never asked.
- The **60s clock starts when the ping is sent**, so a late push or no signal
  still counts as a miss.
- **Nothing tells the handler** when a ping is missed.
- There's **no limit on load** per responder.
- `history.py` keeps scores **in memory**. Do they reset on deploy?
- There's **no rationale anywhere for 60s**. Time-to-accept is a stated
  metric, but answer times aren't stored.
- **Availability Confidence** (committed for 4.2) didn't ship, and the
  roadmap hasn't been reviewed since 30 Jun. It may touch the Supply
  contract.
- **The handoff is wrong in places:** it calls mobile "stable", claims an
  every-year August dip without data, and omits Ravi.

### Open questions and next steps
- Marcus/Wen: is missed = declined intended? Any score recovery? Can we get
  scores or answer times? Do bulk callout and the audit log exist? Marcus
  asked on 14 Aug whether the change was "a decision or… just fell out that
  way"; still unanswered.
- Helen: what happened to Availability Confidence and the other Q3 items?
- Ravi: where are the weekly reports? Can he supply callouts per week for
  July–September 2025 (and ideally 2024)? Callouts fell from about 138 a
  week to 110 in the week of 10 Aug, then 121, 124, 127. That is a one-week
  step, not a gradual drift, so it can't be called seasonal without prior
  years.
- How would Rook spot the next quiet, non-complaining responder?
- Write the "how ranking works" doc Priya asked for.

### How to help me
- Separate evidence from belief, and say which is which.
- Count people, not tickets. Test claims against the pings data and the four
  affected responders.
- Treat tickets as a late, partial signal: use them for timing and for what
  handlers feel, not for size or for who is affected. Check them against the
  pings data by week and by responder before quoting them.
- Say when something comes from code rather than data, or isn't verified.
- **Misses rose for everyone, not just the four.** The other twelve went from
  about 2% missed to 11–16% after 4.2; the four missed 59%. Misses came
  first, then their pings fell. Pings taken per ping fell from 77% to 64%,
  and fewer callouts (-15%) explains most, not all, of the drop in takes.
- **Vesper's own area kept its work.** Old Town callouts barely fell (about
  1.6 to 1.5 a day), but Vesper went from being pinged on all 72 of them
  (took 70) to 10 of 38; Nightwell, Captain Vantage and The Longcast got the
  rest. All four of Vesper's first-week misses were outside Old Town.
  Meteor Mite shows the same pattern in Eastgate. The Undertow's handler
  (Desmond Okafor) filed 11 quiet tickets starting 20 Aug; Vesper, Meteor
  Mite and The Gale have none. Walkthroughs are in
  `01-origin-story/quiet-responders-vesper-and-meteor-mite.md`.
- **Why they don't come back (from code, unconfirmed):** +0.08 for a take,
  -0.12 for a miss or turn-down, floor 0, no decay (the 2019 TODO), kept in
  memory. The score is worth about 19 minutes of travel against proximity. A
  low-scored responder is only asked after everyone above has missed or
  declined, so what they get is the hard leftovers.
- **Wen was away 14–24 Aug**, and Marcus's 14 Aug question about missed =
  declined went unanswered. That is one factor in the slow response, not the
  only one: the 19 Aug "seasonal" call and 26 Aug "August dip" also delayed it.
- **Still to check:** data after 6 Sep (the week of 31 Aug had 5.5% unfilled,
  7 of 127, so it may be recovering); prior-year callouts; the 80% to 63%
  "closest first" claim from colleagues (no distance data in the database).
  Pronouns for the responders aren't known, so use they/them or the name.
