+++
title = "Where to next?"
weight = 80
+++

## Where to go from here

Congratulations on getting your `github.io` website up and running.
Here are some suggestions for directions you might want to explore next.

### Custom domains

See [§C. Cloudflare](@/cloudflare.md) if you want to purchase your own domain!

### What to write on your website (it doesn't have to be a blog!)

If you're thinking about writing essays or a blog, I like
[Blogging Advice for People Exactly Like Me](https://blog.vero.site/post/blogging-advice).
In particular, one thing that's pointed out in there
(that won't be obvious from the title!)
is that you don't _have_ to frame your thoughts and content
as a reverse-chronological list of posts.
See Amy Hoy's [How the Blog Broke the Web](https://stackingthebricks.com/how-blogs-broke-the-web/).

### Embellishments

For <https://ssg.evanchen.cc> itself,
we added a few small embellishments
that we didn't get to in [§6 on Sass](@/styling.md).
Some examples:

- In the templates:
  - The template [`index.html`][repo:index.html]
    additionally has an automatic full table of contents.
    This uses [Zola's built-in ToC support][zola:toc].
  - The template [`page.html`][repo:page.html]
    got some prev/next navigation
    (with the style set in [`tlayout.scss`][repo:tlayout.scss]).
- In the stylesheets:
  - Source code listings were made pretty using
    [Zola's syntax highlighting support][zola:syn] together with
    rules in [`codeblocks.scss`][repo:codeblocks.scss].
  - The inline screenshots and diagrams got their border and width constraints
    from [`images.scss`][repo:images.scss].
    (The SVG diagrams are via [d2](https://d2lang.com/), if you're curious).

Zola also has [theme support][zola:theme] for more packaged styling.

[zola:toc]: https://www.getzola.org/documentation/content/table-of-contents/
[zola:syn]: https://www.getzola.org/documentation/content/syntax-highlighting/
[zola:theme]: https://www.getzola.org/documentation/themes/installing-and-using-themes/
[repo:codeblocks.scss]: https://github.com/vEnhance/ssg.evanchen.cc/blob/main/sass/codeblocks.scss
[repo:images.scss]: https://github.com/vEnhance/ssg.evanchen.cc/blob/main/sass/images.scss
[repo:tlayout.scss]: https://github.com/vEnhance/ssg.evanchen.cc/blob/main/sass/tlayout.scss
[repo:page.html]: https://github.com/vEnhance/ssg.evanchen.cc/blob/main/templates/page.html
[repo:index.html]: https://github.com/vEnhance/ssg.evanchen.cc/blob/main/templates/index.html

### Even more CSS (and CSS frameworks, etc.)

As I said, [§6 on Sass](@/styling.md) is so minimalistic I don't think
it even qualifies as "Introduction to CSS", just "here's some random commands".
If you want to get more serious about web design,
that's something you can really deep dive into.
My haphazard learning process was repeatedly searching things like
"css how to put two buttons side by side" every time I needed to do anything,
and then immediately forgetting the keyword.
If you'd prefer a more structured approach from first principles,
<https://developer.mozilla.org/en-US/docs/Web/CSS> is a good reference.

On the other hand, writing just CSS/Sass by itself from scratch
is likely not the "modern" way to do things.
There are frameworks like [Bootstrap](https://getbootstrap.com/)
or [Tailwind](https://tailwindcss.com/)
that will give you a big toolbox of components.

On the _other_ other hand, here's a counterpoint:
Do you really _want_ to do things the modern way?
Don't you feel that all websites in 2026 look exactly the same now?
And that someone decided pages shouldn't have words on them, just hero images?
I had fun reading the essay
[The Website Obesity Crisis](https://idlewords.com/talks/website_obesity.htm)
and that was written in 2015.
I think it's even worse now.

### Comments

If you go the blog route, comments are quite tricky,
because they don't naturally work with a static site.
(Comments are dynamic.)
You probably need a second external system for this.

Examples of systems along these lines are
[isso-comments](https://github.com/isso-comments/isso/),
[remark42](https://github.com/umputun/remark42),
[utterances](https://github.com/utterance/utterances).

### Upgrading your tools

We used the most primitive tools in this tutorial,
but here are some things that more experienced users might consider an upgrade.

- Bash is the most basic shell,
  but there are others like [Zsh](https://zsh.org)
  (now the default on macOS) and [fish](https://fishshell.com/).
  These can get nice plugins and themes as well.
  For example, you can have the directory and Git status automatically printed
  in the prompt, so you don't need to keep doing `pwd` or `git status` by hand.

- Git itself can be extensively configured with aliases or settings
  (via `~/.gitconfig`).
  For example, you could replace the `diff` pager
  with [delta](https://github.com/dandavison/delta),
  for a much better experience when using `git diff`.

- The terminal itself (around the underlying shell) can also be replaced.
  [ghostty](https://ghostty.org/) is a nice cross-platform one.

- There's also stuff like [lazygit](https://github.com/jesseduffield/lazygit)
  if you want a terminal UI for Git rather than just typing commands.
  Or just ditch Git for [jujutsu](https://github.com/jj-vcs/jj)
  (which is compatible with Git).

- We mentioned this in [§4½ on Prek](@/prek.md),
  but linters and formatters are pretty cool.
  Some hints are in [§L on those tools](@/linters.md).

- On editors, besides classic Vim, there's the modern version Neovim.
  Both of them have a vast plugin ecosystem.
  And then there are other editors like Helix or Visual Studio Code.

- AI agents exist. Usual warnings apply.
  Here's one example of a
  [blog post on AI workflows by Mitchell Hashimoto](https://mitchellh.com/writing/my-ai-adoption-journey),
  written in February 2026.
  If you're going to do this, you should consider using
  a sandbox like [Bubblewrap](https://patrickmccanna.net/a-better-way-to-limit-claude-code-and-other-coding-agents-access-to-secrets/),
  a separate user account, or a separate physical machine.
