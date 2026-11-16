+++
title = "§2. Home"
weight = 20
+++

## No place like ~

You should start by making sure you have a terminal working and can install packages!
This tutorial will be written assuming you are either using

- [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/setup/environment),
- the terminal in Mac OSX; or
- the terminal in any standard Linux distribution.

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
- `ls -la` prints the contents of the current directory.
- Remember the tilde `~` is an abbreviation for `/home/yourusername/`,
  your home folder.

### Packages, and checking your computer can find them

At minimum, you'll want to install `git`, `vim`, and `zola`.
For Zola, follow the
[Zola installation instructions](https://www.getzola.org/documentation/getting-started/installation/).
Git and Vim are likely to be installed already.

To see what's working, the `which` command lets you see, for a given command,
where the system can find it, or if the command doesn't exist at all.
For example on my computer:

```console
$ which git
/usr/bin/git

$ which vim
/usr/bin/vim

$ which zola
/usr/bin/zola

$ which nonexistent_command
which: no nonexistent_command in (/home/evan/.local/bin:…)
```

### Learning Vim

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

There are also interactive games that can work instead of `vimtutor`.
See, e.g., <https://vimventure.dev/learn> (free browser game).

### Writing a `.vimrc`

By default, Vim doesn't enable some very basic QoL features
such as syntax highlighting or line numbers.
You probably want to get those for your sanity.

Let's see if you remember what you learned from `vimtutor`.
We're going to open the file `~/.vimrc` in Vim (which may not exist yet):

```console
$ vim ~/.vimrc
```

Then, you should type the following contents into this file:

```viml,linenos,name=~/.vimrc
syntax on
set number
set cursorline
set cursorlineopt=number
set mouse=a
```

This turns on the most basic features: syntax coloring,
a line number, and mouse support.
Save and quit the file.

If you open `~/.vimrc` again, you should now have line numbers,
and see some colors on the keywords.

### Making a new folder for your website

For this demo I'll put the website in `~/mysite`,
though you're obviously free to pick any other path.

```console
$ mkdir mysite

$ cd ~/mysite

$ pwd
/home/evan/mysite
```

**For the rest of the tutorial,
please ensure you're always working in `~/mysite`**.
If you open a new terminal window, it might spawn in the home folder `~` instead.
You can always use `pwd` to show where you are.
