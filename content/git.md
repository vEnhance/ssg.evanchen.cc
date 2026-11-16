+++
title = "§3. Git"
weight = 30
+++

## Git

We'll start by putting together the pieces of our repository,
and then talk about how to continue using Git afterwards.

### Setting up Git for first use

If you've never used Git before, you need to tell it who you are:

```console
$ git config --global user.name "Your Name"

$ git config --global user.email "public.email@example.org"
```

Treat the email address as public, as if anyone can find it.
(I think the email address isn't used for anything other than display,
so it doesn't actually have to work.
If you don't want people to contact you,
I think [GitHub provides noreply email addresses you can use][gh-noreply]
although I've never tried doing this before.)

[gh-noreply]: https://docs.github.com/en/account-and-profile/reference/email-addresses-reference#your-noreply-email-address

While you're here, you should make the following default settings:

If you trust me, here are a few settings that I think you should just turn on.

```console
$ git config --global init.defaultBranch main

$ git config --global pull.rebase true

$ git config --global push.default current

$ git config --global push.autoSetupRemote true
```

### Start a new repository

We can now go to the directory and make our website.
For this demo I'll put the website in `~/mysite`,
though you're obviously free to pick any other path.

```console
$ mkdir mysite

$ cd ~/mysite

$ git init
Initialized empty Git repository in /home/evan/mysite/.git/
```

This creates an empty repository, which has no commits.
The first thing we should do is create a new commit.
Personally, I like to make the very first commit totally empty,
because the root commit is one that you can't change later
if you change your mind about something.

```console
$ git commit -m "root: Initial commit" --allow-empty
[main (root-commit) c333c0b] root: Initial commit
```

You can see the history of your commits with the following command
(right now, we have just one commit):

```console
$ git log --oneline
054b8b5 (HEAD -> main) root: Initial commit
```

### Using Git

Okay, so Git has a bit of a reputation,
[as you can see from xkcd](https://xkcd.com/1597/).
Here's my attempt to summarize the
[official tutorial](https://git-scm.com/docs/gittutorial),
into 100 words:

- Git tracks history in _commits_ and only takes responsibility for committed code.
  Every commit gets a user-specified message and assigned hash.
  **Best practice is making a commit for every _logical_ change**.
  A good litmus test is whether it's easy to write the commit message.
  ([Here's xkcd again](https://xkcd.com/1296/).)
- To start a commit, use `git add` or `git add -p` to _stage_ desired changes.
- After staging, type `git commit` to _commit_ the staged into the repository.
- At any time, to get your bearings:
  - `git log` or `git log --oneline` shows past commits;
  - `git status` shows a summary of the current directory;
  - `git diff` shows changes not yet staged;
  - `git diff --staged` to see changes staged but not yet committed.

For commit messages, a lot of people (including me) use
[conventional commits](https://www.conventionalcommits.org/en/v1.0.0/),
which prefaces each commit message with `fix:` (bugfix) or `feat:` (new function)
or other word (like `root`, `refactor`, …)
to indicate what "kind" of commit it is.
We'll follow that for this tutorial.

For the rest of the tutorial,
**I'll actually include the Git commands too,
so you can see this happening in practice**.

There are two ways to specify commit messages:

- If you run `git commit` normally, it'll open up a Vim editor
  where you can type the commit message and write it out.
- If you prefer not to open Vim for this,
  you can also specify the message on the command line like this,
  to specify the message on the command line.

```console
git commit -m "feat: enable sharks with lasers on their heads"
```

For the tutorial, I'll use the second form to make the steps easier to follow,
but in real life (and on video) I usually use Vim to write the commit message.

### Writing .gitignore

Our first commit will be to make a file called `.gitignore`.
The `.gitignore` is a special file meant to prevent unwanted files
from accidentally getting committed.

An example of a file you'd want to include in this if you're a Mac user is `.DS_Store`.
This is a file that Mac's automatically generate in every folder
for [reasons unknown to humankind](https://en.wikipedia.org/wiki/.DS_Store).
There is no reason you want this file in your Git history,
but it could get added accidentally if you're not paying attention
(and I have [made fun of people on Instagram before for this](https://www.instagram.com/p/DIcujMcO80Q/)).

A second thing we'll want to exclude is the `public/` folder.
It hasn't happened yet, but later Zola will generate this folder with output files.
We only want Git to track the source code files, not folders like `public/`.

So, let's create that file in Vim:

```console
$ vim .gitignore
```

Put the following two lines in it:

```gitignore,linenos,name=.gitignore
**/.DS_Store
/public/
```

That's a single logical change. Let's commit it.
I've inserted some `git status` so you can see how Git is keeping track of things,
but really the only two things you need to do are `git add .gitignore`
and `git commit`.

```console
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

### (Optional) Pre-commit via prek
