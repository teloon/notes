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

[1]: http://git-scm.com/book