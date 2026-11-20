+++
title = "§4½. Prek"
weight = 45
+++

## Prek

Undoing committed mistakes is tough, so we want to avoid it if we can.
In Git, a **pre-commit hook** is a guard that helps prevent mistakes from sneaking in to begin with.
It runs automatically every time you try to "git commit";
if it notices something wrong, it'll yell at you and abort the commit.

One example of a mistake I see all the time is
[trailing whitespace](https://stackoverflow.com/q/21410075/4826845).
People are often unaware that they have dangling spaces at the end of the lines.
But _I_ can see them, and like `.DS_Store`, this annoys the heck out of me.
You will look less noob-ish if you never let trailing whitespace into your work.

For this tutorial,
we'll use the [prek framework](https://prek.j178.dev/),
which manages pre-commit hooks in an organized fashion.

### Setting up Prek

To start, run `prek init` inside `~/mysite` which creates `prek.toml`.
That can then be edited to specify which kinds of hooks you want.

```console
$ pwd
/home/evan/mysite

$ prek init
Created `prek.toml`
Installed Git hook at `.git/hooks/pre-commit`

$ vim prek.toml
```

Here's a starting configuration for some common errors
(see [the manual for what they do](https://prek.j178.dev/reference/built-in-hooks/#check-added-large-files)):

```toml,linenos,name=prek.toml
# Configuration file for `prek`, a git hook framework written in Rust.
# See https://prek.j178.dev for more information.
#:schema https://www.schemastore.org/prek.json

[[repos]]
repo = "builtin"
hooks = [
    { id = "check-merge-conflict" },
    { id = "check-toml" },
    { id = "detect-private-key" },
    { id = "end-of-file-fixer" },
    { id = "fix-byte-order-marker" },
    { id = "mixed-line-ending" },
    { id = "trailing-whitespace" },
]
```

This is an example of a logical change, so let's commit it.

```console
$ git status
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
  prek.toml

nothing added to commit but untracked files present (use "git add" to track)

$ git add prek.toml

$ git commit -m "ci: create a prek.toml"
check for merge conflicts............................Passed
check toml...........................................Passed
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main ecd69d2] ci: create a prek.toml
  1 file changed, 15 insertions(+)
  create mode 100644 prek.toml
```

You can see `prek` is doing its work now!
That's what each "Passed" is, one for each hook we chose.

### Linters and formatters

In addition to the builtin prek hooks,
you can add a _lot_ more guards into your work.
You can broadly sort these into **linters**
(which usually look for errors related to a specific language)
and **formatters** (which handle things like indentation style).

If that sounds enticing to you,
you can check [§L on linters and formatters](@/linters.md) page
for examples used by `ssg.evanchen.cc` that you might want to borrow.
You can do that now, later, or not at all; this is optional.
