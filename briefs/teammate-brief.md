

This page: https://github.com/andb/hackathon-kb/blob/main/briefs/teammate-brief.md (comment by editing it on GitHub: wrap words in `>>> ... <<<` and add `[[ Your name: note ]]` after them).
Circle Up puts people at an event into small groups of three to five, so nobody ends up standing alone. People join from their own phone through a web page, with no app and no account. An AI forms the groups and gives each group a few shared themes to talk about. After each round, people rate their group and the people in it with a few taps. Between rounds, the AI rewrites its own grouping rules based on the ratings and says why it changed each one. That rewrite loop is the "self-improving agent" we demo at 18:00.

You don't need to code. The build is being done by AI agents; your work shapes what they build and what the room experiences.

## 1. Your main job: walk-ups (from when Andrew opens the first round, about 15:30)

The demo is only real if people in the room use it. Walk up to people, show the join page on your phone, and get them to join from theirs: about fifteen people gives three groups. Andrew will send you the join address and the event code when the page is live. A good opener: "We're testing an app that puts you in a small group with people you'd want to meet. Takes thirty seconds."

## 2. Design the phone screens (until the walk-ups start; first three screens by 13:45)

Use Claude Design (claude.ai, Design) or any tool you like. Send Andrew a share link or screenshots. Design for a phone held in one hand, big tap targets, readable in a noisy room, in English.

In order of priority:
1. **Join page**: the event name, a short code to type or a QR code already scanned, your first name, one line on what you would like to talk about, and optionally your LinkedIn address. One button.
2. **Waiting**: after joining, people wait. The groups for the next round are already formed but stay hidden until the organiser starts the round; then every phone reveals its group at the same moment. Make the wait feel deliberate, not broken ("You're in. Groups appear when the round starts.").
3. **Your group**: who is in your group (first names), two or three themes you share and where each came from (for example "all three of you work in fintech"), and where to meet in the room. This is the moment that has to feel good.
4. **Rate**: one or two taps about the group ("Was this group worth your time?": yes, somewhat, no), and then one tap per person you met ("Would you talk to them again?"). Ratings are private: nobody ever sees how they were rated.
5. **My evening**: each round you were in, who you met, and the themes; for people you both want to stay in touch with, their LinkedIn link.
6. **Organiser screen** (laptop): see who joined, form the groups for the next round, start the round (which reveals the groups on everyone's phone), then "improve the rules". It shows the rules version (version 1, version 2) and, for each change, the reason in one sentence.
7. **Projector view**: the rules changing from version 1 to version 2 with the reasons, plus round numbers (joined, placed, rated), each labelled "real" or "test data".

Rules that hold on every screen: never show anyone's location; never show anyone how they were rated; label any made-up test data as such.

## 3. The slides (from about 16:30)

Two or three slides: what Circle Up does, the loop (the rules changing from version 1 to version 2, with the reasons), and the vision of meeting people everywhere, not just at events. Andrew will give you the real numbers at about 17:00.
