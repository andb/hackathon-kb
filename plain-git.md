# The same in plain Git

For when `./kb` cannot run at all; otherwise use `./kb`, which does these steps with the checks built in. Set your name once in this folder: `git config user.name "Your Name"`. Run one step at a time and read what Git answers before the next; apart from the lock block below, never join several steps into one command. "Rule 3" and "rule 4" are in `README.md`.

Whenever a push is refused because the shared copy has moved on: `git pull --no-rebase --no-edit`, then `git push` again. With several writers that can take a few rounds.

- **Sync:** `git add -A && git commit -m "what I did"`, then `git pull --no-rebase --no-edit`, then `git push`. If the pull says CONFLICT: edit the named files as in rule 4 (in each marked part your lines are above `=======` and the shared copy's are below; keep both people's content), then `git add -A && git commit --no-edit`, then push. `git merge --abort` steps back.
- **Is it locked?** `git pull --no-rebase --no-edit`, then look for `locks/<path>.lock` (for `wiki/page.md`: `cat locks/wiki/page.md.lock`). If the file exists, the person named in it holds the lock. Never write over or delete someone else's lock file: Git will not stop you.
- **Lock:** run this block exactly as it is, with your file in its first line. It pulls, and writes the lock only if nobody holds it.

  ```
  f=wiki/page.md; why="what I am doing"; l="locks/$f.lock"
  if ! git pull --no-rebase --no-edit; then echo "STOP: the pull failed, you have no lock"
  elif [ -e "$l" ]; then echo "STOP: it is locked:"; cat "$l"
  else
    mkdir -p "$(dirname "$l")"
    printf 'file: %s\nholder: %s\nsince: %s\nepoch: %s\nnote: %s\n' "$f" "$(git config user.name)" "$(date -u +%Y-%m-%dT%H:%M:%SZ)" "$(date +%s)" "$why" > "$l"
    git add "$l" && git commit -q -m "lock: $f ($(git config user.name))" -- "$l" && git push && echo "LOCKED: it is yours"
  fi
  ```

  Edit the page only after it has printed `LOCKED`. If it printed neither `STOP` nor `LOCKED`, the push was refused because someone sent something in between: run `git pull --no-rebase --no-edit`. If that pull is clean, `git push`, and the lock is yours once the push succeeds. If it reports a CONFLICT in the lock file, someone else took the lock first: `git merge --abort`, then `git revert --no-edit HEAD` to take your lock back, then pull and push. You do not hold it.
- **Unlock** (only a lock whose `holder:` line is you): `git pull --no-rebase --no-edit`, then `git rm -q locks/wiki/page.md.lock`, then `git commit -q -m "unlock: wiki/page.md (Your Name)"`, then `git push`.
- **A forgotten lock:** taking it over is a person's decision after asking the holder (rule 3). Then: pull, `git rm` the lock file, commit with the message `unlock: wiki/page.md forced by Your Name, was held by Their Name`, push, and lock as above.
- **What changed:** before pulling, `git fetch && git log --stat HEAD..origin/main`. Since a known point: `git log --stat <version>..origin/main`.
- **Review notes:** `git grep -n -e '\[\[' -e '>>>' -- '*.md' ':!README.md' ':!AGENTS.md' ':!plain-git.md'`.
- **Undo the last change to a file:** pull first. `git log --oneline --stat -- <file>` lists its versions, newest first, with what each changed. Pick the version you want back (usually the second in the list), lock the file if it is a shared page, then `git checkout <that version> -- <file>`, commit, push. This puts the whole file back as it was at that version, so check with `git diff <that version> HEAD -- <file>` that nothing but the mistake goes.
