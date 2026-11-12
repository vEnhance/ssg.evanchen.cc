+++
title = "Overview"
weight = 10
[extra]
sec_number = 1
+++

## Overview

### The pieces we'll use

Here are the pieces of technology that were used to build this site.

| Technology               | Kind of thing                   | What it's for                                                  |
| ------------------------ | ------------------------------- | -------------------------------------------------------------- |
| [Git][git]               | Version control                 | Manages and distributes source code including revision history |
| [Vim][vim]               | Text editor                     | Edits text files                                               |
| [Prek][prek]             | Pre-commit framework (optional) | Sanity check guards on Git repository                          |
| [Zola][zola]             | Static site generator           | Takes Markdown, SASS, and templates and generates HTML         |
| [GitHub Pages][gh-pages] | Deployment system               | Publishes the generated HTML to `*.github.io`                  |
| [Cloudflare][cf]         | All-around Internet servicer    | Purchasing domains and pointing them to the deployment system  |

[git]: https://git-scm.com/
[zola]: https://getzola.org/
[prek]: https://prek.j178.dev
[vim]: https://vim.org
[gh-pages]: https://docs.github.com/en/pages
[cf]: https://cloudflare.com

An important thing to note is that
**pretty much all these components can be swapped out**.
If you have a friend helping you out, for example,
they might decide Vim is ~~a trial by fire~~ too complicated for newbies,
and tell you to use VSCode instead.
Or, there are an [ocean of static site generators][ssg-basin] (Jekyll, Hugo, …)
and you friend might prefer a different one.

[ssg-basin]: https://rakhim.org/honestly-undefined/19/

### How to use this guide

Recommendation: **find a friend who can spare a few hours to sit by you
and walk you through this guide**.
It'll be helpful to have someone who can answer your questions right away
and fight inevitable error messages.
I've done this a few times,
and it seems to take on the ballpark of a couple hours.

If you don't have hacker friends, you could probably also use an LLM.
Almost everything that's in this guide is considered really basic
and a frontier LLM should have pretty much no difficulty with it.

However, if you have friends who can guide you along,
I still want to encourage you to spend time with the people in your life.
When's the last time you got to learn something together over a cup of coffee?
Anyway, I tried.

### This isn't a cookbook!

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
