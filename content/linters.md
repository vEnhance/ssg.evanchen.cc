+++
title = "Linters"
weight = 85
+++

## Linters and formatters

As mentioned in the [section on Prek](@/prek.md),
linters and formatters provide a way to automatically
check for errors and enforce a consistent code style.
For example, these days if I'm writing code,
I never bother thinking about indentation.
I just write whatever is technically correct (if ugly), press save,
and let the computer handle making the indentation look nice.

Here, I'll give four examples of tools that this project uses.
Then I'll describe the two places you can put these:

- How to integrate these tools into `prek`.
  This way you ensure that committed code always conforms to your rules
  (regardless of which editor).
- How to integrate these tools into Vim,
  so that while editing files, you can get changes applied live.

### Four examples of tools

Note that each tool has a configuration file that you should `git add`
if you are planning to use it.

#### codespell: the spell-checker

[codespell](https://github.com/codespell-project/codespell)
uses a _list of common misspellings_ rather than a dictionary of bad ones,
so it doesn't tend to generate as many false-positives.

If you did hit a false-positive, you'll need to tell codespell to ignore it.
To do this, you add `.codespellrc` to `~/mysite` with something like:

```ini,linenos,name=.codespellrc
[codespell]
ignore-words-list=foo,bar
```

#### prettier: the JavaScript and CSS formatter

[prettier](https://prettier.io/)
works pretty well with default settings;
I usually don't add a configuration file for this one.

#### djlint: the Tera HTML template formatter

[djlint](https://djlint.com/) is a formatter for the Tera HTML templates
that we'll be using with Zola.
For it to work, you need to create a single file `djlint.toml`
that tells djlint that we're using Tera-style templates.

```ini,linenos,name=djlint.toml
profile = "tera"
```

#### rumdl: the Markdown formatter

[rumdl](https://rumdl.dev/) is a comprehensive Markdown formatter.
It is controlled using the configuration `rumdl.toml`.

There are a _lot_ of rules used in this one.
You'll probably want to turn some of the rules on and off to taste.
Here's the configuration this project is using.
This configuration disables some builtin rules that I personally don't like,
then enables two optional rules (MD060, MD088)
and then tweaks some knobs on some other rules

```toml,linenose,name=rumdl.toml
[global]
disable = [
  "MD014", # Permit $ with no output in code listings
  "MD025", # Allow multiple h1 headings
  "MD026", # Permit punctuation in heading
  "MD033", # Permit inline HTML
  "MD041", # Allow first line to be anything (not just h1)
]
extend-enable = [
  "MD060", # Table alignment
  "MD088", # Insist on ASCII quotes
]
respect-gitignore = true

[MD013] # Line length
line-length = 160
code-blocks = false
tables = false
headings = false

[MD060] # Table alignment
enabled = true
style = "aligned"

[MD088] # Insist on ASCII quotes
enabled = true
normalize-quotes = true
normalize-dashes = false
```

### Adding linters and formatters into prek

The four tools above all have pre-commit support,
so you can add whichever sections you want at the bottom of your `prek.toml`:

```toml,linenos,linenostart=15,name=prek.toml
# … earlier lines not shown …

[[repos]]
repo = "https://github.com/codespell-project/codespell"
rev = "v2.4.3"
hooks = [{ id = "codespell" }]

[[repos]]
repo = "https://github.com/JoC0de/pre-commit-prettier"
rev = "v3.10.1"
hooks = [{ id = "prettier", types_or = ["javascript", "css", "scss"] }]

[[repos]]
repo = "https://github.com/djlint/djLint"
rev = "v1.50.4"
hooks = [
  { id = "djlint-reformat", types = ["html"], files = "templates/" },
  { id = "djlint", types = ["html"], files = "templates/" }
]

[[repos]]
repo = "https://github.com/rvben/rumdl-pre-commit"
rev = "v0.3.4"
hooks = [{ id = "rumdl" }, { id = "rumdl-fmt" }]
```

### Adding linters into Vim via ALE

This is possible, but I think it's quite a bit of installation headache.
I'm going to give an approach that uses only Vim,
but truthfully you might want to start using something like
[Neovim](https://neovim.io/) at this point.

We're going to use the Vim plugin [ALE](https://github.com/dense-analysis/ale).
ALE has two different functionalities:
for _linters_ (which provide diagnostics if you make mistakes)
and for _formatters_ (which run the formatter whenever you save).
ALE configures linters automatically,
but formatters have to be turned on in your configuration file.
ALE has the following setup:

| Tool      | Linter        | Formatter     |
| --------- | ------------- | ------------- |
| codespell | Not supported | Not supported |
| djlint    | Supported     | Supported     |
| prettier  | Not supported | Supported     |
| rumdl     | Supported     | Supported     |

To get this setup to work,

- Install the Tools into your _system_, and then
- Install ALE itself and turn on the formatters.

#### Installing the linters

The `prettier` formatter is popular enough you should be able to install
it using your system package manager.
But `rumdl` and `djlint` might not be.
Fortunately, Astral's `uv` tool can get the others,
and is _also_ popular enough that your package manager should have it.

If you're using Homebrew, the following should work:

```console
$ brew install prettier uv
…

$ uv tool install djlint
Resolved 9 packages in 4.01s
Prepared 7 packages in 456ms
Installed 9 packages in 7ms
 + click==8.5.1
 + cssbeautifier==2.0.3
 + djlint==1.54.0
 + editorconfig==0.17.1
 + jsbeautifier==2.0.3
 + json5==0.17.3
 + pathspec==1.1.1
 + pyyaml==6.0.3
 + regex==2026.11.19
Installed 1 executable: djlint

$ uv tool install rumdl
Resolved 1 package in 678ms
Prepared 1 package in 851ms
Installed 1 package in 9ms
 + rumdl==0.3.5
Installed 1 executable: rumdl

$ which djlint
/home/evan/.local/bin/djlint

$ which rumdl
/home/evan/.local/bin/rumdl
```

If you're using a different package manager (`apt` on Ubunt,
`pacman` on Arch Linux, etc.), replace the first line accordingly.
And if you get a warning about `uv tool update-shell`,
you should run that and restart the shell.

One warning: the versions of the linters
you get may not match the versions in `prek.toml`.
If so, you probably want to bump the versions in `prek.toml` to match.
Otherwise, the editor and pre-commit hooks may have subtly different results,
which can be fairly annoying.

#### ALE and configuration

[ALE](https://github.com/dense-analysis/ale) is a popular plugin for Vim
that will handle IDE-like things for you.
You can install it like this:

```console
$ mkdir -p ~/.vim/pack/git-plugins/start

$ git clone --depth 1 https://github.com/dense-analysis/ale.git ~/.vim/pack/git-plugins/start/ale
Cloning into '/home/evan/.vim/pack/git-plugins/start/ale'...
remote: Enumerating objects: 2678, done.
remote: Counting objects: 100% (2678/2678), done.
remote: Compressing objects: 100% (1812/1812), done.
remote: Total 2678 (delta 601), reused 1909 (delta 447), pack-reused 0 (from 0)
Receiving objects: 100% (2678/2678), 1.16 MiB | 7.87 MiB/s, done.
Resolving deltas: 100% (601/601), done.
```

If this works correctly, linters should already be working when you open Vim.
Open one of the Markdown files and see if `:ALEInfo` shows you anything.

Formatters are more disruptive, since they'll actually change your file,
so those need configuration in`~/.vimrc`.

```vimscript,linenostart=12,name=~/.vimrc
let g:ale_fixers = {
\   '*': ['remove_trailing_lines', 'trim_whitespace'],
\   'htmldjango': ['djlint'],
\   'scss': ['prettier'],
\   'markdown': ['rumdl'],
\}
let g:ale_fix_on_save = 1
```

That should enable format-on-save.
