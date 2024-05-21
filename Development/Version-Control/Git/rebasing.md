# Rebasing

## Interactive
Using `git rebase -i` the interactive rebase can be started. This makes it possible to reword, squash, edit, etc previous commits. A text editor will be opened, listing the possible options.

The number of commits to go back can be set relative to the current `HEAD`, or all commits can be selected using the `--root` argument:
```bash
git rebase -i --root
```
Select the last three commits:
```bash
git rebase -i HEAD~3
```
