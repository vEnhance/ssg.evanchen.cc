+++
title = "§2½. Vim"
weight = 25
+++

## Vim

Before going on to start the project, let's make sure you can edit files
in the world's best text editor.

### Learning Vim

You can launch a Vim instance, together with a builtin tutor, by using

```console
$ vimtutor
```

Vim is a **modal editor**: at any point, it's in a certain mode
which controls what keys do.
The three modes we'll focus on are:

- **Normal mode**: Keys perform movement or direct operations.
  (You begin in this mode when Vim starts up.)
  The most basic ones are:

  - The `hjkl` work like arrow keys ←↓↑→ for moving the cursor.
  - Typing `i` switches to insert mode.
  - Typing `:` switches to command mode.

  But in fact, pretty much _every_ key on your keyboard does something different,
  case-sensitively.
  (Random examples: `x` deletes a character, `u` is undo,
  `$` jumps to the end of line, `q` records a macro, …)
  The way you become really fast with Vim is internalizing
  which keys let you do which operations.
  In principle, you rarely if ever need a mouse.
- **Insert mode**: Keys type text into the document directly.
  This is what you're used to.
  Typing Esc returns to normal mode.
  (Arrow keys work in here, but their usage is discouraged;
  most users switch back to normal mode.)
- **Command mode**: Vim has its own internal command line.
  You press Enter after typing a command, or Esc to abort.
  The most important ones for us are `:q` to quit
  (`:q!` without saving), `:w` to save, `:wq` for both.

Here's a survival chart showing the information above.
This is sort of the _absolute minimum_ that you could use to type in Vim.
([Download full-size diagram](/d2/vimmodes.svg).)

[![Survival chart for Vim](/d2/vimmodes.svg)](/d2/vimmodes.svg)

However, **I don't recommend using only the survival chart**.
Rather, I think it's worth learning at least a few more normal mode keys.
It's a bit like riding a bicycle: it takes a while to get used to,
but as you add get used to more and more commands in normal mode,
you'll be able to do editing faster and faster.

Rather than redo a manual of common normal mode keys, you have a couple options.
One is to look through the `vimtutor` I told you to open, which starts with:

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
you should at least finish lesson 1.1,
then see how much appetite you have to learn more.
After completing 1.1 of `vimtutor`, you should be able to open a file,
edit it, save it, and quit Vim.

Alternatively, if you prefer video games to `vimtutor`,
you can use <https://vimventure.dev/learn> (free browser game) instead.
That will also teach you a lot of normal mode commands.
(There are some other similar games out there.)

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
unlet! skip_defaults_vim
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

(If you're trying to use copy/paste in Vim rather than typing these 11 lines by
hand, first enter _Insert mode_ with `i`, then use whatever paste sequence
you would use if you were trying to copy paste a command into the shell.)

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

Vim has a builtin plugin system.
If you put plugins in `~/.vim/pack/git-plugins/start/`
(or any folder name in place of `git-plugins`),
Vim will automatically load them on startup.

To give an example, we'll install the `lightline.vim` package,
available at <https://github.com/itchyny/lightline.vim>.
This adds a pretty status bar to the bottom of Vim
which I think helps you keep track of what file you're editing
and which Vim mode you're in.

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
[vim-gitgutter](@/styling.md) in §6 and [ale](@/linters.md) in the Linters section.
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
