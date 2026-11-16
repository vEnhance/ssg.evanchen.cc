+++
title = "§4. Git"
weight = 40
+++

## Using Git

Okay, so Git has a bit of a reputation,
[as you can see from xkcd](https://xkcd.com/1597/).
Here's my attempt to summarize the
[official tutorial](https://git-scm.com/docs/gittutorial) into 100 words:

- Git tracks history in _commits_ and only takes responsibility for committed code.
  Every commit gets a user-specified message and assigned hash.
- **Best practice is making a commit for every _logical_ change**.
  A good litmus test is whether it's easy to write the commit message.
  ([Here's xkcd again](https://xkcd.com/1296/).)
- To start a commit, use `git add` or `git add -p` to _stage_ desired changes.
- After staging, type `git commit` to _commit_ staged changes into the repository.
- At any time, to get your bearings:
  - `git log` or `git log --oneline` shows past commits;
  - `git status` shows a summary of the current directory;
  - `git diff` shows changes not yet staged;
  - `git diff --staged` to see changes staged but not yet committed.

For the rest of the tutorial,
**I'll actually include the Git commands too,
so you can see this happening in practice**.

### About commit messages

For commit messages, a lot of people (including me) use
[conventional commits](https://www.conventionalcommits.org/en/v1.0.0/),
which prefaces each commit message with `fix:` (bugfix) or `feat:` (new function)
or other word (like `root`, `refactor`, …)
to indicate what "kind" of commit it is.
We'll follow that for this tutorial.

There are two ways to specify commit messages:

- If you run `git commit` normally, it'll open up a Vim editor
  where you can type the commit message and write it out.
- If you prefer not to open Vim for this,
  you can also specify the message on the command line like this,
  to specify the message on the command line.

```console
$ git commit -m "feat: enable sharks with lasers on their heads"
```

For the tutorial, I'll use the second form to make the steps easier to follow,
but in real life (and on video) I usually use Vim to write the commit message.

### Writing .gitignore

Our first substantial commit will be to make a file called `.gitignore`.
The `.gitignore` is a special file meant to prevent unwanted files
from accidentally getting committed.

An example of a file you'd want to include in this if you're a Mac user is `.DS_Store`.
This is a file that Mac's automatically generate in every folder
for [reasons unknown to humankind](https://en.wikipedia.org/wiki/.DS_Store).
There is no reason you want this file in your Git history,
but it could get added accidentally if you're not paying attention
(and I have [made fun of people on Instagram for this](https://www.instagram.com/p/DIcujMcO80Q/)).

A second thing we'll want to exclude is the `public/` folder.
It hasn't happened yet, but later Zola will generate this folder with output files.
We only want Git to track the source code files, not folders like `public/`.

So, let's create that file in Vim:

```console
$ pwd
/home/evan/mysite

$ vim .gitignore
```

Put the following two lines in it:

```gitignore,linenos,name=.gitignore
**/.DS_Store
/public/
```

That's a single logical change. Let's commit it.
I've inserted some `ls` and `git status` to help you keep track,
but the two commands actually doing things are just
`git add .gitignore` and `git commit`.

```console
$ pwd
/home/evan/mysite

$ ls -la
total 16
drwxrwx---  3 evan evan 4096 Nov 15 20:51 .
drwx------ 62 evan evan 4096 Nov 15 20:47 ..
drwxrwx---  8 evan evan 4096 Nov 15 20:51 .git
-rw-rw----  1 evan evan   22 Nov 15 20:51 .gitignore

$ git status
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
  .gitignore

nothing added to commit but untracked files present (use "git add" to track)

$ git add .gitignore

$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  new file:   .gitignore

$ git commit -m "chore: add gitignore"
[main eb7aafd] chore: add gitignore
  1 file changed, 2 insertions(+)
  create mode 100644 .gitignore

$ git log --oneline
eb7aafd (HEAD -> main) chore: add gitignore
054b8b5 root: Initial commit
```

Alright! That's our first substantial commit.

### Undoing mistakes in git

Although I won't emphasize this in the tutorial,
if you make a mistake with a commit, you can often fix it.
Maybe you committed a file you didn't mean to,
or missed a file from the stage you meant to add, or just saw a typo.
If so, some options:

- If you do `git commit --amend` instead of `git commit`,
  instead of creating a new commit,
  it'll combine the staged changes into the most recent commit.
  So if you notice a typo right after committing something,
  you would fix it, `git add`, and then run `git commit --amend`.

- If you need to do something more complicated like amending an older commit
  or changing the order of commits,
  `git rebase --interactive [target]`
  will let interactively modify all the commits _target_ the hash.
  I often choose target as `HEAD~10`, say, to pull up the 10 most recent commits.

- `git reset` is a bit more complicated. Use with caution.

### Preventing mistakes in git

One thing you can do to minimize the
