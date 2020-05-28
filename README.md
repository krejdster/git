### youtube-dl

Get single YouTube video
```
youtube-dl -f "[ext=mp4][height<=720]" <video-id>
```

Get Udemy course with autonumber (or without it)
```
youtube-dl --cookies ./cookies.txt https://www.udemy.com/<course_name>/ -o "./%(playlist)s/%(chapter_number)s-%(chapter)s/%(autonumber)03d-%(title)s.%(ext)s"

youtube-dl --cookies ./cookies.txt https://www.udemy.com/<course_name>/ -o "./%(playlist)s/%(chapter_number)s - %(chapter)s/%(title)s.%(ext)s"
```

Resume YouTube / Udemy downloading starting from particular index
```
youtube-dl -f "[ext=mp4][height<=720]" https://www.youtube.com/user/<channel>/videos --playlist-start=72
```

### grep

Zwraca linie w których znajduje się "text". Case sensitive.
```
cat filename.txt | grep "text"
```

Zwraca linie w których nie znajduje się "text". Case sensitive.
```
cat filename.txt | grep -v "text"
```

---

### Git workflow

Reset repository. Removes even ignored files which means it's gonna leave you with completely fresh-like repository.
```
git clean -fdx
```

---

### Git Workflow (commit)

Undo commit
```
git reset HEAD^
```

Remove latest commit from master
```
git reset HEAD^ --hard
git push origin -f
```

Rename commit that was not yet pushed to server
```
git commit --amend
```

Add new files / updated files to already pushed commit
```
1. Zaktualizuj / dodaj pliki jakie chcesz
2. git add <pliki>
3. git commit --amend
4. Zapisz message bez zmian
5. git push --force
```

---

### Git Workflow (cherry picking)

Cherry pick without commiting
```
git cherry-pick -n <commit>
```

---

### Git Workflow (status, diff)

Sprawdź `diff` dla plików `staged`
```
git diff --cached
```

---

### Git Workflow (stash)

Stash apply force
```
git stash show -p | git apply --3
```

Zapisz pliki włącznie ze staged:
```
git stash save --keep-index
```

Nadaj stashowi własną nazwę
```
git stash save 'your name' --include-untracked
```

Merge two or more stashes (można oczywiście zamienić na `git stash apply`)
```
git stash pop
git add . && git commit -am 'WIP'
git stash pop
git reset --soft HEAD^
```

---

### Git Workflow (gitignore)

Untrack files already added to git repository based on .gitignore
```
git rm -r --cached .
git add .
git commit -m ".gitignore fix"
```
