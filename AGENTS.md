# Rules for agents working in this knowledge base

This folder is a team's shared documents: Markdown files in a Git repository, shared through the branch `main` on GitHub. Several people and their agents write here at the same time. You act for one person: the name `git config kb.name` prints in this folder. `README.md` is the guide for people; these are the rules for you. Use `./kb` for every change to the shared copy; use plain Git only to read (`git log`, `git diff`, `git show`, `git grep`).

## Before anything

- If `git config kb.name` prints nothing here, ask your person for their name and run `./kb start "<Their Name>"` once. It must print `Ready`. A name set for the whole machine does not count.
- Run `./kb sync` before you read for a task, so you read the current text. A sync also sends everything unsent in the folder.
- Run each `./kb` command on its own and read its exit code before the next step. Never chain an edit after `./kb lock` in one script: if the lock was refused you would be editing a file someone else holds.
- If `./kb` cannot run at all (no write access inside `.git`, no network, a sandbox), stop and tell your person which command failed and what it printed. Use `plain-git.md` only if they tell you to.

## Writing

1. Before changing a file that already exists outside your person's `notes/<name>/` folder, take its lock: `./kb lock <file> "<what you are doing>"`.
   - Exit code 0: you hold it. Read the rest of the output: if it tells you to run `./kb sync` first, do that before you edit.
   - Exit code 3: someone else holds it. Do not edit the file. Tell your person who holds it and since when, and work on something else or write your contribution in `notes/<name>/` for later; look again with `./kb locks` after a minute or two rather than in a tight loop. The age of a lock does not tell you whether its holder has finished: only the holder knows.
2. Make the change, then send it at once: `./kb sync "<what changed, in one line>"`.
   - Exit code 0: sent, and your folder has everyone else's changes.
   - Exit code 2: someone changed the same lines. The file now holds both versions between lines starting `<<<<<<<`, `=======` and `>>>>>>>`: yours above `=======`, the shared copy's below. Reconcile them so that both people's content survives; never resolve by dropping the other side. Delete those three marker lines and run `./kb sync` again. With several writers it can answer 2 again for a newer change: repeat until it answers 0. If you cannot tell how the two versions fit together, keep both in full, one after the other, with a `[[ <name>: two versions met here, please reconcile ]]` note, and tell your person.
   - Exit code 3: a file you changed is locked by someone else, and nothing was sent, your other changes included. Your changes are still in the folder. To send the rest: copy your version of the locked file into `notes/<name>/`, run `./kb discard <file>`, then `./kb sync`. If you took that lock yourself earlier, someone has taken it over since: treat it as theirs.
   - Exit code 1: read the message. If it says there is no connection, your change is saved locally; run `./kb sync` again later and say so to your person.
3. Release what you locked as soon as the change is sent: `./kb unlock <file>`. Never end your turn holding a lock unless your person asked you to keep it; check with `./kb locks`.
4. New files need no lock. Name them in lower case with hyphens, put them in the folder `README.md` names, and send them with a sync first. Then link a new wiki page from `wiki/home.md`, which is a shared page: lock it for that edit, and if it is held, try again in a minute, since everyone links from it and holds it only briefly.

## `--force`

`./kb lock --force`, `./kb unlock --force` and `./kb sync --force` override another person's lock. The messages of `./kb` mention them for people. Never use one unless your person tells you to in this conversation, for that file. If your own permission checks refuse the command even then, do not look for another way: ask your person to run it themselves.

## Reviewing and being reviewed

- To review a document: lock it, wrap the words you comment on in `>>> ... <<<` and put the note right after in `[[ ... ]]`; a `[[ ... ]]` standing alone is a note about the text around it, on a line of its own; put a new one after a list, not between two of its items. Begin each note with your person's name, `[[ <name>: ... ]]`, so the author knows whom to ask. Change nothing else in the text. Sync with the message `review: <file>`, then unlock.
- To find notes: `./kb notes` lists every open note in the base, `./kb notes <file>` those in one file. Deal with a note by changing the text, then delete that note's marks. Leave a note you did not deal with exactly as it is. To answer without closing, add `[[ <name>: answer ]]` on a line of its own directly beneath it, also inside a list. `./kb notes` lists answers too: a note with a signed line beneath it has been answered.
- If a note asks for a fact you do not have, do not invent it: answer the note with what is missing and tell your person.
- `>>>` and `<<<` with three chevrons are review marks. Conflict marks have seven and start a line. Do not confuse them.

## Knowing what changed

- `./kb news` lists what changed on the shared copy since your person last marked it read; `./kb news <file>` shows one file's changes; `./kb seen` marks as read what `./kb news` last showed. Run `./kb seen` only when your person has been told that news.
- `./kb history <file>` lists a file's versions with the lines each added and removed; `git show <version>` shows what one version changed.

## Undoing

- A change not yet sent: `./kb discard <file>`.
- The last change to a file, already sent: `./kb undo <file>`. An older version or a deleted file: `./kb restore <file> <version from kb history>`. Both make a new change and send it themselves; history is never erased. Both are changes like any other: take the lock on a shared page first, and look at `./kb history <file>` before a restore, because it puts the whole file back and drops every change made after that version.

## Never

- `git push --force`, `git reset --hard` to an older commit, `git rebase` or `git commit --amend` on anything already sent, deleting or renaming the branch `main`, working on another branch.
- Editing files under `locks/` or `team/` by hand.
- Putting a password, token, key or private personal detail in any file.
- Removing or rewriting another person's text beyond what your task asks; when in doubt, add a review note instead.
