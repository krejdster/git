### fdupes

Remove duplicate files
```
$ sudo apt-get install fdupes
$ brew install fdupes
fdupes -rdN ./dir/
fdupes -r ./dir/ --- DRY RUN
```

### JPEG / JPG / Image operations

**SIPS** Resize JPG files (Mac Terminal) - resizes all JPG files in the current directory
```
sips -Z 3000 *.JPG
```

**All PNG in directory to JPEG; remove all PNG from directory**
```
for i in *.png; do sips -s format jpeg -s formatOptions 80 "$i" -o "$i.jpg"; done && rm -rf ./*.png
```

**Downsize all JPG in the directory by replacing**
```
for i in *.jpg; do sips -s format jpeg -s formatOptions 75 --resampleWidth 1800 "$i" -o "$i"; done
```

**exiftool** Remove all EXIF data from all files (DOES NOT recompress the image), include subdirectories starting from the top folder
```
brew install exiftool
exiftool -overwrite_original -r -all= *
# or without overwrite flag
find . type f -name '*.JPG_original' -delete
```

### youtube-dl

Get single YouTube video
```
youtube-dl -f "[ext=mp4][height<=720]" <video-id>
```

Get preferred - 720p, name with date prefix, subs
```
youtube-dl \
--format 'bestvideo[height<=720]+bestaudio/best[height<=720]' \
--retries '3' \
--rate-limit 10240k \
--write-info-json \
--write-thumbnail \
--all-subs \
--convert-subs 'srt' \
--embed-subs \
--output "%(upload_date)s_%(id)s_%(title)s.%(ext)s" \
'CHANNEL_URL'
```

Get Udemy course with autonumber (or without it)
```
youtube-dl --cookies ./cookies.txt https://www.udemy.com/zzzCOURSE_NAMEzzz/ -o "./%(playlist)s/%(chapter_number)s-%(chapter)s/%(autonumber)03d-%(title)s.%(ext)s"

youtube-dl --cookies ./cookies.txt https://www.udemy.com/zzzCOURSE_NAMEzzz/ -o "./%(playlist)s/%(chapter_number)s - %(chapter)s/%(title)s.%(ext)s"
```

Resume YouTube / Udemy downloading starting from particular index
```
youtube-dl -f "[ext=mp4][height<=720]" https://www.youtube.com/user/zzzCHANNEL_NAMEzzz/videos --playlist-start=72
```

### tiktok

Get single profile, latest 50 videos in HD without watermark, up to 3 concurrent downloads, save metadata to CSV files and keep download history so there is no need to redownload history
```
tiktok-scraper user zzzUSERNAMEzzz --download --hd --noWaterMark --asyncDownload 3 --filetype csv --number 50 --store --historypath ./
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

### Git Workflow (branches)

Create a branch based on another branch
```
git checkout -b myFeature development
```

Force push it to another branch
```
git push origin myLocalBranch:myTargetBranchOnRemote --force
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
git commit --amend --author="Your Name <email@gmail.org>"
```

Add new files / updated files to already pushed commit
```
1. Zaktualizuj / dodaj pliki jakie chcesz
2. git add <pliki>
3. git commit --amend
4. Zapisz message bez zmian
5. git push --force
```

Merge two last commits into one
```
$ git rebase -i HEAD~2
> You should see two lines starting with "pick". To proceed with squashing, change the first word of the second line from "pick" to "squash". Then save your file, and quit. Git will squash your first commit into your second last commit.
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
or... (this one is dangerous as it overrides everything you got in stash in your working tree) - this one fully restores both tracked contents and untracked contents
```
git checkout stash -- .
git checkout stash^3 -- .
```

Preview zawartość Stasha
```
git stash show -p stash@{2}
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

---

### ffmpeg

convert MOV to MP4
```
for file in *.mov; do
	FNAME="$(cut -d'.' -f1 <<<"$file")"
	ffmpeg -i "$file" -vcodec h264 -an "$FNAME.mp4"
done
```
