#[Pro Git][1]

## 1.3 Git Basics
> The major difference between Git and any other VCS (Subversion and friends included) is the way Git thinks about its data.

Most other systems store information as a list of file-based changes. Git thinks of its data more like **a set of snapshots** of a mini **filesystem**

> Git has **Integrity**.

Everything in Git is check-summed before it is stored and is then referred to by that checksum. Checksum is a 40-character string composed of hexadecimal characters and calculated by the SHA-1 algo. 

The three states:

1. modified
2. staged
3. committed 

![lifecycle](res/lifecycle.png)

## 1.5 Git Config

Configure files:

* /etc/gitconfig
	* git config --system
* ~/.gitconfig
	* git config --global
* .git/config

Check setttings: `git config --list`

## 1.6 Git Help

* `git help <verb>`
* `git <verb> --help`
* `man git-<verb>`

## 2.2.3 Staging Modified Files

`git add <FILE>`, BANG!

**Note**:the status of a file can be both staged and unstaged. If it is commited now, the version of the file is the version of the staged file, not the one currently working on.

Git can also stage certain parts of a file, that is **stage patch**. Check interactive staging: `git add -i`.

## 2.2.4 ignoring files

Sample **.gitignore** file:

```
# a comment  this is ignored
*.a       # no .a files
!lib.a    # but do track lib.a, even though you’re ignoring .a files above
/TODO     # only ignore the root TODO file, not subdir/TODO
build/    # ignore all files in the build/ directory
doc/*.txt # ignore doc/notes.txt, but not doc/server/arch.txt
```
## 2.2.5 Viewing Staged and Unstaged Changes

Roughly: `git status`.

Details: 

* changes of not staged: `git diff`
* staged changes: `git diff --cached` or `git diff --staged` for Git version >= 1.6.1

## 2.2.7 Skipping the Staging Area

`git commit -a` will automatically stage every file that is tracked.

## 2.2.8 Removing Files

Delete file **permanently**: `git rm <FILE>`

Stop tracking file: `git rm --cached <FILE>`, that is to remove it from the staging area.

**NOTE**: `git rm` supports file-glob: `git rm log/\*.log`. The backslash(\\) is needed because Git does its own filename expansion.

## 2.3 Viewing Commit History

Useful `git log` **options**:

### `git log -p -2`

`-p`: show diff in each commit.

`-2`: limmit the output to the latest 2 commits.

### `git log --stat`

Show abbreviated status for each commit.

### `git log --pretty[=oneline|short|full|fuller]`

Show commits with less or more infomation.

### `git log --pretty=format:"%h - %an, %ar : %s"`

The selected list of format options:

| Option | Description of Output   |
|:------ |:---------------------   || %H     | Commit hash             |
| %h     | Abbreviated commit hash || %T     | Tree hash               || %t     | Abbreviated tree hash   || %P     | Parent hashes| %p     | Abbreviated parent hashes| %an    | Author name| %ae    | Author e-mail| %ad    | Author date (format respects the date= option) 
| %ar    | Author date, relative| %cn    | Committer name| %ce    | Committer email| %cd    | Committer date| %rr    | Committer date, relative| %s     | Subject

**NOTE**: For "%s" option, check the [bug of zsh][zsh-bug].### `git log --graph`
`--graph` option will show branch and merge history.
Check [A pretty format with color][git-log-format]

### 2.3.1 Limiting Log Output

Limit options:

| Option | Description |
|:------ |:----------- |
| -\<n\>   | show last n commits |
| --since, --after | ... |
| --author | … |
| --committer | … |
| --grep | search for keywords in the commit messages |

**NOTE**: if using `--author` and `--grep` at the same time, you have to add `--all-match`, or the command will match commits with either.

## 2.4 Undoing Things

### Changing Last Commit

`git commit --amend`

**NOTE**: check [Learn GIt Branching][2] for changing some commit in the history.

### Unstaging a Staged File

`git reset HEAD <FILE>` as `git status` indicates.

### Unmodifying a Modified File

`git checkout -- <FILE>` as `git status` indicates.

## 2.6 Tagging

### Listing Tags

`git tag`

`git tag -l 'v1.4.*'`

### Creating Tags

There're 2 types of tags:

* **lightweight**
	* `git tag v1.4.1`
* **annotated**
	* `git tag -a v1.4.1 -m "Hello World"`
	* `git tag -s v1.4.1 -m "signed tag"` to sign tag with GPG

#### Verify signed tags

`git tag -v v1.4.1`. It needs the signer's public key in the keyring.

#### Show tag info

`git show v1.4.1`

#### Tagging past commit

`git tag v1.4.1 9fceb02`

### Sharing Tags

By default, git doesn't tranfer tags to remote servers. Explicitly push tags to a shared server: `git push orign v1.4.1`.

Or push all the tags at once: `git push origin --tags`.

## 2.7 Tips and Tricks

### Git Aliases

```
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
```

[1]: http://git-scm.com/book
[2]: http://pcottle.github.io/learnGitBranching/
[zsh-bug]: https://github.com/robbyrussell/oh-my-zsh/issues/521
[git-log-format]: http://www.jukie.net/bart/blog/pimping-out-git-log

## 3.1 How Git Branch Works?

Single commit repository data:

![img](res/commit-object.png)

**NOTE**:Git doesn’t store data as a series of changesets or deltas, but instead as a series of snapshots.

## 3.2.3 Basic Merge Conflicts

`git mergetool` to visually merge conflicts.

## 3.3 Branch Management

`git branch -v` to see the last commit on each branch.

`git branch --merged` to see which branches are already merged into the branch you’re on

* `git branch --no-merged`

## 3.4.1 Long-Running Branches

Keep branches at different levels of stability:
![img](res/running-branches.png)

## 3.5 Remote Branches

Initial:

![img](res/remote-branch1.png)

After `git fetch origin`:

![img](res/remote-branch2.png)

## 3.5.1 Pushing

`git push origin serverfix:awesomebranch`: **serverfix** is the local branch name and **awesomebranch** is the branch name on the remote project.

## 3.5.2 Tracking Branches

Tracking branches are local branches that have a directed relationship with a remote branch. 

Checking out a local branch from a remote branch will automatically creates a tracking branch: `git checkout -b sf origin/serverfix`. Now the local branch **sf** will automatically push to or pull from **origin/serverfix**.

If let the local branch use the same name as the remote branch, you can use `--track` option: ``git checkout --track origin/serverfix`.

## 3.5.3 Deleting Remote Branches

`git push origin :awesomebranch`

> A way to remember this command is by recalling the `git push [remotename] [localbranch]:[remotebranch]` syntax that we went over a bit earlier. If you leave off the **[localbranch]** portion, then you’re basically saying, “Take nothing on my side and make it be **[remotebranch]**.”

## 3.6.2 More Interesting Rebases

Full git-rebase command:
`git rebase [-i | --interactive] [options] [--onto <newbase>] [<upstream>] [<branch>]`

**--onto** has the exact same effect as `git reset --hard <upstream>` (or \<newbase>).

Check the example of `git rebase --onto master server client` in the book. It means:

> Check out the client branch, figure out the patches from the common ancestor of the client and server branches, and then replay them onto master.

## 3.6.3 The Perils of Rebasing

> Don't rebase the commits that you have pushed to a public repository.
> Treat rebasing as a way to clean up and work with commits before you push them

## 4 Git on the Server

> A remote repository is generally a bare repository — a Git repository that has no working directory. Because the repository is only used as a collaboration point, there is no reason to have a snapshot checked out on disk; it’s just the Git data. In the simplest terms, a bare repository is the contents of your project’s .git directory and nothing else.

## 4.1 The Protocals

* Local Protocal
* SSH Protocal
	* the only network-based protocol that you can easily read from and write to.
	* **main problem**: People must have access to your machine over SSH to access it(no anonymous access), even in a read-only capacity.
* Git Protocal
* HTTP/S Protocal

## Getting Git on a Server

Export an existing repository into a new bare repository — a repository that doesn’t contain a working directory:
> `git clone --bare my_project my_project.git`

This is roughly equivalent to:
> `cp -Rf my_project/.git my_project.git`

## 4.2.2 Github's SSH Access Setup

Create a single ‘git’ user on the machine, ask every user who is to have write access to send you an SSH public key, and add that key to the /.ssh/authorized keys file of your new ‘git’ user. At that point, everyone will be able to access that machine via the ‘git’ user. 

## 4.6 GitWeb

Simple web-based visualization:

> `git instaweb --httpd=webrick` for OSX or `git instaweb` for Linux
> `git instaweb --httpd=webrick --stop` to stop

## 4.7 Gitosis

Access control for git.

## 5.1.3 Dictator and Lieutenants Workflow

It's generally used by huge projects like the Linux kernel.

![img](res/dict-lieu-workflow.png)

## 5.2 Commit Guidelines

### Don't submit any whitespace errors

Use `git diff --check` to check it before really commiting.

### Make each commit logically separate changeset

Try to make sure at least one commit per issue. If some of the changes modify the same file, use `git add --patch` to partially stage files.

### Create quality commit message

General rule:

1. Start with a single line with no more than 50 chars to describe the commit;
2. A blank line.
3. A more detailed explanation: motivation for the change and contrast its implementation with previous behavior.
