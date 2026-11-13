# Circle Up screens specification, 2026-11-13 13:32

Sources, in authority order: the accepted teammate brief (`private/reports/channel/2026-11-13-1245-teammate-brief-design.md`, called "the brief"), the build plan (`kit/idea-group-matching.md`, called "the plan", with prompts A, B, B2, C and D), and the builder prompts (`kit/prompts.md`). Where the brief and the plan describe the same thing differently, the line starts with `CONFLICT:` and gives both; nothing here resolves one. "Plan only" and "brief only" mark an element only one source has. Your own `[[ ]]` notes in `kit/intent-group-matching.md` are quoted inside open questions only; the replies there are unreviewed and are not used here.

## How to review

- Wrap any text you want to comment on in `>>> ... <<<` and put your note right after it in `[[ ... ]]`; a standalone `[[ ... ]]` is a note about the text around it.
- Unmarked text counts as not yet reviewed, not as approved. If you stop partway, say how far you got.
- Answer the open questions at the end with one word each, either inline in `[[ ]]` or in chat.

## Rules that hold on every screen

- Never show anyone's location (brief). The meeting spot ("where to meet in the room") is a fixed place in the room the organiser names, not a person's location.
- Never show anyone how they were rated (brief); ratings are stored as numbers without a name or alias (plan, prompt B2).
- Label made-up data (brief: "test data"; the plan's words are "synthetic roster", "simulated ratings" and "deterministic split"; see the label conflict under Projector).
- Participant text is never rendered as HTML, is capped at 140 characters per field, and is never logged or sent to analytics (plan, prompt A).
- Phone screens: one column, large tap targets, readable without zooming at 375 pixels wide, readable in a noisy room, in English (brief and plan agree).
- Look: the teal brand colour and the Fraunces heading font from step 4 of `kit/prompts.md`, built only from the shadcn components in `src/components/ui` and Tailwind.
- App title "Circle Up" on the join page and on stage (the team name; neither source fixes the app's title).

## 1. Join

- Purpose: someone at the event joins from their own phone in about thirty seconds, with no app and no account.
- Route: `/join`, and `/` renders the same page (server-rendered, not a redirect) (plan, prompt A).
- Shows, top to bottom:
  - App title "Circle Up" (team name).
  - Event name (brief only: the plan's event has a four-digit code and three spots but no name, so there is no source for it yet).
  - The plan's four-line notice above the form (plan only): "What you type is sent to Featherless, an AI inference provider, to form groups." "Your group sees your alias and nothing else you typed." "Your text is deleted when you leave a round or the organiser ends the event; the organiser may download a backup and deletes that file after the event." "Ratings are stored without your name or alias and their counts are sent to PostHog, an analytics service."
  - Event code field, typed, or already filled from a scanned QR code (brief). The plan's QR on `/organiser` encodes the join address only, with no code in it.
  - CONFLICT: name field. Brief: "your first name". Plan: "alias".
  - CONFLICT: what you bring. Brief: one line, "what you would like to talk about". Plan: four fields, topic, offer, need, and a preference with three choices (help, peers, exploration).
  - CONFLICT: LinkedIn. Brief: an optional LinkedIn address field. Plan: no such field; the only profile input is prompt D's optional prefill, where linkedin.com is listed but disabled.
  - Prefill field and button "Paste the address of your own public profile or personal site to prefill" (plan only, prompt D, optional), with two more notice lines.
  - One submit button (brief and plan agree; label not fixed by either).
- Actions:
  - Submit: validates the code, creates the participant (or updates the same one if this browser joined before, by an id in localStorage), then shows Waiting.
  - Prefill (plan only, prompt D): reads the pasted page once through Apify and fills the fields for the person to edit; nothing is submitted automatically.
- States:
  - Empty: the form, with the code filled if it came from the QR.
  - Loading: submit disabled while the request runs (neither source specifies more).
  - Error: wrong code answers "check the code on the screen"; rate limit answers "too many joins, try again in a minute"; during a synthetic round a real join answers "a demo round is running, join after it" (plan).
  - Before the round: joins open. After Lock: new arrivals see "next round" (plan).
- Never shows: anyone else's name or text; any count of who has joined.
- Build stage: prompt A, deployed before round 1; in the 18:00 cut. Prompt D's prefill is optional and third in the plan's cut order.

## 2. Waiting

- Purpose: after joining, the person knows they are in and that the wait is deliberate, not broken.
- Route: a state of `/join`, not its own address (plan, prompt A: "then a waiting state that polls every three seconds").
- Shows, top to bottom:
  - Confirmation line, for example "You're in. Groups appear when the round starts." (brief).
  - The person's own name and round number (neither source specifies; omit unless you want it).
  - Withdraw button, available from joining onward (plan only, prompt A).
- Actions:
  - Withdraw (plan only): before Publish, marks the person withdrawn and, if groups were formed, clears them so the organiser forms them again.
  - The page polls the server every three seconds; no tap needed.
- States:
  - Before the round: groups may already be formed but stay hidden (brief); in the plan they are formed after Lock and hidden until Publish.
  - CONFLICT: reveal moment. Brief: the organiser "starts the round" and every phone reveals its group "at the same moment". Plan: the organiser presses Publish, and each phone sees it on its next three-second poll, so reveals spread over up to three seconds.
  - After the round starts: the screen switches to Your group.
  - Between rounds: a person who tapped "Stay for the next round" returns here for the next round (plan, prompt A carries them over).
  - Error: if the server restarts, memory is empty and the person must join again (plan); no message for this is specified.
- Never shows: the groups before they are revealed; who else has joined.
- Build stage: prompt A (waiting and polling), deployed before round 1; in the 18:00 cut.

## 3. Your group

- Purpose: the moment that has to feel good: who you are with, what you share, and where to go (brief).
- Route: a state of `/join` after Publish (plan, prompt B).
- Shows, top to bottom:
  - CONFLICT: group size. Brief: groups of three to five. Plan: four to six, or three when fewer than eight people are in.
  - Where to meet: the spot's name and description, one of three spots the organiser edits on `/organiser` (brief "where to meet in the room"; plan "spot").
  - CONFLICT: themes. Brief: two or three themes you share, each with where it came from (for example "all three of you work in fintech"). Plan: one shared topic of five words or fewer plus one sentence on why these people fit; no list of themes and no sources.
  - CONFLICT: what a theme's source may reveal. Brief's "where each came from" shows facts from members' answers to the group. Plan's join notice promises "Your group sees your alias and nothing else you typed."
  - CONFLICT: names. Brief: the other members' first names. Plan: the other members' aliases.
  - Opening question (plan only, prompt B).
  - CONFLICT: "I'm here" button. Brief: none. Plan: a button "I'm here" setting the arrived flag, with arrived counts on `/organiser`.
  - Withdraw button (plan only).
- Actions:
  - "I'm here" (plan only): marks the person arrived.
  - Withdraw (plan only): after Publish, triggers a model call that repairs only the affected group; the phones whose group changed show "your group changed".
  - Clarifying question (plan only): a person left unmatched sees the model's question; their answer triggers a model call that places them in a group or explains why not.
- States:
  - Before the round: not shown (Waiting instead).
  - Loading: none on the phone; group forming happens on `/organiser`.
  - Unmatched (plan only): shows the clarifying question instead of a group.
  - Group changed (plan only): "your group changed" and the repaired group.
  - After the round: when the organiser asks for ratings, or ten minutes after Publish, the screen moves to Rate (plan).
  - Synthetic round: every screen says the people are synthetic (plan).
- Never shows: anyone's location; anything a member typed beyond what the themes conflict above settles; how anyone was rated.
- Build stage: prompt B, deployed before round 1; in the 18:00 cut. The clarifying question is cut fifth and the withdrawal repair sixth in the plan's cut order.

## 4. Rate

- Purpose: a few private taps after each round, which feed the agent's rewrite of its rules (brief and plan agree this is never cut).
- Route: a state of `/join` (plan, prompt B2).
- Shows, top to bottom:
  - "Was this group worth your time?" with yes, somewhat, no (brief and plan agree; stored as 2, 1, 0).
  - CONFLICT: talk again. Brief: one tap per person met, "Would you talk to them again?". Plan: one question about the group, "Would you talk to one of them again?" (yes, no), stored against the round and group number only, "never against the person".
  - Button "Stay for the next round" after rating (plan only).
- Actions:
  - Each tap records its answer; a second rating by the same person in the same round is refused (plan).
  - "Stay for the next round" (plan only): carries the person over; anyone who does not tap it leaves at the round's end and their text is deleted (plan, prompt A).
- States:
  - Before: appears when the organiser presses "Ask for ratings" or ten minutes after Publish (plan).
  - After rating: a thank-you plus the stay button (wording not specified).
  - Already rated: the refusal (wording not specified).
  - Simulated ratings (plan only, prompt C): in a synthetic round the organiser can fill ratings by a fixed rule, labelled "simulated ratings" wherever they appear.
- Never shows: anyone's ratings, of the group or of a person, to anyone; tallies appear only on `/organiser`.
- Build stage: prompt B2, deployed before round 1; in the 18:00 cut and never cut.

## 5. My evening

- Purpose: each person keeps a record of the evening: each round, who they met, the themes, and the LinkedIn link of people they both want to stay in touch with (brief).
- Route: not in the plan yet.
- Shows, top to bottom:
  - One block per round the person was in: round number, the group's members' names, the themes (brief). Source in memory for the current round only; see conflicts below.
  - For each person met where both tapped "would talk again": their LinkedIn link (brief). No source in the plan: it collects no LinkedIn address and no per-person tap.
- Actions: open a LinkedIn link (brief). Nothing else specified.
- States:
  - Empty: before the first round is revealed, nothing to show (wording not specified).
  - CONFLICT: earlier rounds. Brief: every round you were in. Plan: participants who leave at a round's end have their text deleted at that moment, and "End event and delete everything" deletes every group's text, so earlier rounds cannot be shown after either.
  - Error: memory is lost on restart, so the record is lost with it (plan's single-process memory).
- Never shows: anyone's LinkedIn unless both people tapped yes for each other (brief); how anyone was rated.
- Build stage: not in the plan yet; not in any prompt, so not in the 18:00 cut unless you add it.

## 6. Organiser

- Purpose: the organiser, on a laptop, sees who joined, forms the groups, starts the round, and has the agent improve its rules (brief).
- Route: `/organiser`, behind `ORGANISER_SECRET`; without the cookie it shows only a password form (plan, prompt A).
- Shows, top to bottom (plan; the brief names the starred items):
  - Login form, until the secret is entered.
  - Round number and event code, large.
  - QR code for the join address, with an input to set the address phones reach.
  - Live count of joined people, aliases only, never their text (brief "see who joined"*).
  - "synthetic roster" label whenever the round is synthetic.
  - The three spots, editable.
  - Buttons Lock round, Form groups*, Publish, "Download event (JSON)", "End round and start the next", "End event and delete everything".
  - CONFLICT: the start button's name. Brief: "start the round" (which reveals the groups). Plan: "Publish".
  - Seed button and, in a synthetic round only, each participant with an "open as this participant" link and QR (plan only).
  - While a model call runs: "Forming groups with the model, this can take 30 seconds" with a spinner, buttons disabled (plan only).
  - Groups by spot, with arrived counts and the round's rules version; "deterministic split" and "Try the model again" if the model failed (plan only).
  - "Ask for ratings" button; per group the rating tallies and averages, per round the average worth (plan only).
  - "Improve the rules"* button, enabled once the latest model-formed round has ratings.
  - Rules version (version 1, version 2)* and each change with its reason in one sentence* (brief); the plan shows side by side the previous version, the evidence table (with "matches claimed by the model"), the new version and each change with its reason, a reason without a number marked "no number cited", then the history of versions with each round's average worth.
  - "Restore history (JSON)" (plan only, prompt B2).
  - "Simulate ratings" in a synthetic round only, the PostHog read-back numbers, and the agenda read (plan only, prompt C, laptop only).
- Actions: each button does what its label says, as specified in prompts A, B, B2 and C; the organiser's browser also downloads a backup by itself after Publish, when ratings close and after "Improve the rules".
- States:
  - Logged out: password form only; five attempts a minute.
  - Empty: round 1, nobody joined.
  - Loading: the model-call progress state above.
  - Error: "deterministic split" with "Try the model again"; a rejected rewrite keeps the previous version and shows why; "model unreachable, groups formed by the deterministic split" (prompt C).
  - Before the round: Lock, Form groups, Publish. After: ratings, "Improve the rules", "End round and start the next".
  - CONFLICT: when round 1 opens. Brief: about 15:30, when walk-ups start. Plan: about 15:15, right after the last deploy.
- Never shows: any participant's text beyond aliases; anyone's location; individual ratings (only tallies per group).
- Build stage: prompts A, B and B2, deployed before round 1, in the 18:00 cut; the prompt C items on the laptop only, after round 1, as stretch in the plan's cut order.

## 7. Projector / stage

- Purpose: the room and the judges see the rules change from version 1 to version 2 with the reasons, and the round's numbers (brief).
- Route: `/stage`, organiser only (plan, prompt C item 6).
- Shows, top to bottom:
  - CONFLICT: content. Brief: rules version 1 to version 2 with the reasons, plus round numbers (joined, placed, rated). Plan: the organiser screen on the left and two phone-shaped frames on the right, each showing one participant's phone, with a picker under each.
  - CONFLICT: real rounds. Brief: implies the real rounds' numbers. Plan: `/stage` works only in a synthetic round and otherwise says "stage view needs a synthetic round"; for real rounds the plan's stage uses `/organiser` and a PostHog chart of ratings by rules version.
  - CONFLICT: labels. Brief: each number labelled "real" or "test data". Plan: "synthetic roster", "simulated ratings", "deterministic split" and "from simulated ratings".
- Actions: plan only: a participant picker under each phone frame. Nothing on stage presses a button that calls the model (plan).
- States:
  - Not synthetic (plan): "stage view needs a synthetic round".
  - Before groups: frames default to two members of one group once groups are formed (plan).
  - Error: none specified.
- Never shows: anyone's location; how any person was rated; participant text.
- Build stage: prompt C item 6, laptop only and never deployed; the phone frames are fourth in the plan's cut order, so stretch, not in the 18:00 cut as a deployed screen.

## Open questions for the operator

1. Join fields: brief (first name, one line, optional LinkedIn) or plan (alias, topic, offer, need, preference)?
2. Show an event name on Join (needs a name field on `/organiser`): yes or no?
3. Should the join QR carry the event code so nobody types it: yes or no?
4. Keep the plan's four-line privacy notice on Join: yes or no?
5. Reveal: is the plan's three-second poll close enough to "the same moment": yes or no?
6. Group size: brief (three to five) or plan (four to six)?
7. Your group: plan's one topic or brief's two or three themes with sources?
8. May a theme's source show members' answers to their group (the plan's notice says only the alias is shown): yes or no?
9. Keep the "I'm here" button (your note: "I don't see the value"): keep or drop?
10. Keep the opening question on Your group: yes or no?
11. Keep Withdraw on the phone: yes or no?
12. "Talk again" tap: per person (brief) or per group (plan)?
13. Keep "Stay for the next round" after rating: yes or no?
14. Build My evening for 18:00: yes or no?
15. My evening keeps earlier rounds for people who leave (the plan deletes their text at round end): yes or no?
16. Start button label: "Start round" or "Publish"?
17. Round 1 opens at: 15:15 or 15:30?
18. Projector content: brief (rules and numbers) or plan (organiser screen beside two phones)?
19. Projector works in real rounds: yes or no?
20. Label wording: "test data" or the plan's "synthetic" and "simulated"?
