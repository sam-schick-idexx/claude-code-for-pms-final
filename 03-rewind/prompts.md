# 03 · Rewind — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly.

Last session you read four conversations and every support ticket
since 4.2 — and found the two piles did not agree.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

Open 00-rook/data/callout-history.csv. Every row is one responder in one week: how many times we pinged them, and how many of those they took. Release 4.2 shipped on 12 August. Tell me what changed after that date. Show me the weekly numbers before and after, and show me the rows you used to get them.

### 2.

I need 3 prompts to understand the data
outcome: try to come up with a number you would feel comfortable sharing with leadership on what we are observing (not the root cause, but the impact)

### 3.

Others also noted the following:

[pasted text from others omitted]

Tell me if that alters your conclusions, and tell me what the most interesting feedback or findings I can paste would actually be

### 4.

Tell me what to actually share, in language that would make sense coming from me

### 5.

How can we research the "why it's happening"?

### 6.

I had shared the following. Tell me if this needs to be revised:

[pasted text of my own post omitted]

### 7.

Attached are several graphs people supplied. Analyze these for interesting results, and tell me if we should look at any of this data differently.

### 8.

Does this data check out:

1. The ping timeout (how long an offer stays live before it’s marked “missed”) was cut from 90 seconds to 60 seconds in release 4.2,
2. One more thing worth flagging: Wen Li, the staff engineer who built the ranking logic and is the only real source for how it works, was away 14–24 August — right in the first ten days after this started. That’s not a data point about the four responders, but it may explain why nobody caught this sooner.

### 9.

Do all of those three things you would do differently

### 10.

Pick one responder from the file who went quiet, and ask Claude to follow their whole month, week by week, in plain English.

### 11.

Once somebody's gone quiet, what would have to happen for them to start getting pinged again?

### 12.

provide a visual timeline of events for the undertow including any relevant data (credit to Kate for suggesting this as a prompt)

### 13.

Help me prepare a single sentence describing in very plain language what happened to one of the people I picked. This is to be shared with my group. If one of these people came to me, how can I explain what went wrong?

### 14.

Other observations people have made:

* After 4.2 cut the answer window to 60 seconds, Vesper's missed pings dragged her score down until Dispatch went from about 14 pings a week to one, and nobody filed a ticket.
* Right after the 12 Aug update, Vesper started missing a lot of pings, and the system then mostly stopped asking Vesper to take callouts, even in Vesper's own area, where there was still plenty of work.
* Farlight was a reliable respondent until the august 12th, when she started receiving less pings and missed more than usual, which eventually caused no pings to go her way. 
* Vesper missed or declined callouts outside his area that he normally wouldn't have taken anyway. After this happened, he stopped getting pings at all. His handler Dot never filed a complaint because she thought they were just 'quiet' weeks.
*
