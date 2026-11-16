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

- How to integrate these tools into `prek`, and
- How to integrate them into Vim.

### Four examples of tools

#### codespell: the spell-checker

[`codespell`](https://github.com/codespell-project/codespell)
is a spellchecker.
It uses a _list of common misspellings_ rather than a dictionary of bad ones,
so it doesn't tend to generate as many false-positives.

If you did hit a false-positive, you'll need to tell codespell to ignore it.
To do this, you add `.codespellrc` to `~/mysite` with something like:

```ini,linenos,name=.codespellrc
[codespell]
ignore-words-list=foo,bar
```

#### prettier: the JavaScript and CSS formatter

[prettier](https://prettier.io/)
is a formatter which I typically use JavaScript and CSS.
The default settings work fine for me.

#### djlint: the HTML template formatter

[djlint](https://djlint.com/) is a formatter for the Tera HTML templates
that we'll be using with Zola.
For it to work, you need to create a single file `djlint.toml`
that tells djlint that we're using Tera-style templates.

```ini,linenos,name=djlint.toml
profile = "tera"
```

#### rumdl: the Markdown formatter

[rumdl](https://rumdl.dev/) is an extremely opinionated Markdown formatter.
It is controlled using the configuration `rumdl.toml`.

There are a _lot_ of rules used in this one.
You'll probably want to turn some of the rules on and off.
Here's the configuration this project is using:

```toml,linenose,name=rumdl.toml
[global]
disable = [
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

[MD014]
show-output = false # some silent commands
```

### Adding linters and formatters into prek

The four tools above all have pre-commit support,
so you can add whichever sections directly into `prek.toml`:

```toml,linenos,filename=prek.toml
[[repos]]
repo = "https://github.com/codespell-project/codespell"
rev = "v2.4.3"
hooks = [{ id = "codespell" }]

[[repos]]
repo = "https://github.com/JoC0de/pre-commit-prettier"
rev = "v3.10.1"
hooks = [{ id = "prettier", types_or = ["css", "javascript", "json", "scss", "ts", "yaml"] }]

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

Here's a rundown of each one.
Note that each one has a configuration file,
that you'll need to `git add` as well.

### Adding linters into Vim via ALE
