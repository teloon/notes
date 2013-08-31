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
|:------ |:---------------------   |
| %H     | Commit hash             |
| %h     | Abbreviated commit hash |
| %T     | Tree hash               |
| %t     | Abbreviated tree hash   |
| %P     | Parent hashes
| %p     | Abbreviated parent hashes
| %an    | Author name
| %ae    | Author e-mail
| %ad    | Author date (format respects the date= option) 
| %ar    | Author date, relative
| %cn    | Committer name
| %ce    | Committer email
| %cd    | Committer date
| %rr    | Committer date, relative
| %s     | Subject

**NOTE**: For "%s" option, check the [bug of zsh][zsh-bug].

### `git log --graph`

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

## Some Usefull Command

`git push -f origin featureA`: specify the `-f` to your push command in order to be able to replace the **featureA** branch on the server with a commit that isn’t a descendant of it.

`git merge --no-commit --squash featureB`: `--squash` option takes all the work on the merged branch and squashes it into one non-merge commit on top of the branch you’re on. `--no-commit` option tells Git not to automatically record a commit. This allows you to introduce all the changes from another branch and then make more changes before recording the new commit. (check the example in 5.2.4)

`git request-pull origin/master myfork`: The `request-pull` command takes the base branch into which you want your topic branch pulled and the Git repository URL you want them to pull from, and outputs a summary of all the changes you’re asking to be pulled in.

`git format-patch -M origin/master`: You use `git format-patch` to generate the mbox-formatted files that you can e-mail to the list. The `format-patch` command prints out the names of the patch files it creates. The `-M` switch tells Git to look for renames.

`git send-email *.patch`: First set up the imap section in **./.gitconfig** file. You can use `git send-email` to place the patch series in the folder of the specified IMAP server.

## 5.3.1 Working in Topic Branch

If you create a branch name based on the theme of the work you're going to try, you can easily remember it. Besides, **namespacing** the branch also helps, e.g. namespace it by who contribute the work: wy/featureA.

## 5.3.2 Applying Patches from E-mail

### Method1: `git apply`

If you received the patch from someone who generated it with the `git diff` or a Unix `diff` command, you can apply it with the `git apply` command.

`git apply featureA.patch`. **NOTE**: `git patch` won't create a commit for you. You must stage and commit the changes manually.

`git apply --check featureA.patch` to see if a patch applies cleanly before actually applying it.

### Method2: `git am`

If the patch is generated by `format-patch`, you can apply it using `git am`.  If you can, encourage your contributors to use `format-patch` instead of `diff`.

`git am featureA.patch`.

If conflict happens, edit the conflict file to resolve it, stage the file and run `git am --resolved` to continue to the next patch.

More intelligent way to resolve: `-3` option to make Git attempt a three-way merge. **Requisite**: you must have the commit that the patch is based on so that the three-way merge can be done.

If you’re applying a number of patches from an mbox, you can also run the am command in **interactive** mode: `git am -3 -i mbox`.

## 5.3.3 Checking Out Remote Branches

Jessica says she has a great new feature in the **ruby-client** of her repository:

```
$ git remote add jessica git://github.com/jessica/myproject.git
$ git fetch jessica
$ git checkout -b rubyclient jessica/ruby-client
```

If you only need the one-time pull and don't need to remember the remote repository, the `git pull` is also ok: `git pull git://github.com/onetimeguy/project.git`

## 5.3.4 Determining What Is Introduced

`git log featureA --not master`: `--not master` will exclude the commits in the master branch.

**NOTE**: To see a full diff of what would happen if you were to merge this topic branch with another branch, `git diff master` will **not** work. For example, if you’ve added a line in a file on the master branch, a direct comparison of the snapshots will look like the topic branch is going to remove that line.

### Get the common ancestor of two commits

`git merge-base contrib master`. Then compare it with the last commit of the branch you're working on.

or **directly** `git diff master...contrib`. `git diff A…B` is equivalent to  `git diff $(git-merge-base A B) B`.

## 5.3.5 Integrating Contributed Work

* Merging Workflows
	* master, develop branch
* Large-Merging Workflows
	* Several long-running branches: e.g. master, next, pu, maint.
* Rebasing and Cherry Picking Workflows

## 5.3.6 Tagging Your Releases

How to distribute the public PGP key used to sign your tags:

1. Export the GPG key
2. `git hash-object` to write a new blob with the content of the key
3. tag the key in the repo
4. share the GPG key via the blob in the repo, just pulling the blob out and inport it into GPG

## 5.3.7 Generating a Build Number

```
$ git describe master
v1.6.2-rc1-20-g8c5b85c
```

{nearest_tag}-{number_of_commits}-{partial_SHA1_value}

## 5.3.8 Preparing a Release


```
$ git archive master --prefix='project/' | gzip > `git describe master`.tar.gz
$ ls *.tar.gz
v1.6.2-rc1-20-g8c5b85c.tar.gz
```
or

```
$ git archive master --prefix='project/' --format=zip > `git describe master`.zip
```

## 5.3.9 The Shortlog

the following gives you a summary of all the commits since your last release, if your last release was named v1.0.1:

```
$ git shortlog --no-merges master --not v1.0.1
Chris Wanstrath (8):
      Add support for annotated tags to Grit::Tag
      Add packed-refs annotated tag support.
      Add Grit::Commit#to_patch
      Update version and History.txt
      Remove stray ‘puts‘
      Make ls_tree ignore nils
Tom Preston-Werner (4):
      fix dates in history
      dynamic version method
      Version bump to 1.0.2
      Regenerated gemspec for version 1.0.2
```

## 6.1.2 Short SHA

If you pass `--abbrev-commit` to the git log command, the output will use shorter values but keep them unique.

## 6.1.4 Branch References

See which specific SHA a branch points to: `git rev-parse topic1`.

## 6.1.5 RefLog Shortnames

**RefLog** — a log of where your HEAD and branch references have been for the last few months.

See RefLog: `git reflog`. Check some specific log: `git show HEAD@{5}`.

Time restrict: `git show master@{yesterday}`.

See reflog formatted like the `git log` output:

```
$ git log -g master
commit 734713bc047d87bf7eac9674765ae793478c50d3
Reflog: master@{0} (Scott Chacon <schacon@gmail.com>)
Reflog message: commit: fixed refs handling, added gc auto, updated
Author: Scott Chacon <schacon@gmail.com>
Date:   Fri Jan 2 18:32:33 2009 -0800
    fixed refs handling, added gc auto, updated tests

commit d921970aadf03b3cf0e71becdaab3147ba71cdef
Reflog: master@{1} (Scott Chacon <schacon@gmail.com>)
Reflog message: merge phedders/rdocs: Merge made by recursive.
Author: Scott Chacon <schacon@gmail.com>
Date:   Thu Dec 11 15:08:43 2008 -0800
    Merge commit ’phedders/rdocs’
```

## 6.1.7 Commit Ranges


### Double Dot

`master..experiment`: all commits reachable by **experiment** that aren’t reachable by **master**.

### Multiple Points

These three commands are equivalent:

```
$ git log refA..refB
$ git log ˆrefA refB
$ git log refB --not refA
```

This is nice because with this syntax you can specify more than two references in your query, which you cannot do with the double-dot syntax. For insance, if you want to see all commits that are reachable from refA or refB but not from refC, you can type one of these:

```
$ git log refA refB ˆrefC
$ git log refA refB --not refC
```

### Triple Dot

Specifies all the commits that are reachable by either of two references but not by both of them:

```
$ git log master...experiment
F
E
D
C
```

A common switch to use with the log command in this case is --left-right, which shows you which side of the range each commit is in. This helps make the data more useful:

```
$ git log --left-right master...experiment
<F
<E
>D
>C
```
## 6.3 Stashing

> Stashing takes the dirty state of your working directory — that is, your modified tracked files and staged changes — and saves it on a stack of unfinished changes that you can reapply at any time.

When the working directory is dirty, you can't switch branches.

### Commands

`git stash`: stash the current working dir.

`git stash list`: list all the stashes done previously.

`git stash apply [STASH-ID]`: reapply the stash to the current working dir, if `[STASH-ID]` is ommited, the latest stash will be applied.

**NOTE**: Having a clean working directory and applying it on the same branch **aren’t** necessary to successfully apply a stash.

`git stash apply --index`: For `git satsh apply`, the changes to your files were reapplied, but the file you staged before wasn’t restaged. To do that, you must run the git stash apply command with a --index option to tell the command to try to reapply the staged changes.

`git stash drop`: `git stash apply` only apply stash, but don't remove it. This command will do it.

`git stash pop`: apply and drop.

## 6.3.2 Creating a Branch from a Stash

It's helpful when conflicts happen to `git stash apply`.

`git stash branch BRANCH-NAME [STASH-ID]`: Creates a new branch for you, checks out the commit you were on when you stashed your work, reapplies your work there, and then drops the stash if it applies successfully

## 6.4.2 Changing Multiple Commit Messages

`git rebase -i HEAD ̃3`: **Note** that these commits are listed in the **opposite** order than you normally see them using the log command.(When rebasing, apply the commits top to bottom)

## 6.4.5 Splitting a Commit

In `git rebase -i HEAD~3`, change the instruction on the commit to **edit**.

```
pick f7f3f6d changed my name a bit
edit 310154e updated README formatting and added blame
pick a5f4a0d added cat-file
```

When you save and exit the editor, Git rewinds to the parent of the first commit in your list, applies the first commit (f7f3f6d), applies the second (310154e), and drops you to the console. 

```
$ git reset HEADˆ
$ git add README
$ git commit -m ’updated README formatting’
$ git add lib/simplegit.rb
$ git commit -m ’added blame’
$ git rebase --continue
```

## 6.4.6 The Nuclear Option: filter-branch

Rewrite a larger number of commits in some scriptable way — for instance, changing your e-mail ad- dress globally or removing a file from every commit. 

**NOTE**: you probably shouldn’t use it unless your project isn’t yet public and other people haven’t based work off the commits you’re about to rewrite.

### Removing a File from Every Commit

`git filter-branch --tree-filter ’rm -f passwords.txt’ HEAD`: The `--tree-filter` option runs the specified command after each checkout of the project and then recommits the results.

It’s generally a good idea to do this in a testing branch and then hard-reset your master branch after you’ve determined the outcome is what you really want.

To run `filter-branch` on all your branches, you can pass `--all` to the command.

### Making a Subdirectory the New Root

If you want to make the trunk subdirectory be the new project root for every commit: `git filter-branch --subdirectory-filter trunk HEAD`.

### Changing E-Mail Addresses Globally

Use `--commit-filter` to go through and rewrite every commit.

```
$ git filter-branch --commit-filter ’
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
fi’ HEAD
```

## 6.5 Debugging with Git

## 6.5.1 File Annotation

If you track down a bug in your code and want to know when it was introduced and why, file annotation is often your best tool. It shows you what commit was the last to modify each line of any file.

you can annotate the file with `git blame` to see when each line of the method was last edited and by whom.

`git blame -L 12,22 simplegit.rb`: `-L` option to limit the output to lines 12 through 22

For `git blame` output, the commit SHA-1 starting with **^** means this line is in this file's original commit. 

If you pass `-C` to git blame, Git analyzes the file you’re annotating and tries to figure out where **snippets of code within it originally came from** if they were copied from elsewhere.

## 6.5.2 Binary Search

The bisect command does a binary search through your commit history to help you identify as quickly as possible which commit introduced an issue.

start:

```
$ git bisect start
$ git bisect bad
$ git bisect good v1.0
```

repeat:

```
$ git bisect good
or $ git bisect bad
```
until finding the first bad commit.

**NOTE**: When you’re finished, you should run `git bisect reset` to reset your HEAD to where you were before you started.

Automate this process:

```
$ git bisect start HEAD v1.0
$ git bisect run test-error.sh
```

`test-error.sh` is a script that test the project and exit 0 if the project is good or non–0 if the project is bad.

## 6.6 Submodules

Scenarios: you want to be able to treat the two projects as separate yet still be able to use one from within the other.

## 6.6.1 Create Submodules

`git submodule add git://github.com/chneukirchen/rack.git rack`: add external project **rack** as submodules.

Git doesn’t track submodule's contents when you’re not in that directory. Instead, Git records it as a particular commit from that repository. 

When commit the project, you'll see 160000 mode for the submodule entry.

## 6.6.2 Cloning a Project with Submodules

`git clone git://github.com/schacon/myproject.git`: but the submodule directory will be empty.

To get the submodule:

1. `git submodule init` to initialize;
2. `git submodule update` to fetch all the data from the project and **check out appropriate commit listed in your superproject**.

Now the submodule subdirectory is at the exact state it was in when you committed earlier. 

### Merge with other's work

Two steps:

1. Update the point the submodule commit in superproject;
2. update the content of the submodule to the version the superproject points to;

For 1, by `git merge origin/master`, it only merges a change to the pointer for your submodule; but it doesn’t update the code in the submodule directory.

For 2, by `git submodule update`, it'll update the content of the submodule to the correct version.

**One common problem** happens when a developer makes a change locally in a sub- module but doesn’t push it to a public server. Then, they commit a pointer to that non-public state and push up the superproject. When other developers try to run git submodule update, the submodule system can’t find the commit that is referenced, be- cause it exists only on the first developer’s system. If that happens, you see an error like this:

## 6.6.4 Issues with Submodules

* The submodule is detached. The second `git submodule update` will overwrite the previous changes, which has no branch pointing to and won't be found.
	* Create a branch when you work in a submodule directory.
* Switching branches with submodules in them can also be tricky. If you create a new branch, add a submodule there, and then switch back to a branch without that submodule, you still have the submodule directory as an untracked directory.
	* You have to either move it out of the way or remove it, in which case you have to clone it again when you switch back—and you may lose local changes or branches that you didn’t push up.
* switching from subdirectories to submodules.
	* first, unstage the submodule directory
	* then, add the submodule by `git submodule add …`
	* If you want to switch to another branch where those files are still in the actual tree rather than a submodule, you need to move the rack submodule directory out of the way by `mv SUBMODULE-DIR /tmp/`. When you switch back, you can either run `git submodule update` to reclone, or you can move your /tmp/SUBMODULE-DIR  directory back into the empty directory.
	
## 6.7 Subtree Merging

Merging strategy:

1. recursive strategy
2. octopus strategy
3. subtree merge

The idea of the subtree merge is that you have two projects, and one of the projects maps to a subdirectory of the other one and vice versa.

Suppose **Rack** is the submodule project.

You want to pull the Rack project into your master project as a subdirectory: `git read-tree --prefix=rack/ -u rack_branch`

### Merge changes from Rack

```
$ git checkout rack_branch
$ git pull
$ git checkout master
$ git merge --squash -s subtree --no-commit rack_branch
```


### diff between current **rack** subdirectory and the `rack_branch` branch

```
$ git diff-tree -p rack_branch
```

### diff between current **rack** subdirectory and the master branch on the server 

```
$ git diff-tree -p rack_remote/master
```







