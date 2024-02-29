---
theme: league
---
# Git Deep Dive

---

## About Me

![website|150x150](data:image/png;base64,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)

Jordan Duabe -- `@j4ckofalltrades`

- Software Engineer @ WiseTime
- Polyglot dev & open-source enthusiast
- Command-line ninja :ninja:
- Has one too many MKBs :keyboard:

---

## Agenda

- What is git (and some basic concepts)
- Diving into `.git`
- Configuring `git`
- The feature branch workflow
- `git` commands to add to your toolkit

note:

1. A short history of git
2. Exploring git internals
3. Some git options and external tools that might be useful
4. Branching, dealing with conflicts, merge vs rebase, and some info on the usual "day-to-day" commands (add, commit, push, etc).
5. Useful git commands (plus some useful command options)

---

## What is git?

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. 

---

![git|420](https://imgs.xkcd.com/comics/git.png)

xkcd: [xkcd.com/1597/](https://xkcd.com/1597/)
 
---

### A Short History of git

- Written in April 2005 by Linus Torvalds (creator of Linux)
- Maintained (since July 2005) by Junio Hamano
- v1.0 release was on December 2005
- v2.44 (latest release) at time of writing

---

### Get git with git

```sh
git clone https://github.com/git/git
```

note:

Total commits: `git rev-list --count --all`

Total files: `git ls-files | wc -l`

Total lines of code: `git ls-files | xargs wc -l`

Total committers: `git shortlog --summary --numbered | wc -l`

---

### Three Main Sections

![](https://git-scm.com/book/en/v2/images/areas.png)

note:

Diagram from the `Pro Git` book.

The basic Git workflow goes something like this:

1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds _only_ those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

---

### Status lifecycle of your files

![](https://git-scm.com/book/en/v2/images/lifecycle.png)

note:

Diagram from the `Pro Git` book

Remember that each file in your working directory can be in one of two states: _tracked_
or _untracked_.

Tracked files are files that were in the last snapshot, as well as any newly staged files;
they can be unmodified, modified, or staged. In short, tracked files are files that Git
knows about.

Untracked files are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area.

---

## Diving into `.git`

---

### `git` internals

> [!error] Here be dragons

```sh
myrepo/.git/
├── HEAD
├── config
├── hooks
│   ├── pre-commit
├── index
├── objects
├── refs
│   ├── heads
└── └──  └── main
```

---

> [!info] Demo: Diving into `.git`

note:

- `HEAD`  and the refs directory
- `config`
- `index`
- the objects directory

---

## Configuring git

---

### Initial Setup

The `git config` subcommand lets you get and set configuration variables that control all aspects of how Git looks and operates.

```sh
# system wide configuration
git config --system

# user specific configuration
git config --global

# repo specific configuration
git config [--local]
```

Each level overrides values in the previous level.

---

### Basic configuration items

```sh
# identity
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# preferred editor
# defaults to "vi" on most systems
git config --global core.editor "code --wait"
```

---

### Conditional config

```sh
[includeIf "gitdir:~/Developer/work/"]
  path = ~/.gitconfig.work

[includeIf "gitdir:~/Developer/oss/"]
  path = ~/.gitconfig.oss
```

note:

Using the `includeIf` directive, it is possible to setup custom configuration items.

---

> [!tip] Alias your git commands

```sh
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.di diff
git config --global alias.lg 'log --oneline --graph'
git config --global alias.rs restore
git config --global alias.sh stash
git config --global alias.st 'status -sb'
git config --global alias.sw switch
```

---

> [!tip] Configure a global gitignore

```sh
git config --global core.excludesFile gitignore.global
```

---

> [!tip] Enable autocorrect

```sh
git config --global help.autocorrect <option>

# where option is one of
# 1. immediate (run closest command immediately)
# 2. prompt (ask for confirmation)
# 3. <timeout> (in deciseconds before running the command)
```

---

> [!tip] Configure auto push upstream for new branches

```sh
git config --global push.autoSetupRemote true
```

This removes the need for `git push --set-upstream <remote> <branch>`

note:

This will automatically create new branches (on the remote).

---

> [!tip] Configure `delta` as the pager tool

[git-delta](https://dandavison.github.io/delta/)

```sh
[pager]
  diff = delta
  log = delta
  reflog = delta
  show = delta
  blame = delta

[delta]
  features = side-by-side line-numbers decorations
  syntax-theme = Solarized (dark)
  plus-style = "syntax #003800"
  minus-style = "syntax #3f0001"
```

---

> [!tip] Configure commit signing

[About commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification)
```sh
git config --global commit.gpgSign true
```


![](https://docs.github.com/assets/cb-17614/mw-1440/images/help/commits/verified-commit.webp)

---

### Getting Help

The following are equivalent ways of getting manual page (manpage) help for any git command.

```sh
# detailed help
git add --help

# concise tl;dr version
git add -h
```

note:

Generally speaking when reading manpages for command-line applications, angle
brackets denote _required_ arguments while square brackets denote _optional_ flags.

---
## The feature branch workflow

---

### Getting a git repository

```sh
# Option 1: Clone an existing repository
git clone <repo>

# Option 2: Initializing a repo in an existing directory
git init
git remote add <remote> <remote-url>

# to check if the remote was properly configured
git remote --verbose

# the remote URLs can also be updated
# generally used when the remote repository is renamed
# or when switching hosting providers e.g. GitHub
git remote set-url <remote> <new-remote-url>
```

---

### Creating a branch

```sh
# create a new branch
git branch <branch-name>

# create a branch and automatically 'switch' to it
git switch --create <branch-name>
```

---

### Moving between branches

```sh
git switch <branch-name>

git switch - # switch back to previous branch
```

---

### What about `git checkout`?

As of v2.23 (Q3 2019) `git checkout` was split out into two new commands, `switch` and `restore.`

| Old command                     | New command                         |
| ------------------------------- | ----------------------------------- |
| git checkout -- \<pathspec\>    | git restore \<pathspec\>            |
| git checkout \<branch-name\>    | git switch \<branch-name\>          |
| git checkout -b \<branch-name\> | git switch --create \<branch-name\> |

---

### `git checkout`

```sh
checkout: split part of it to new command 'switch'

"git checkout" doing too many things is a source of confusion 
for many users (and it even bites old timers sometimes). To 
remedy that, the command will be split into two new ones: switch 
and restore. The good old "git checkout" command is still here 
and will be until all (or most of users) are sick of it.

See the new man page for the final design of switch. The actual
implementation though is still pretty much the same as "git 
checkout" and not completely aligned with the man page. 

Signed-off-by: Nguyễn Thái Ngọc Duy <pclouds@gmail.com>
Signed-off-by: Junio C Hamano <gitster@pobox.com>
```

The [commit](https://github.com/git/git/commit/d787d311dbd7a4104a9dde23b90ae24528a15cf9) that added the `switch` command to `git`.

---

### Ignoring files

Tell git to ignore certain files and directories by creating a `.gitignore` file in your
working directory.

> [!info] This is applied in addition to a global gitignore (if configured).

---

### Viewing the status of your files

`git status`

> [!tip] For a tl;dr status
> `git status --short`

---

### Removing untracked files

`git clean`

```sh
# list files to be removed
git clean --dry-run

# remove files recursively (also remove untracked directories)
git clean -df
```

---

### Tracking new files and/or Staging modified files

`git add`

```sh
git add --all # add everything
git add . # everything under the current directory
git add src # everything under the src directory
git add *.txt # all txt files under the current directory
git add test/*.js # all js files under the test directory
```

---

> [!tip] Use the `--dry-run` option to see what files will be staged

---

> [!info] `git stage` is an alias for `git add`

---

### Interactive add

`git add -p`

> [!info] Demo: Selectively staging changes from a file

---
### Viewing changes

`git diff`

```sh
git diff # show changes in the working tree
git diff --staged # show changes in the staging area
# show all changes (working tree and staging area)
git diff HEAD # or
git diff @ # @ is an alias for HEAD
```

---

### Committing changes

`git commit`

```sh
git commit # opens up the default editor
git commit --message "wrote some code" # skips the editor
# bypass the staging area and commit directly
git commit --only --message "wrote some code" <pathspec>
# bypass the staging area and commit *all files* directly
git commit --all --message "wrote some code" 
```

---

### Commit messages

![](https://imgs.xkcd.com/comics/git_commit_2x.png)

xkcd: [xkcd.com/1296/](https://xkcd.com/1296/)

---

### Viewing commit logs / history

`git log`

```sh
git log # show full history (paged view)
git log -<number-of-commits> # limit output to only n commits
git log --graph # draw a graphical representation
git log --patch # show changes made for the commit
git log --patch -1 # limit output to only the latest commit
```

---

### View a specific commit

```sh
git show <commit-hash>
```

---

### Undoing changes

> [!warning] Discard changes from the working directory
> `git restore <pathspec>`

> [!info] Discard changes from the staging area
> `git restore --staged <pathspec>`

---

### Undoing commits

> [!info] Soft reset (undo commit but keep changes)
> `git reset <commit-hash>`

> [!warning] Hard reset (undo commit and discard changes)
> `git reset --hard <commit-hash>`

---

### Amending commits

```sh
git commit --amend
```

Use the `--amend` flag to add files to an existing commit and/or update the commit message.

> [!warning] This creates a new commit!
> To avoid possible conflicts and diverged branches, only run this command on unpushed commits.

---

### `reflog` to the rescue

`git reflog`

Records when the tips of branches and other references were updated in the local repository.

> [!tip] Use this command to go back to a known "working" version of your code.

---

### Pushing changes

```sh
git push <remote> <branch>
```

The `git push` command is used to upload local repository content to a remote repository.

---

### Fetching contents from the remote

```sh
# fetch all branches and changes from remote
git fetch <remote>

# fetch a specific branch from remote
git fetch <origin> <branch>
```

To list all remote branches, run `git branch --remotes` .

note:

The `git fetch` command downloads commits, files, and refs from a remote repository into your local repo.

Remote branches are stored in `.git/refs/remotes`.

---

### Pull

```sh
git pull <remote> <branch>
```

Basically an alias for:

```sh
# assuming the current branch is main
git fetch origin main
git merge origin/main
```

note:

The git pull command is used to fetch and download content from a remote repository and immediately update the local repository to match that content. 

---

### Diverged history

![](https://git-scm.com/book/en/v2/images/basic-rebase-1.png)

---

### Reconcile diverged branches

```sh
hint: You have divergent branches and need to specify how to 
reconcile them. You can do so by running one of the following 
commands sometime before your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to 
set a default preference for all repositories. You can also pass 
--rebase, --no-rebase, or --ff-only on the command line to 
override the  configured default per invocation.
fatal: Need to specify how to reconcile divergent branches.
```

---

### Merge

```sh
# Merging main to your feature branch
git switch main
git pull origin main
git switch my-feature-branch
git merge main

# Alternatively pull directly from your feature branch
git pull origin main 
```

---

### Merge (visual)

![merge](https://git-scm.com/book/en/v2/images/basic-rebase-2.png)

---

### What really happens

![merge|620](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExMzB0ZHY3NmVvemM2eHdkeWlpbGxhdTBhcTNkczhhN3JzcG4wcjNmYSZlcD12MV9naWZzX3NlYXJjaCZjdD1n/cFkiFMDg3iFoI/giphy.gif)

---

### Rebase

```sh
# Rebasing main on to your feature branch
git switch main
git pull origin main
git switch my-feature-branch
git rebase main

# Alternatively pull directly from your feature branch
git pull origin main --rebase
```

> [!danger] This command rewrites the commit history
> Do not rebase commits that exist outside your repository and that people may have based work on.

---

### Rebase (visual)

![](https://git-scm.com/book/en/v2/images/basic-rebase-3.png)

---

> [!info] Demo: Creating a pull request on GitHub

---
## git commands to add to your toolkit

---

### `git blame`

Show what revision and author last modified each line of a file.

```sh
git blame <file>

# limit to a specific line range
git blame -L <start,end> <file>

# limit to a specific function
git blame -L <funcname> <file>
```

My favorite command <!-- element class="fragment" -->

... until I see the result. <!-- element class="fragment" -->

---

### `git grep`

Print lines matching a pattern.

```sh
git grep <search-term> [pathspec]
```

---

### `git revert`

Undo some existing commits.

```sh
git revert <commit>

# revert a specific commit
git revert <commit>

# revert the commit but doesn't commit the changes
git revert --no-commit <commit>
```

note:

Useful for undoing commits that have been already pushed.

---

### `git stash`

Stash the changes in a dirty working directory away.

```sh
git stash create <message>

# list all existing stashes
git stash list
# apply specific stash from list
git stash apply stash{0}
# apply stash at top of stack (LIFO) then delete it
git stash pop
```

---

### `git cherry-pick`

Apply the changes introduced by some existing commits (from some other branch).

```sh
git cherry-pick <commit>
```

---

### `git bisect`

Use binary search to find the commit that introduced a bug.

```sh
git bisect <subcommand> <options>

# Basic commands: start, bad, good
git bisect start
git bisect bad
git bisect good <commit> # can also be a tag

# Cleaning up
git bisect reset
```

---
### Attribution

- Images from [xkcd](https://xkcd.com) and [louim](https://twitter.com/louim)
- Diagrams from the [Pro Git](https://git-scm.com/images/about/index1@2x.png) book

---

### Recommended reading

- [Pro Git by Scott Chacon](https://git-scm.com/book)
- [Julia Evans' blog](https://jvns.ca/) 
- [git documentation](https://git-scm.com/docs)

---

### Slides

![slides|200x200](data:image/png;base64,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)

---

### Questions

> ???

---

![git-in-case-of-fire](https://raw.githubusercontent.com/hendrixroa/in-case-of-fire-1/master/in_case_of_fire.png)