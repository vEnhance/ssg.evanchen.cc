+++
title = "§L. Linters"
weight = 85
+++

## Linters and formatters

As mentioned in [§4½ on Prek](@/prek.md),
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

```toml,linenos,name=rumdl.toml
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

In [§2½ on Vim](@/vim.md), I recommended installing the
[ALE](https://github.com/dense-analysis/ale) plugin.
ALE lets you talk to the tools above directly within Vim.
If you haven't already, run the install command there.

ALE has two different functionalities:

- _linters_, which provide diagnostics if you make mistakes
- _formatters_, which run the formatter whenever you save.

ALE doesn't support every tool in both capacaties.
For the four tools above, here is the support chart:

| Tool      | Linter        | Formatter     |
| --------- | ------------- | ------------- |
| codespell | Not supported | Not supported |
| djlint    | Supported     | Supported     |
| prettier  | Not supported | Supported     |
| rumdl     | Supported     | Supported     |

In any case, we start by installing the tools onto the computer itself.
(Prek doesn't do this: it manages the tools separately.)
All of them are on Homebrew, although we skip `codespell` since ALE doesn't
support it anyway:

```console
$ brew install prettier rumdl djlint
```

One warning: the versions of the linters you install from this
you get may not match the versions in `prek.toml`.
If so, you probably want to bump the versions in `prek.toml` to match.
Otherwise, the editor and pre-commit hooks may have subtly different results,
which can be fairly annoying.

Linters will start working right away once they're installed on the computer.
Formatters are more disruptive, since they'll actually change your file,
so those need configuration in `~/.vimrc`, which you can add:

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

If set up correctly, Vim will now provide diagnostics from `djlint` and `rumdl`
directly inside the editor, and apply all the formatters on save.
You can use `:ALEInfo` in command line mode to get a pulse on if ALE is working.
