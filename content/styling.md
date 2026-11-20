+++
title = "§6. Styling"
weight = 60
+++

## Styling with SCSS

We now can generate HTML pages.
That's cool and all, but it'd be nice
if we could add some more customization on top of this.

Historically, styling has been done with [CSS](https://en.wikipedia.org/wiki/CSS).
Zola allows us to use [Sass](https://sass-lang.com/): "CSS with superpowers".
That sounds about right to me.
Since Zola is supporting Sass for us out of the box anyway, we'll use it;
the file extension we'll use for Sass is `*.scss`.
(The really basic stuff we're doing won't actually use the extra power,
but there's no real cost here to using Sass,
since all valid CSS is also valid SCSS.)

**I am going to do only a few small things here as a proof-of-principle.**
CSS is a real beast; you can do a _lot_ with CSS.
Again, this should be treated more like
"this is what a few lines of CSS can do as a demo",
not even "an introduction to CSS".

### §6.A. A T-shaped layout

**Text is most readable with no more than 80 or so characters per line.**
If you look at any website I've built,
you'll notice it's narrower than many other websites or documents.
But by default, if you have no styling,
an HTML document just runs the entire width of the browser page.

So the first thing I'm going to do is put a bit of CSS that arranges
the navigation into a sidebar at the left,
with a header running the full top.
I'm also going to constrain the wrapper to not be too wide,
and center it.

Recall the overall skeleton of `templates/layout.html`:

```html
<div id="wrapper">
    <header id="layout-header">
        …
    </header>
    <aside id="layout-nav">
        …
    </aside>
    <article id="layout-content">
        …
    </article>
</div>
```

We gave four elements unique ID's that will let us refer to them as
`#wrapper`, `#layout-header`, and so on.
Those are the selectors we'll use in our stylesheet.

The stylesheets live in `sass/`; let's make a file in there called `tlayout.scss`.
We'll also need to point to `tlayout` from `templates/layout.html`.
(At this point I'm going to stop typing `pwd` and `vim …` everywhere;
you know the drill by now.)
Here are the lines that give this style, full of comments:

```sass,linenos,name=sass/tlayout.scss
/* === T-SHAPE LAYOUT: Full-row header and 8em nav bar on left === */
#wrapper {
  display: grid; // make the three elements of #wrapper form a grid
  grid-template-columns: 8em 1fr; // allocate 8em to nav, rest to right col
  column-gap: 1em; // add a small gap between the two columns of the grid
  max-width: 45em; // maximum width of page to not overwhelm large displays
  margin: 0 auto; // center the wrapper relative to the entire page
}
#layout-header {
  grid-column: 1 / -1; // from the first column to the last column
}
```

(To explain the units:
`em` is one of the many units of CSS; it represents the height of the font.
I remember it as "width of capital M", even though that's not literally true.
See [StackOverflow](https://stackoverflow.com/a/609561).
And `1fr` means "everything left over".)

If you point your browser at <http://127.0.0.1:1111/tlayout.css>,
you'll see Zola has now created a new (compressed) version of this file.
We need to go back to our base template and add a `link` to it.
Update the `<head>` block with the new line:

```html-derivative,linenos,linenostart=3,name=templates/layout.html,hl_lines=5-6
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>{% block html_title %}MySite{% endblock html_title %}</title>
    <link rel="stylesheet"
          href="{{ get_url(path='tlayout.css', cachebust=true) }}" />
</head>
```

(If you took my advice to install `vim-gitgutter` back in [§2½ on Vim](@/vim.md),
then you might notice as you do these edits that there are plus signs
appearing in the left of Vim as you edit the text.
I can explain what those are now.
The `vim-gitgutter` plugin puts markers for edited and added lines
relative to what's been staged or committed.
This makes easier to see what is new since the last commit.
You're welcome.)

The `get_url` is another Zola built-in.
You could just type `href="/tlayout.css"`, but using `get_url`
has the advantage that Zola will yell at you if you typo the file name
or it otherwise can't find the file.
(Without this, it would silently fail.)
The `cachebust=true` also makes sure that the user's browser
will get a fresh copy of the file if it ever changes
(search "cache busting" for more on this).

We're also going to change the `for` loop to have the links in paragraphs `p`,
so they all lie on a new line.
Update that loop to have a surrounding `<p>` tag as well.

```jinja,linenos,linenostart=17,name=templates/layout.html
{% for p in root.pages %}
    <p>
        <a href="{{ p.permalink }}">{{ p.title }}</a>
    </p>
{% endfor %}
```

Now, <http://127.0.0.1:1111> should pick up those changes!

![The CSS layout starting to take shape](/screenshots/tlayout.webp)

This is a single logical change, so we commit it.
Here, I'm going to mention that there's actually a shortcut
`git add -A` that means "add everything, even new untracked files".
So instead of typing `git add templates/layout.html sass/`,
if you're sure you've made no other changes,
`git add -A` will do the job.
(Use with caution, though; this is how `.DS_Store` is born.)

```console
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
  modified:   templates/layout.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
  sass/

no changes added to commit (use "git add" and/or "git commit -a")

$ git add -A

$ git commit -m "feat: T-shaped layout"
check for merge conflicts............................Passed
check toml.......................(no files to check)Skipped
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main fd2c440] feat: T-shaped layout
  2 files changed, 16 insertions(+), 1 deletion(-)
  create mode 100644 sass/tlayout.scss
```

### §6.B. Colors and fonts

We can go back to `sass/tlayout.scss` and add more.
Let's do colors and fonts.
Here's something close to what `ssg.evanchen.cc` is using;
feel free to tweak the font size, colors, font names to your taste.

```sass,linenos,linenostart=13,name=sass/tlayout.scss
/* === FONTS AND COLORS === */
body {
  background-color: #fff6f6; // slightly off-white background color
  font-family: "Source Serif 4", serif; // default font for everything
  font-size: 1.3rem; // 20.8px with a default browser setting of rem=16px
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: "Exo 2", sans-serif; // all headings use Exo 2
}

/* === RESTYLE LINKS: dotted underline, change color on hover === */
a {
  color: #0033ff;
  text-decoration: underline dotted;
  text-decoration-thickness: from-font; // FF and chrome differ without this
}
a:hover {
  color: #33aaff;
  text-decoration: underline;
}
```

The first section is just tweaking the fonts and colors of the main body.

Colors will work right away, but **fonts require an actual provider**.
(Chances are a random web user doesn't have, say, "Source Serif 4" installed.)
I usually just use [Google Fonts](https://fonts.google.com) for this,
where you can browse a selection of fonts and get a code snippet
that loads the fonts you like.
When you get the fonts you want, you paste `link` code that Google gives you
above your existing stylesheet.

```html-derivative,linenos,linenostart=3,name=templates/layout.html,hl_lines=5-8
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>{% block html_title %}MySite{% endblock html_title %}</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?…[long URL goes here]…"
          rel="stylesheet">
    <link rel="stylesheet"
          href="{{ get_url(path='tlayout.css', cachebust=true) }}" />
</head>
```

The very long URL is truncated above, but it has the fonts you chose.
So you have to make sure that URL matches the font names you picked in your stylesheet.
If all goes well:

![With a few color and font choices](/screenshots/colored.webp)

We commit this change.
Since I just taught you about `git add -A`,
here's another shortcut for you: `git commit -a`.
It's short for `git add [all changed files]` followed by `git commit`.
But unlike `git add -A`, it doesn't add untracked files
(so no `.DS_Store` risk).

```console
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
  modified:   sass/tlayout.scss
  modified:   templates/layout.html

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -a -m "feat: fonts and colors"
check for merge conflicts............................Passed
check toml.......................(no files to check)Skipped
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main 8228a46] feat: fonts and colors
  2 files changed, 30 insertions(+)
```

### §6.C. Mobile support

People view websites on cell phones.
Drives me nuts, but that's how it is.
Since phones and laptops are nothing alike,
you probably want to make the website display differently on a phone.

So we add a few more lines into `sass/tlayout.scss`:

```sass,linenos,linenostart=39,name=sass/tlayout.scss
/* === MOBILE SUPPORT: For narrow width, 1 column, nav at bottom === */
@media screen and (max-width: 55rem) {
  #wrapper {
    grid-template-columns: 1fr; // revert to a single column
  }
  #layout-nav {
    order: 1; // this bumps the navigation down to the bottom
  }
}
```

The `@media` directive means that the lines in this clause
only apply for screens whose width is at most `55rem`
(`rem` is a variant of `em` that is based on the document root).
It gets rid of the T-shape altogether and moves the navigation to the bottom.
So you might get something that looks like this on a phone display:

![Mobile layout: navigation moves to bottom](/screenshots/mobile.webp)

That's good enough for now.
Let's commit all this before we go on to deploying.

```console
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
  modified:   sass/tlayout.scss

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -a -m "feat: only use one column on mobile"
check for merge conflicts............................Passed
check toml.......................(no files to check)Skipped
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main ba666af] feat: only use one column on mobile
  1 file changed, 10 insertions(+)
```
