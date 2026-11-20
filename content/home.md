+++
title = "§2. Home"
weight = 20
+++

## No place like ~

You should start by making sure you have a terminal working and can install packages!
This tutorial will be written assuming you are either using

- [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/setup/environment)
  (abbr. WSL; if you are a Windows user, set that up now);
- the terminal in macOS; or
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
- `ls` prints the contents of the current directory.
  - `ls -l` prints one entry per line
  - `ls -la` prints hidden files and folders as well
    (those whose name starts with a `.`)
- `mkdir` creates an empty directory.
- Remember the tilde `~` is an abbreviation for `/home/yourusername/`,
  your home folder. (Might be `/Users/yourusername` on macOS.)

### System packages, and checking with `which`

You'll want to install `vim`, `git`, `prek`, and `zola`.
Vim and Git are likely to be installed already; if not, search the web.
Here are [prek installation instructions](https://prek.j178.dev/installation/)
and [Zola installation instructions](https://www.getzola.org/documentation/getting-started/installation/).
(For Mac users, the easiest approach is to install
the [Homebrew package manager](https://brew.sh/);
then you just type `brew install zola` and `brew install prek`.
I think these days Homebrew works on WSL too.)

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
may just print empty output rather than an error message.)

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
such as syntax highlighting (on Mac) or line numbers.
You probably want to get those for your sanity.

Let's see if you remember what you learned from `vimtutor`.
We're going to open the file `~/.vimrc` in Vim (which may not exist yet):

```console
$ vim ~/.vimrc
```

Then, you should type the following contents into this file:

```viml,linenos,name=~/.vimrc
unlet! skip_default_vim
source $VIMRUNTIME/defaults.vim
set number
set cursorline
set cursorlineopt=number
set laststatus=2
set linebreak breakindent
set ignorecase smartcase
set signcolumn=yes
set updatetime=100
colorscheme retrobox
```

The first two lines turn on the default settings;
the `set …` lines change some basic settings I think should be on by default.
(If you're curious about any of them, you can type, say, `:help cursorline`
to learn what `cursorline` option works.)
Also, I picked a builtin color scheme (`retrobox`)
for you since the default scheme is hideous;
you can change to a [different builtin color scheme](https://justinjoyce.dev/every-built-in-vim-color-scheme-with-screenshots/)
if you don't like my taste in colors.
Save and quit the file.

If you open `~/.vimrc` again, you should now have line numbers and colors.

### Installing Vim plugins

Vim has a builtin plugin system:
if you put plugins in `~/.vim/pack/git-plugins/start/`
(or any folder name in place of `git-plugins`),
and Vim will automatically load them on startup.

To give an example, we'll install the `lightline.vim` package,
available at <https://github.com/itchyny/lightline.vim>,
which adds a pretty status bar to the bottom of Vim.
(The main advantage is that it will make it more obvious what mode you're in,
and what file you're editing. Also I think it looks nice.)

Vim packages these days are all usually on GitHub,
and `git clone` will allow you to make a copy.
So installing `lightline` means making
the empty folder `~/.vim/pack/git-plugins/start` and cloning:

```console
$ mkdir -p ~/.vim/pack/git-plugins/start

$ cd ~/.vim/pack/git-plugins/start

$ ls

$ git clone https://github.com/itchyny/lightline.vim
Cloning into 'lightline.vim'...
remote: Enumerating objects: 3031, done.
remote: Counting objects: 100% (97/97), done.
remote: Compressing objects: 100% (67/67), done.
remote: Total 3031 (delta 44), reused 43 (delta 28), pack-reused 2934 (from 3)
Receiving objects: 100% (3031/3031), 799.04 KiB | 7.54 MiB/s, done.
Resolving deltas: 100% (1446/1446), done.

$ ls
lightline.vim

$ ls lightline.vim/
autoload  colorscheme.md  doc  LICENSE  plugin  README.md  test
```

If you start Vim back up, you should see a nice looking status bar.
(Note this requires you to have `set laststatus=2` to work.)
Here's a picture on my computer:

![Vim, with the `retrobox` theme and `lightline` installed](/screenshots/vimrc.webp)

Throughout this tutorial we will suggest a couple more plugins
you could install:
[vim-gitgutter](@/git.md) in §6 and [ale](@/linters.md) in the Linters section.
Since you're already in this directory,
if you want you could just install them now too;
but until we get to the relevant sections these plugins won't do anything.

```console
$ pwd
/home/evan/.vim/pack/git-plugins/start

$ git clone https://github.com/airblade/vim-gitgutter
Cloning into 'vim-gitgutter'...
remote: Enumerating objects: 4373, done.
remote: Counting objects: 100% (199/199), done.
remote: Compressing objects: 100% (64/64), done.
remote: Total 4373 (delta 150), reused 156 (delta 130), pack-reused 4174 (from 3)
Receiving objects: 100% (4373/4373), 1.41 MiB | 11.44 MiB/s, done.
Resolving deltas: 100% (2363/2363), done.

$ git clone https://github.com/dense-analysis/ale
Cloning into 'ale'...
remote: Enumerating objects: 39838, done.
remote: Counting objects: 100% (1325/1325), done.
remote: Compressing objects: 100% (551/551), done.
remote: Total 39838 (delta 1163), reused 777 (delta 774), pack-reused 38513 (from 3)
Receiving objects: 100% (39838/39838), 19.93 MiB | 53.29 MiB/s, done.
Resolving deltas: 100% (27001/27001), done.

$ ls
ale  lightline.vim  vim-gitgutter
```
