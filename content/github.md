+++
title = "§7. GitHub"
weight = 70
+++

## Wiring up GitHub

At this point, we have a minimally functional website.
To recall the picture of [§1.A.](@/overview.md),
we've now covered all the steps inside "Your Computer"
from the [big picture](/d2/bigpicture.svg).

![A repeat of the picture from §1.A. We've now done the left half.](/d2/bigpicture.svg)

This final section will publish our website onto `venhance.github.io`.

A quick warning: as of 2026, GitHub has been
[struggling with reliability](https://mrshu.github.io/github-statuses/),
and a lot of prominent projects have recently left GitHub
(e.g. [Zig](https://ziglang.org/news/migrating-from-github-to-codeberg/),
[ghostty](https://mitchellh.com/writing/ghostty-leaving-github)).
The trend seems to be to move towards [Codeberg](https://codeberg.org/).

### §7.A. Creating a repository on GitHub

Register an account on <https://github.com> if you haven't already,
and then set up a new repository.
You can name the repository as you see fit.
For this tutorial to work, you should make it a **public repository**,
since GitHub Pages (at the time of writing) is only free there.

A standard practice is to pick the repo name to be the same as the
URL you want to publish the site to.
If you're using the free `github.io` domain,
that means `venhance.github.io` as the repository name.
So the repository's full name should look like `vEnhance/venhance.github.io`.

Our plan is to use GitHub Pages, so while you're here,
let's turn that in the repository settings.
Go under Settings, then to Pages, and under GitHub Pages,
and change "Source" to "GitHub Actions".

(One other setting I always change right away when I create a new repository:
under "General > Pull Requests", uncheck "Allow merge commits".
I really don't like merge commits.)

### §7.B. Setting up an SSH key

The recommended protocol for talking to GitHub is not HTTPS,
but rather a different one called [SSH](https://en.wikipedia.org/wiki/SSH).
Follow the [GitHub documentation][gh-docs-ssh-top] to do the following:

[gh-docs-ssh-top]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

1. Create a [new SSH public and private key][gh-docs-ssh-new].
   To do this, you can just run `ssh-keygen -t ed25519 -C user@example.com`
   anywhere in your terminal, with `user@example.com` being the email
   you registered on GitHub with.
   Using a passphrase is recommended, pick something you'll remember.
2. [Add the SSH public key][gh-docs-ssh-add], so GitHub knows about it.
   This probably means copying the content of `~/.ssh/id_ed25519.pub`
   and then pasting it into your Account Settings on GitHub.

That's it! You can check if it's working with `ssh -T git@github.com`.
All told:

```console
$ ssh-keygen -t ed25519 -C "user@example.com"
Generating public/private ALGORITHM key pair.
Enter a file in which to save the key (/home/evan/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:

$ cat ~/.
ssh-ed25519 AAAA… user@example.com

$ ssh -T git@github.com
Hi vEnhance! You've successfully authenticated,
but GitHub does not provide shell access.
```

### §7.C. Push the repository

You can now configure `~/mysite` to have GitHub as a remote.
This means you can now `git push` to the repository (to send your updates to GitHub)
and conversely `git pull` (if you want to pull updates from GitHub).

```console
$ cd ~/mysite

$ git remote add origin git@github.com:vEnhance/example.com

$ git push -u origin main
TODO
```

(A word of warning: once you `git push` work — at least to the `main` branch,
but we only have the `main` branch in this tutorial —
the usual best practice is to consider it _set in stone_.
Treat pushed commits as permanently published
and don't rewrite them with `git commit --amend`, `git rebase -i`, etc.)

### §7.D. Setting up the GitHub Action

We earlier set up GitHub Pages to use a so-called "Action".
In GitHub, this means adding a certain file that tells GitHub
that our repository uses Zola and what to do.
The special location for this is `.github/workflows/*.yml`.
Use `mkdir -p .github/workflows/` in `~/mysite` to create the folder,
then you can just copy-paste the one used by this project.
(It's named `deploy.yml`, but the name doesn't matter.)

```yaml,linenos,name=.github/workflows/deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

# No permissions by default; each job requests only what it needs.
permissions: {}

# Allow one concurrent deployment; let an in-progress run finish.
concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read # to check out the repository
    steps:
      - name: Checkout
        uses: actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4
        with:
          persist-credentials: false

      - name: Build Zola + upload Pages artifact
        uses: getzola/github-pages@435a833fd3f1edcb28d70576c89e30a6365cd8f7 # v1
        with:
          zola_version: v0.23.6 # keep in sync with the local zola version

  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      pages: write # to deploy to Pages
      id-token: write # to verify the deployment originates from this workflow
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@cd2ce8fcbc39b97be8ca5fce6e763baed58fa128 # v5
```

Commit this, and push it to GitHub.

```console
$ git add .github/workflows/deploy.yml

$ git commit -a -m "ci: Set up GitHub Pages workflow for Zola"
TODO

$ git push
TODO
```

If that works, and you look at the repository on GitHub,
you'll see an orange circle
and eventually a green one next to the commit if it passes.
If so, going to <https://venhance.github.io/> should show the website.

You're done! 🎉

[gh-docs-ssh-new]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
[gh-docs-ssh-add]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
