# Git

- `git checkout develop`

- `git checkout -b release/1.2.3`

- `git push -u origin release/1.2.3`

- `git add .`
- `git commit -am "message for the commit"`

- `git diff HEAD^ HEAD`
  > difference between previous and current commit

`gh pr create --title "master <> release" --body "Merge Release into master" --base "parsable/gutenberg:master"`

`gh pr create --title "develop <> release" --body "Merge Release into develop" --base "parsable/gutenberg:develop"`

- `git status -sb`

  > better look for diff

- `git push —set-upstream origin {whatever branch}`

- `git init`

  > inside a folder, create a new git repo

- `git log --oneline --graph --decorate`

- `git diff`
- `git diff HEAD`
