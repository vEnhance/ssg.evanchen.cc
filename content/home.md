+++
title = "No place like ~"
weight = 20
+++

## No place like ~

You should start by making sure you have a terminal working and can install packages!
If you're on Linux, use whatever you're used to.
Mac users can use the built-in one too.
For Windows, the correct long-term approach is
[Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/setup/environment),
but you might be able to get away with Windows Powershell
(which should be built-in).

This tutorial will be written assuming you are either using

- Windows Subsystem for Linux;
- Mac OSX; or
- any standard Linux distribution.

We also assume you have figured out how to use a shell, so you can do things like

```text
$ cd ~

$ pwd
/home/evan
```

(where your username should show instead of `evan`).
If you've never seen a shell before, you might check the
["Filesys concepts" page on my website first](https://web.evanchen.cc/filesys.html).

### Packages, and checking your computer can find them

At minimum, you'll want to install `git`, `vim`, and `zola`.
For Zola, follow the
[Zola installation instructions](https://www.getzola.org/documentation/getting-started/installation/).
Git and Vim are likely to be installed already.

To see what's working, the `which` command lets you see, for a given command,
where the system can find it, or if the command doesn't exist at all.
For example on my computer:

```text
$ which git
/usr/bin/git

$ which vim
/usr/bin/vim

$ which zola
/usr/bin/zola

$ which nonexistent_command
which: no nonexistent_command in (/home/evan/.local/bin:…)
```

### Launching vimtutor

Type `vimtutor` into your terminal
to get a briefing on how to use the Vim text editor.
It looks like this at the beginning:

```text
===============================================================================
=    W e l c o m e   t o   t h e   V I M   T u t o r    -    Version 1.7      =
===============================================================================
=                              C H A P T E R   ONE                            =
===============================================================================

     Vim is a very powerful editor that has many commands, too many to
     explain in a tutor such as this.  This tutor is designed to describe
     enough of the commands that you will be able to easily use Vim as
     an all-purpose editor.
     The approximate time required to complete the tutor is 30 minutes,
     depending upon how much time is spent with experimentation.
```

The lessons go from 1.1 to 1.7 in here;
but you could technically survive off just Lesson 1.1…
not that I recommend this…

After completing `vimtutor`, you should be able to open a file,
edit it, save it, and quit Vim.

### Writing a `.vimrc`

By default, Vim doesn't enable some very basic QoL features
such as syntax highlighting or line numbers.
You probably want to get those for your sanity.

Let's see if you remember what you learned from `vimtutor`.
We're going to open the file `~/.vimrc` in Vim (which may not exist yet):

```text
$ vim ~/.vimrc
```

Then, you should type the following contents into this file:

```vimrc
syntax on
set number
set cursorline
set cursorlineopt=number
set mouse=a
```

This turns on the most basic features: syntax coloring,
a line number, and mouse support.
Save and quit the file.

If you open Vim again, you should now have line numbers.
