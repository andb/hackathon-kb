# Circle Up: prompts for Claude Design, created 13:59

## How to use

1. Open claude.ai and start a new Design.
2. Paste prompt 0 first and let Claude confirm it understood. Then paste one screen prompt at a time (1 to 6), waiting for each result before the next.
3. When a screen looks right, share the result link with Andrew.
4. Mark anything that feels wrong, confusing or hard to tap, in the chat or in a note to Andrew.

The Rate screen is folded into My evening (prompt 4): feedback on the group and on each person is given there, so there is no separate Rate prompt.

## Prompt 0: shared style and rules (paste first)

```text
We are designing phone mock-ups for "Circle Up", an app that puts people at an event into small groups of three to five so nobody stands alone. People join from their own phone on a web page, with no app and no account. I will send you one screen at a time. Remember these rules for every screen.

Device: a phone held in one hand, 375 pixels wide, one column, scrolling down. Thumb-friendly: big tap targets (at least 48 pixels tall, full-width buttons), important actions near the bottom.
Readable in a noisy, busy room: large text, high contrast, short sentences, no tiny grey text, nothing that needs zooming.
Language: English only.
Look: calm and warm. A teal brand colour, a warm off-white background, rounded corners, generous spacing, and a friendly serif heading font (like Fraunces) with a plain sans-serif for body text. No clutter, no more than one main button per screen.
Privacy rules that hold on every screen: never show anyone's location (a fixed meeting spot in the room is fine, a person's whereabouts is not). Never show anyone how they were rated, and never show a list of who rated whom.
Made-up content: use realistic made-up names and text, and label every piece of made-up data with a small visible tag saying "test data".
Show every screen as a phone frame, and where I ask for an empty or waiting state, show it as a second frame next to the main one.
Reply with the mock-up only, plus one line listing anything you were unsure about.
```

## Prompt 1: Join

```text
Design the Join screen of Circle Up (phone, same style and rules as before).

Purpose: someone at the event joins from their own phone in about thirty seconds, with no app and no account.

Elements, top to bottom:
1. App title "Circle Up" in the heading font, with a one-line promise under it: "Small groups, so nobody stands alone."
2. Event name, for example "Founders Evening, Prague" (test data).
3. Event code field: a short four-digit code, for example 4821 (test data), typed in a large field. Show a second frame where the code is already filled in because the person scanned a QR code.
4. Name field, labelled "Your first name", for example "Maya" (test data).
5. One-line field, labelled "What would you like to talk about?", for example "How small teams decide what to build next" (test data). Maximum 140 characters, show a small character counter.
6. Optional field, labelled "LinkedIn address (optional)", with a helper line: "Shown to the people in your groups, only when they tap your name."
7. A short privacy note above the button: "What you type is used only to form your groups. Your group sees your first name. Nobody ever sees how they were rated." (to confirm)
8. One big full-width button: "Join".

Taps:
- Tapping Join checks the code and takes the person to the Waiting screen.

States to show:
- Empty: the form with all fields blank.
- Code from QR: the code field filled in.
- Loading: the button greyed out and reading "Joining...".
- Error: a wrong code shows "Check the code on the screen" under the code field; too many tries shows "Too many joins, try again in a minute".
```

## Prompt 2: Waiting

```text
Design the Waiting screen of Circle Up (phone, same style and rules as before).

Purpose: after joining, the person knows they are in and that the wait is deliberate, not broken. The groups for the next round are already formed but stay hidden until the organiser starts the round. Then every phone reveals its group at the same moment.

Elements, top to bottom:
1. App title "Circle Up", small.
2. A calm, friendly illustration or animation idea: soft circles gently moving towards each other, in teal. Keep it quiet.
3. Big confirmation line: "You're in."
4. Under it: "Groups appear when the round starts."
5. A small line: "Keep this page open. It changes by itself." Show a subtle pulsing dot to signal the page is alive.
6. The person's own first name and round, for example "Maya, round 1" (test data).
7. A quiet text button at the bottom: "Withdraw".

Taps:
- Withdraw asks "Leave this round?" with two big buttons, "Leave" and "Stay".
- Nothing else needs a tap: the screen changes to Your group by itself when the organiser starts the round.

States to show:
- Main waiting state as described.
- Between rounds: the same screen with the line "Next round starts soon. Your groups from earlier rounds are in My evening."
- Never show who else has joined or what the groups are.
```

## Prompt 3: Your group

```text
Design the Your group screen of Circle Up (phone, same style and rules as before). This is the moment that has to feel good, so make it warm and clear.

Purpose: when the organiser starts the round, the person sees who they are with, what they share, and where to go.

Elements, top to bottom:
1. A short headline: "Your group" with a small round label "Round 1".
2. Where to meet, as a prominent card with a pin-style icon: the spot name and a one-line description, for example "The green sofa by the window - next to the coffee table" (test data). This is a fixed spot in the room, not anyone's location.
3. The other members, as three or four large rounded chips with first names only, for example Priya, Tomas, Lena (test data). Groups are three to five people including the person.
4. "What you share": two or three themes, each as a card with the theme and where it came from, for example "Early-stage fintech: all three of you work in it" and "Hiring first employees: two of you wrote about it" and "Moving to a new city: Lena and you mentioned it" (all test data). (to confirm)
5. An opening question to break the ice, in a soft tinted box, for example "What is the first thing you would build if money was no object?" (test data).
6. A quiet text button at the bottom: "Withdraw".

Taps:
- Withdraw asks "Leave this group?" with "Leave" and "Stay".
- Nothing else needs a tap.

States to show:
- Main state as described.
- Group changed: a banner at the top saying "Your group changed" with the updated members.
- Unmatched: no group yet, a friendly card says "We need one more detail to place you" with one question, for example "Do you prefer meeting people who do the same work as you, or people from other fields?", a text field and a button "Send".
- Never show anyone's location, how anyone was rated, or anything the members typed beyond the themes.
```

## Prompt 4: My evening (feedback on the group and on each person)

```text
Design the My evening screen of Circle Up (phone, same style and rules as before). This screen is also where all feedback is given, so there is no separate rating screen.

Purpose: a personal record of the evening and the one place to give feedback: one rating for each group, and one for each person in it. All ratings are private: nobody ever sees how they were rated.

Elements, top to bottom:
1. Headline "My evening" and a small line "Tap a name to see what they shared."
2. One card per round the person was in, newest first. Show two rounds, round 2 and round 1 (test data). In each card:
   a. Round number and the themes of that group, for example "Round 2 - Early-stage fintech, Hiring first employees" (test data).
   b. Group rating: the question "Was this group worth your time?" with three big buttons side by side: "Yes", "Somewhat", "No".
   c. A list of the group's members, one row each with the first name, for example Priya, Tomas, Lena (test data). Each row has the question "Would you talk to them again?" and three small but thumb-sized buttons: "Yes", "Maybe", "No".
3. After the person has rated the newest round, a big button: "Stay for the next round". Under it a small line: "If you don't tap it, you leave at the end of this round and what you typed is deleted."
4. Show round 1 as already rated: selected answers highlighted with a small tick, and a note "Thanks, your answers are private." Ratings can be given only once per round, so once chosen the buttons are locked.

Taps:
- Tapping Yes, Somewhat or No for the group records it, highlights it and locks it.
- Tapping Yes, Maybe or No on a person row does the same for that person.
- Tapping a person's name (not the rating buttons) opens a small sheet from the bottom showing what that person entered when joining: their first name, their one-line topic, and their LinkedIn address if they gave one, as a tappable link, for example "linkedin.com/in/priya-test (test data)". If they gave none, it says "No LinkedIn address shared". The sheet has a "Close" button. Never show a list of everyone's LinkedIn links on the main screen.

States to show:
- Empty: before the first round is revealed, a friendly empty screen with the line "Your groups will appear here."
- Main state: two rounds, round 2 unrated and round 1 rated.
- The tap-a-person sheet open.
- Earlier rounds stay on this screen until the event ends, so they can still be rated and viewed.
- Never show how anyone was rated, anyone's location, or a list of LinkedIn links.
```

## Prompt 5: Organiser (laptop)

```text
Design the Organiser screen of Circle Up. This one is for a laptop browser (1280 pixels wide), not a phone. Keep the same warm teal look and the "test data" labels. Use large text, because the organiser looks at it while talking to people.

Purpose: the organiser sees who joined, forms the groups, starts the round, and then has the agent improve its grouping rules.

Elements, top to bottom:
1. A password page first: a centred card with "Organiser" and one password field and a button "Enter".
2. After login, a header with the round number and the event code, both large, for example "Round 1" and "Code 4821" (test data). Next to it a QR code box with a small field "Address phones use" showing "circleup.example/join" (test data).
3. A "test data" tag at the top whenever the people are made up.
4. Joined people: a live counter like "15 joined" and a plain list of first names only, for example Maya, Priya, Tomas, Lena (test data). Never show what people typed.
5. Three meeting spots, each an editable row with a name and a one-line description, for example "Green sofa", "Window table", "Bar corner" (test data).
6. Action buttons in the order of use: "Lock round", "Form groups", "Start round" (this reveals the groups on everyone's phone), "Ask for ratings", "Improve the rules", "End round and start the next". Show a plain "Download event backup" button and a red "End event and delete everything" at the far end with a confirm step. (to confirm)
7. While groups are being formed: the buttons are disabled and a spinner reads "Forming groups, this can take 30 seconds."
8. Groups by meeting spot: each group as a card with its member first names and "3 of 4 arrived" style counts (test data), and the rules version, for example "Rules version 1".
9. After ratings: for each group a tally like "Yes 3, Somewhat 1, No 0" and an average, and for the round "Average worth 0.8" (test data). Only totals, never who said what.
10. Rules panel: "Rules version 1" and "Rules version 2" side by side, and under them each change with its reason in one sentence, for example "Group size 4 to 5: groups of five rated higher (average 1.4 against 0.9)" (test data).

Taps:
- Each button does what its label says. "Improve the rules" is greyed out until the latest round has ratings.

States to show:
- Logged out: the password card only.
- Empty: round 1, nobody joined yet, "0 joined".
- Forming groups: the spinner state.
- After the round: ratings, the "Improve the rules" button active, and the rules panel filled.
- Error: a banner "Model unreachable, groups formed by the basic split" with a "Try again" button.
- Never show anyone's location, anything people typed beyond their first name, or individual ratings.
```

## Prompt 6: Projector / stage

```text
Design the Projector view of Circle Up. This is a full-screen 16:9 page shown on a projector in a room, for people sitting far away. Same warm teal look, but bigger: very large type, high contrast, no small text, no buttons anyone has to press.

Purpose: the room and the judges see the grouping rules change from version 1 to version 2, with the reasons, and the numbers of the round.

Elements, top to bottom:
1. Title "Circle Up" at the top left, and the round at the top right, for example "Round 2".
2. Left half: "Rules version 1" in a card, with three or four rules in plain words, for example "Groups of four", "Match on shared topics", "Mix work backgrounds" (test data).
3. Right half: "Rules version 2" in a card, with the same rules and each change highlighted in teal, with the reason in one short sentence under it, for example "Groups of five: they were rated higher (1.4 against 0.9)" (test data). An arrow between the two halves.
4. A numbers strip along the bottom with three big numbers: "Joined 15", "Placed 15", "Rated 12", each with a small tag "real" or "test data" next to it (use "test data" for all three in the main mock-up).
5. Never show names, ratings of any person, locations, or anything people typed.

Taps: none. This screen is only looked at.

States to show:
- Before the improvement: only version 1 on the left and the right card showing "Waiting for the rules to improve".
- After: both versions with the highlighted changes.
- A second frame where the numbers show a mix, for example "Joined 15 real", "Rated 12 test data".
```
