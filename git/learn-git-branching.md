#[Learn Git Branching][src]

##git branch

> branch early, and branch often

##git rebase

Both `git merge` and `git rebase` can combine work between branches.

Just as the name "**rebase**" indicates, `git rebase` will make the current branch based on the target branch

Rebase takes a set of commits: "copy" them, them plops them down somewhere else.

> the advantage of rebasing is that it can be used to make a nice linear sequence of commits

##HEAD

> HEAD is the symbolic name for the currently checked out commit.

Normally HEAD points to the branch name. But you can detach it by `git checkout COMMIT`. Then HEAD will points to the commit "**COMMIT**".

##commit hash

You don't need to specify the whole hash, only the prefix with enough characters to uniquely identify the commit. 

##relative refs

Two ways:

1. `HEAD^` or `HEAD^^` …
2. `HEAD~1` or `HEAD~2` …

Common use case: move branches around -- reassign a branch to a new commit with a `-f` option. `git branch -f master HEAD~3`

##reverse changes

There're two ways to achieves this goal: `git reset` and `git revert`.

### `git reset HEAD~2`

"**rewrite**" the commit log/history, as if the commits **after** `HEAD~2` has never been made in the first place.

### `git revert HEAD~2`

If you want to **share** the reversing with others, you neen to commit the reversing, which is what `git revert` does. This command will make a new commit that revert **to** the version of `HEAD~2`.

Locally, in the perspective of the final result, `git reset HEAD~2` is euqal to `git revert HEAD~2`.

##move commits

Two ways:

1. `git cherry-pick <commit1> <commit2> …`
2. `git rebase -i HEAD~4` -- interactively move or omit commits

[src]: http://pcottle.github.io/learnGitBranching/

