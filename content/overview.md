+++
title = "Getting Started"
weight = 10
+++

# Getting Started

## Bird's eye-view

Here are the pieces of technology that were used to build this site.

| Technology               | Kind of thing                   | What it's for                                                  |
| ------------------------ | ------------------------------- | -------------------------------------------------------------- |
| Git                      | Version control                 | Manages and distributes source code including revision history |
| [Vim][vim]               | Text editor                     | Edits text files                                               |
| [Prek][prek]             | Pre-commit framework (optional) | Sanity check guards on Git repository                          |
| [Zola][zola]             | Static site generator           | Takes Markdown, SASS, and templates and generates HTML         |
| [GitHub Pages][gh-pages] | Deployment system               | Publishes the generated HTML to `*.github.io`                  |
| [Cloudflare][cf]         | All-around Internet servicer    | Purchasing domains and pointing them to the deployment system  |

An important thing to note is that aside from Git
**pretty much all these components can be swapped out**.
If you have a friend helping you out, for example,
they might decide Vim is ~~a trial by fire~~ too complicated for newbies,
and tell you to use VSCode instead.
Or, there are an ocean of static site generators (Jekyll, Hugo, …)
and you might have someone suggest a different one.

## How to use this guide

Recommendation: **find a friend who can spare a few hours to sit by you
and walk you through this guide**.
It'll be helpful to have someone who can answer your questions right away
or knows what to do with things that are broken.
I've done this a few times, and it seems to take on the ballpark of 2 hours,
depending on the person.

If you don't have nerdy friends, you could probably also use an LLM.
Almost everything that's in this guide is considered really basic
and a frontier LLM should have pretty much no difficulty with it.

However, if you do have friends who can guide you along,
I still want to encourage you to spend time with the people in your life.
When's the last time you got to learn something together?
Anyway, I tried.

## This isn't a cookbook!

Accordingly, this guide will _not_ be written as a step-by-step tutorial,
where I tell you every command to type and you copy and paste it in line by line.
I'm worried about people just blindly pressing `Ctrl-V` with no understanding
of what the commands are doing; that's not so good for learning.

I want to do this more with broad strokes, like,
this is roughly what this piece does and what you should be trying to do.
Then you can ask the friend or LLM to fill in the details and debug issues.

So, overall I'd say this guide is more like

1. a lesson plan for your friend; or
2. a starting place for your own research

instead of something like a cookbook to follow.

[zola]: https://getzola.org/
[prek]: https://prek.j178.dev
[vim]: https://vim.org
[gh-pages]: https://docs.github.com/en/pages
[cf]: https://cloudflare.com
