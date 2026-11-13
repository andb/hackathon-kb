# Team knowledge base

Our shared documents: notes, briefs, analyses, decisions, wiki pages. Plain Markdown files in one Git repository. Everyone, person or agent, works in their own copy of this folder and sends changes to the one shared copy on GitHub (the branch `main`). The helper `./kb` does every step in one command; the plain Git behind each is in the last section, in case the helper ever fails.

Web view (read, history, quick fixes): https://github.com/andb/hackathon-kb

## Start (once per machine)

```
git clone https://github.com/andb/hackathon-kb.git team-kb
cd team-kb
./kb start "Your Name"
```

`start` sends one small file to the shared copy to prove your login works. If it prints `Ready`, you are set. Agents: read `AGENTS.md` next. On Windows use Git Bash, or `bash kb ...`.

If the clone asks for a password or says the repository was not found: accept the invitation first (https://github.com/andb/hackathon-kb/invitations), then either log in with the GitHub tool (`gh auth login`, then `gh repo clone andb/hackathon-kb team-kb`) or, if you use SSH keys with GitHub, `git clone git@github.com:andb/hackathon-kb.git team-kb`.

## Every day

| I want to | Command |
|---|---|
| get everyone's changes and send mine | `./kb sync "what I did"` |
| say "I am working on this file now" | `./kb lock wiki/page.md "why"` |
| say I am finished with it | `./kb unlock wiki/page.md` |
| see who is working on what | `./kb locks` |
| see what changed since I last looked | `./kb news`, then `./kb seen` to mark it read |
| see what changed in one file | `./kb news wiki/page.md` |
| list open review notes | `./kb notes` |
| see a file's versions | `./kb history wiki/page.md` |
| take back the last change to a file | `./kb undo wiki/page.md` |
| bring back an older version or a deleted file | `./kb restore wiki/page.md <version>` |
| drop my changes I have not sent | `./kb discard wiki/page.md` |
| check that my set-up is healthy | `./kb check` |

## The rules

1. **Sync before you start and after every piece of writing.** Small and often. Nothing is shared until you sync, and a sync sends everything unsent in your folder.
2. **Lock a shared page before you edit or review it, and unlock when your change is sent.** `./kb lock` also brings your folder up to date, or tells you to sync first; edit only after it has answered. `./kb sync` refuses to send a change to a file someone else has locked. Your own folder under `notes/` needs no lock.
3. **A file you want is locked:** work on something else, or write your part in `notes/<your name>/` and add it when the lock is gone (`./kb locks`). If the lock looks forgotten, ask the holder. No answer: a person, not an agent on its own, takes it over with `./kb lock --force <file>`; the history records who took it from whom.
4. **Two people changed the same lines at once:** `./kb sync` stops and puts both versions in the file between lines starting `<<<<<<<`, `=======` and `>>>>>>>`. Your text is above `=======`, the other person's below. Edit the file so it reads right and keeps both people's content, delete those three lines, run `./kb sync` again. When several people are writing it can stop again on a newer change: do the same again. Nobody's text is lost unless you delete it. `./kb abort` steps back.
5. **A mistake is undone by a new change, never by erasing history.** `./kb undo` and `./kb restore` make that change and send it. Never `git push --force`.
6. **The browser editor and plain `git push` do not check locks.** Before editing on github.com, look in the `locks/` folder.
7. **No passwords, tokens or keys in any file here.**

## Reviewing a document

Lock it, write your notes into the text, sync, unlock:

```
The launch is planned for >>> the first week of March <<< [[ too early: the pilot ends on the 14th ]].

[[ This section needs a source for the numbers. ]]
```

`>>> ... <<<` wraps the words you are commenting on, and `[[ ... ]]` is the note. A `[[ ... ]]` standing alone, on a line of its own, is a note about the text around it. When several people review, begin your notes with your name: `[[ Ana: too early ]]`. The author runs `./kb notes` (or `./kb notes wiki/page.md` for one file), changes the text, deletes the marks that are dealt with, and syncs. To answer a note without closing it, add `[[ Your Name: answer ]]` on a line of its own beneath it.

## Where things go

- `wiki/`: pages that last; `wiki/home.md` is the index, so link new pages from it.
- `notes/<your name>/`: your working notes; yours alone, no lock needed.
- `briefs/`, `decisions/`: one file per brief or decision.
- `locks/` and `team/` are kept by `./kb`; do not edit them by hand.

File names in lower case with hyphens: `pricing-options.md`.

## If `./kb` fails

`./kb check` says what is wrong with your set-up. If `./kb` cannot run at all, `plain-git.md` gives every step above in plain Git.
