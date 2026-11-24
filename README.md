# ssg.evanchen.cc

These are the source files for <https://ssg.evanchen.cc>,
a short guide on how to set up a website using an SSG.
You should see the website for an explanation of the motivation
and the parts used to put this together.

## Anatomy of this repository

Here's a rundown of all the files that are in here and what they do:

| File or directory                                              | Type of files   | What they're for                                                               |
| -------------------------------------------------------------- | --------------- | ------------------------------------------------------------------------------ |
| [`content/*.md`](content/)                                     | Markdown pages  | The actual text of all the pages                                               |
| [`sass/*.scss`](sass/)                                         | SCSS            | Generates stylesheets. See below.                                              |
| [`static/`](static/)                                           | Static files    | Files copied as-is; images like <https://ssg.evanchen.cc/d2/bigpicture.svg>.   |
| [`templates/*.html`](templates/)                               | Tera templates  | Used by Zola to specify how HTML is built for each page. See below.            |
| [`zola.toml`](zola.toml)                                       | Zola config     | Options read by Zola                                                           |
| [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) | GitHub workflow | Used by GitHub Pages to set up deployment to `venhance.github.io`              |
| [`.gitignore`](.gitignore)                                     | gitignore       | Tells Git about files to ignore. No effect on deployed website.                |
| [README.md](README.md)                                         | README          | This file. Documentation for the contents here. No effect on deployed website. |
| [LICENSE](LICENSE)                                             | LICENSE         | [Open source license][osl] for using this code. No effect on deployed website. |

[osl]: https://en.wikipedia.org/wiki/Open-source_license

## Breakdown of the HTML and CSS

This repository has three templates read by Zola:

- [`layout.html`](templates/layout.html) (~60 lines of code):
  A base template with the top-level structure of the HTML page.
  Has two blocks `content` and `html_title` that are overridden.

- [`index.html`](templates/index.html) (~20 lines of code):
  Uses `layout.html` to render the index page, overriding `content`.
  Includes the table of contents.

- [`page.html`](templates/page.html) (~20 lines of code):
  Uses `layout.html` to render a normal page, overriding `content` and `html_title`.
  Includes page navigation.

In addition, it has three stylesheets for visual effects:

- [`sass/tlayout.scss`](sass/tlayout.scss) (~80 lines of code):
  Sets up the T-shaped page layout, margins, colors, and fonts.
  Also switches the layout on a smaller screen.

- [`sass/codeblocks.scss`](sass/codeblocks.scss) (~40 lines of code):
  Stylistic tweaks to make the inline code blocks look nicer.

- [`sass/images.scss`](sass/images.scss) (~10 lines of code):
  Styles the screenshots shown in the tutorial.

This code is intentionally really heavily commented so that beginners can read it.
(If I was writing for myself, I wouldn't bother with most of the comments.)

## Prek files

There are four more files that are used to configure some quality checks
on the source code here, called _pre-commit hooks_.
Since they don't actually change the website
(their only purpose is to define "mistakes" and catch them),
you can ignore them if you're a beginner:

- [`prek.toml`](prek.toml)
- [`djlint.toml`](djlint.toml)
- [`rumdl.toml`](rumdl.toml)
- [`.github/workflows/prek.yml`](.github/workflows/prek.yml)

If you're a beginner you also can just copy these blindly and use the same
rules that I (Evan) use for day-to-day work, if you want.
