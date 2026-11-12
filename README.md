# ssg.evanchen.cc

These are the source files for <https://ssg.evanchen.cc>,
a short guide on how to set up a website using an SSG.
You should see the website for an explanation of the motivation
and the parts used to put this together.

## Anatomy of this repository

Here's a rundown of all the files that are in here and what they do:

| File or directory                                              | Type of files   | What they're for                                                                    |
| -------------------------------------------------------------- | --------------- | ----------------------------------------------------------------------------------- |
| [`content/*.md`](content/)                                     | Markdown pages  | The actual text of all the pages                                                    |
| [`sass/tlayout.scss`](sass/tlayout.scss)                       | SCSS            | Generates <https://ssg.evanchen.cc/tlayout.css>, used to style the site. See below. |
| [`static/favicon.ico`](static/favicon.ico)                     | Icon            | Copied directly to <https://ssg.evanchen.cc/favicon.ico>, website icon              |
| [`templates/*.html`](templates/)                               | Tera templates  | Used by Zola to specify how HTML is built for each page. See below.                 |
| [`zola.toml`](zola.toml)                                       | Zola config     | Options read by Zola                                                                |
| [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) | Github workflow | Used by GitHub to tell how to deploy the website to `venhance.github.io`            |
| [README.md](README.md)                                         | README          | This file. Documentation for the contents here. No effect on deployed website.      |
| [LICENSE](LICENSE)                                             | LICENSE         | [Open source license][osl] for using this code. No effect on deployed website.      |

[osl]: https://en.wikipedia.org/wiki/Open-source_license

## Breakdown of the HTML and CSS

This repository has three templates read by Zola:

- [`layout.html`](templates/layout.html) (~60 lines of code):
  A base template with the top-level structure of the HTML page.
  Has two blocks `content` and `html_title` that are overridden.

- [`index.html`](templates/index.html) (4 lines of code):
  Uses `layout.html` to render the index page, overriding `content`.

- [`page.html`](templates/page.html) (7 lines of code):
  Uses `layout.html` to render a normal page, overriding `content` and `title`.

In addition, it has a single stylesheet:

- [`sass/tlayout.scss`](sass/tlayout.scss) (~70 lines of code):
  Sets up the T-shaped page layout, margins, colors, and fonts.
  Also switches the layout on a smaller screen.

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
