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

### Editing Previous Commits
By selecting the `edit` option during interactive rebase it is possible to easily edit previous commits. Changes commited with the `--amend` option will be added to the commit.
```bash
git commit --amend --no-edit
```
Beware of merge conflicts when editing files that are being modified in a future commit.