+++
title = "§2. Home"
weight = 20
+++

## No place like ~

You should start by making sure you have a terminal working and can install packages!
This tutorial will be written assuming you are either using

- [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/setup/environment) (abbr. WSL).
  If you are a Windows user, you should set that up now: `wsl --install`.
- The terminal in macOS.
- The terminal in any standard Linux distribution.

So you should be able to do something like this:

```console
$ cd ~

$ pwd
/home/evan
```

If you don't know what `cd` and `pwd` are,
check the ["Filesys concepts" page on my website first](https://web.evanchen.cc/filesys.html).
If you want a quick refresher or cheat sheet:

- `cd` changes the current directory, and you can use tab-completion for it.
- `pwd` prints the current directory.
- `ls` prints the contents of the current directory.
  - `ls -l` prints one entry per line
  - `ls -la` prints hidden files and folders as well
    (those whose name starts with a `.`)
- `mkdir` creates an empty directory.
- Remember the tilde `~` is an abbreviation for `/home/yourusername/`,
  your home folder. (Might be `/Users/yourusername` on macOS.)

### §2.A. Package manager

To work through this tutorial, you'll need to be able to install things.
If you are using any of WSL, macOS, or Ubuntu,
the recommendation for this tutorial is [Homebrew](https://brew.sh/),
and we'll assume that going forward.
You can check the homepage for installation details,
but the following should work in many places:

```console
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

If you're using a Linux distribution that isn't Ubuntu…
you probably already have a package manager you're used to,
like Arch Linux's `pacman`.
Feel free to use that instead of `brew` in that case.

### §2.B. System packages, and checking with `which`

You'll want to install `vim`, `git`, `prek`, and `zola`.
Vim and Git are likely to be installed already.
Meanwhile, `prek` and `zola` are both on Homebrew, so you can get them with:

```console
$ brew install prek zola
```

To see what's working, the `which` command lets you see, for a given command,
where the system can find it, or if the command doesn't exist at all.
For example on my computer:

```console
$ which vim
/usr/bin/vim

$ which git
/usr/bin/git

$ which prek
/usr/bin/prek

$ which zola
/usr/bin/zola

$ which nonexistent_command
which: no nonexistent_command in (/home/evan/.local/bin:…)
```

(Depending on your distribution, `which nonexistent_command`
may just print empty output rather than an error message.
If you're installing with Homebrew, those paths will likely be different.)

All of these commands happen to support `--version`.
I think most of this tutorial will work for any non-ancient versions,
but just to have in writing the versions that I'm writing this with:

```console
$ vim --version
VIM - Vi IMproved 9.2 (2026 Feb 14, compiled Oct 12 2026 22:01:04)
Included patches: 1-1178
Compiled by Arch Linux
… [much more output truncated] …

$ git --version
git version 2.56.1

$ prek --version
prek 0.5.11 (ed919951c 2026-11-16)

$ zola --version
zola 0.23.6
```

We can now run commands!
The command we'll use most is Vim, so we'll go there next.
