+++
title = "§5. Zola"
weight = 50
+++

## Using Zola

Now we can get to the part about actually setting up the site!
There is an ocean of different generators out there,
but the one we're going to use is [Zola](https://www.getzola.org/documentation/getting-started/overview/).

There are three types of files we'll use within Zola:

- **Markdown content**, where you write your prose
- **Tera HTML templates**, for specifying an HTML layout
- **Sass stylesheets**, for styling the HTML.

We'll handle the first two now and then move on to styling on the next page.

### Getting `zola serve` running

To kick things off, in `~/mysite`, run `zola init -f`,
type in the target website name
(if you haven't decided on a domain name yet,
change this later) and accept the defaults for the two questions.
This will create a bunch of empty folders and a `zola.toml`.
Then, finally type `zola serve`.

```console
$ pwd
/home/evan/mysite

$ ls
prek.toml

$ zola init -f
Welcome to Zola!
Please answer a few questions to get started quickly.
Any choices made can be changed by modifying the `zola.toml` file later.
> What is the URL of your site? (https://example.com): https://mysite.dev
> Do you want to enable Sass compilation? [Y/n]:
> Do you want to build a search index of the content? [y/N]:

Done! Your site was created in /home/evan/mysite

Get started by moving into the directory and using the built-in server: `zola serve`
Visit https://www.getzola.org for the full documentation.

$ ls
content  prek.toml  sass  static  templates  themes  zola.toml

$ zola serve
Building site...
-> Creating 0 pages (0 orphan) and 0 sections
Done in 52ms.

Listening for changes in /home/evan/mysite/{zola.toml,content,sass,static,templates}
Press Ctrl+C to stop

Web server is available at http://127.0.0.1:1111 (bound to 127.0.0.1:1111)
```

Now point your web browser to <http://127.0.0.1:1111>.
You should see a welcome page: you have an empty site running!
Zola's server does live reload, so you can leave this terminal running
and any changes you make will refresh automatically.

So leave this process running, and **open a new terminal** pointing at `~/mysite`,
where we'll continue to do our Git/Vim/etc. work.

Before going on, let's keep our habit of committing after every logical change.
We'll commit the newly added `zola.toml`.
Since all the folders are empty, we just commit `zola.toml` to Git.
(Git doesn't care about empty folders, only files.)

```console
$ git status
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
  zola.toml

nothing added to commit but untracked files present (use "git add" to track)

$ git add zola.toml

$ git commit -m "feat: new zola.toml"
check for merge conflicts............................Passed
check toml...........................................Passed
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main 3f418d2] feat: new zola.toml
  1 file changed, 16 insertions(+)
  create mode 100644 zola.toml
```

### Pages in `content/`

We'll start by creating an index page.
Zola uses [Markdown](https://commonmark.org/),
which is so easy to learn that you can probably
[just look at this repository for examples](https://github.com/vEnhance/ssg.evanchen.cc/tree/main/content)
and never need a manual.
(It's basically plain text with some self-explanatory embellishments.)
If you do want a cheat sheet anyway,
[here's the 60-second one](https://commonmark.org/help/).

To start populating our site, we're going to make three pages,
a landing page `_index.md`, an `about.md` and a `contact.md`.
These all go in `content/`.
The name `_index.md` is special to Zola.
Let's open up all three:

```console
$ vim content/_index.md content/about.md content/contact.md
3 files to edit
```

(In Vim, you can use `:n` and `:p` to move between files.)
We'll put together three files like so:

```markdown,linenos,name=content/_index.md
+++
title = "Home"
sort_by = "weight"
+++

This is the homepage!
```

```markdown,linenos,name=content/about.md
+++
title = "About"
weight = 10
+++

Wagahai wa neko de aru.
```

```markdown,linenos,name=content/contact.md
+++
title = "Contact"
weight = 20
+++

Call me! (323) 634–5667.
```

The `+++` header is a mandatory header section that Zola uses on each post.
We've created a top-level section sorted by a manually specified weight,
which we put inside each of the other posts.

Let's commit these changes.
We can use `git add content/` which will add the entire folder in, then commit it.

```console
$ git add content/

$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  new file:   content/_index.md
  new file:   content/about.md
  new file:   content/contact.md


$ git commit -m "feat: Three new pages"
check for merge conflicts............................Passed
check toml.......................(no files to check)Skipped
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main 4cf98d5] feat: Three new pages
  3 files changed, 18 insertions(+)
  create mode 100644 content/_index.md
  create mode 100644 content/about.md
  create mode 100644 content/contact.md
```

### Tera HTML templates in `templates/`

Zola uses the [Tera HTML template engine](https://www.getzola.org/documentation/templates/overview/).
These are files that look like HTML but have a bit of extra syntax;
it'll make more sense to look at some examples first.

Zola has three distinguished templates:

- `index.html`, for the homepage at top-level `_index.md`;
- `section.html`, for subdirectories of `content/` (we won't use this);
- `page.html`, for individual pages like About or Contact in our example.

However, we often actually want the homepage and the subpages to look pretty
much the same.
So we're going to define a basic template called `layout.html`
(the name `base.html` is common too, you can pick that too) as a foundation;
then we'll have `index.html` and `page.html` inherit it.
Let's go there:

```console
$ vim templates/layout.html templates/index.html templates/page.html
```

Here's a `layout.html` to start with:

```html-derivative,linenos,name=templates/layout.html
<!DOCTYPE html>
<html lang="{{ lang }}">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <title>{% block html_title %}MySite{% endblock html_title %}</title>
    </head>
    <body>
        <div id="wrapper">
            <header id="layout-header">
                <h1><a href="/">My Website</a></h1>
            </header>
            <aside id="layout-nav">
                {% set root = get_section(path="_index.md") %}
                {% for p in root.pages %}
                    <a href="{{ p.permalink }}">{{ p.title }}</a>
                {% endfor %}
            </aside>
            <article id="layout-content">
                {% block content %}
                {% endblock content %}
            </article>
        </div>
    </body>
</html>
```

Most of this is just boilerplate HTML.
(The two `meta` lines in particular should just be always included on any page
you write, in my opinion.)
The parts special to Tera are the things in curly braces.
Basically, `{% … %}` is an actual command,
while `{{ … }}` is an expression that prints a value.
There are two things Tera is doing here:

- First, there are two `block` regions, with user-chosen names
  `html_title` and `content`.
  These `block` regions can be overridden by child templates, as we'll see soon.
- The second is the navigation thing; this is a bit of Zola's black magic.
  Here `get_section` is a predefined function that looks up where `_index.md` is,
  then we can `for`-loop over pages, outputting the link and the title.

The templating language is pretty powerful.
I won't dig into it during this tutorial,
just using a few snippets I need.

With this layout, designing `index.html` and `page.html` is easy:

```jinja,linenos,name=templates/index.html
{% extends "layout.html" %}
{% block content %}
    {{ section.content|safe }}
{% endblock content %}
```

```jinja,linenos,name=templates/page.html
{% extends "layout.html" %}
{% block html_title %}
    {{ page.title }} | MySite
{% endblock html_title %}
{% block content %}
    {{ page.content|safe }}
{% endblock content %}
```

In `index.html` and `page.html`,
Zola automatically provides the `section` and `page` context variables.
The `safe` filter is necessary in order to prevent HTML-escaping.

At this point, if you look in the browser again,
you should have a little personal page!
It's real!
No styling yet, though.

![Raw HTML website is alive. CSS forthcoming.](/screenshots/basehtml.webp)

Let's commit these changes.
Here (and this is subjective) I would argue that the `layout.html`
is a single change, and the two child templates are another one,
so I'm going to do this in two commits.

```console
$ git add templates/layout.html

$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  new file:   templates/layout.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
  templates/index.html
  templates/page.html


$ git commit -m "feat: create base layout template"
check for merge conflicts............................Passed
check toml.......................(no files to check)Skipped
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main 4eb369f] feat: create base layout template
  1 file changed, 25 insertions(+)
  create mode 100644 templates/layout.html

$ git add templates/index.html templates/page.html

$ git commit -m "feat: create child templates"
check for merge conflicts............................Passed
check toml.......................(no files to check)Skipped
detect private key...................................Passed
fix end of files.....................................Passed
fix utf-8 byte order marker..........................Passed
mixed line ending....................................Passed
trim trailing whitespace.............................Passed
[main 7d44f4c] feat: create child templates
  2 files changed, 11 insertions(+)
  create mode 100644 templates/index.html
  create mode 100644 templates/page.html
```
