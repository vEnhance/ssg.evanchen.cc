+++
title = "§1. Overview"
weight = 10
+++

## Overview

### §1.A. The pieces we'll use

Here are the pieces of technology that were used to build this site.

- **[Vim](https://vim.org): Text editor**.\
  Edits text files.

- **[Git](https://git-scm.com/): Version control**.\
  Manages and distributes source code including revision history.

- **[Prek](https://prek.j178.dev): Pre-commit framework**.\
  Sanity check guards on Git repository.

- **[Zola](https://getzola.org/): Static site generator**.\
  Takes Markdown, Sass, and templates and generates HTML.

- **[GitHub Pages](https://docs.github.com/en/pages): Deployment system**.\
  Publishes the generated HTML to `*.github.io`.

- **[Cloudflare](https://cloudflare.com) (optional): All-around Internet servicer**.\
  Purchasing domains and pointing them to the deployment system.

And here is a picture showing how they all fit together.
(You should [download the full-size diagram](/d2/bigpicture.svg);
it's likely too small to read inline.)

[![Overview of the entire system](/d2/bigpicture.svg)](/d2/bigpicture.svg)

An important thing to note is that
**pretty much all these components can be swapped out**.
If you have a friend helping you out, for example,
they might decide Vim is ~~a trial by fire~~ too complicated for newbies,
and tell you to use VSCode instead.
Or, there are an [ocean of static site generators][ssg-basin] (Jekyll, Hugo, …)
and your friend might prefer a different one.

[ssg-basin]: https://rakhim.org/honestly-undefined/19/

### §1.B. How to use this guide

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

### §1.C. This isn't a cookbook!

Accordingly, this guide will _not_ be just a list of commands to copy and paste.
I want to do this more with broad strokes, like,
this is roughly what this piece does and what you should be trying to do.
Then you can ask the friend or LLM to fill in the details and debug issues.

Overall I'd say this guide is more like

1. a lesson plan for your friend; or
2. a starting place for your own research

than something you should follow exactly as written.
