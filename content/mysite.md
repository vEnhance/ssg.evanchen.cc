+++
title = "§2. ~/mysite"
weight = 20
+++

## ~/mysite

We'll now create a new folder and initialize an empty Git repository in it.

### §2.A. Making a new folder for your website

For this demo I'll put the website in `~/mysite`,
though you're obviously free to pick any other path.

```console
$ mkdir ~/mysite

$ cd ~/mysite

$ pwd
/home/evan/mysite
```

If you've never heard of tab-completion before, now's a good place to learn that.
You don't have to actually keep typing things like `mysite` over and over.
If you type just `cd ~/m` and then hit Tab, the shell will give you a list of
completions (i.e. folders that start with m) to finish the command.
This will generally save you a lot of typing,
where you only type the first few letters of a file or folder.
(If you're using a more sophisticated shell like `zsh` or `fish`,
the autocompletion can do even more than this.)

**For the rest of the tutorial,
please ensure you're always working in `~/mysite`**.
If you open a new terminal window, it might spawn in the home folder `~` instead.
You can always use `pwd` to show where you are.

### §2.B. Setting up Git for first use

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

If you trust me, here are a few settings that I think you should just turn on.

```console
$ git config --global init.defaultBranch main

$ git config --global pull.rebase true

$ git config --global push.default current

$ git config --global push.autoSetupRemote true
```

### §2.C. Start a new repository

Make sure that you're in `~/mysite`, then type `git init`:

```console
$ pwd
/home/evan/mysite

$ git init
Initialized empty Git repository in /home/evan/mysite/.git/
```

You can now see the hidden folder `.git` when you run `ls -la`.
That's where Git stores its work; you don't need to look inside yourself.

```console
$ ls -la
total 16
drwxrwx---  3 evan evan 4096 Nov 15 20:02 .
drwx------ 62 evan evan 4096 Nov 15 20:47 ..
drwxrwx---  8 evan evan 4096 Nov 15 20:47 .git
```

Personally, I like to start with an initial totally empty commit,
which you can do with:

```console
$ git commit -m "root: Initial commit" --allow-empty
[main (root-commit) 054b8b5] root: Initial commit
```

You can see the history of your commits with the following command
(right now, just that one first commit):

```console
$ git log --oneline
054b8b5 (HEAD -> main) root: Initial commit
```
