+++
title = "FAQ"
weight = 90
+++

## FAQ

My defense of some decisions made in writing this tutorial.

### FAQ 1. What motivated this tutorial??

I had the idea on [post #364 on my microblog](https://wall.evanchen.cc/364/).
After running this a few times I decided to actually write a full tutorial.

### FAQ 2. Why static sites?

I like how Loris Cro describes it in the post
[The Static Site Paradox](https://kristoff.it/blog/static-site-paradox/):

> In front of you are two personal websites, each used as a blog and to display
> basic contact info of the owner:
>
> 1. One is a complex CMS written in PHP that requires a web server, multiple
>    workers, a Redis cache, and a SQL database. The site also has a big frontend
>    component that loads as a Single Page Application and then performs
>    navigation by requesting the content in JSON form, which then gets
>    "rehydrated" client-side.
> 2. The other is a collection of static HTML files and one or two CSS
>    files. No JavaScript anywhere.
>
> If you didn't know any better, you would expect almost all normal users to have
> [2] and professional engineers to have something like [1], but it's actually the
> inverse: only few professional software engineers can "afford" to have the
> second option as their personal website, and almost all normal users are stuck
> with overcomplicated solutions.

_You shouldn't pay for complexity that you don't need_.
If you are going to build a big multi-user application or an online marketplace,
then yeah, get a server.
But most people's first website shouldn't be something that complicated.
If you just want a personal blog, or a website for your small business,
then there's a good chance all you need to do is put some text on the Internet.
In that case, you can use a static site.

Besides, a lot of readers are probably new to this kind of thing.
In that case, I think your learning will go much better
starting from an empty repository with parts added in one at a time,
so you can understand every step of the process.

### FAQ 3. Why teach caveman technology in 2026? Why not an IDE?

Because most of the people who read my stuff are math students,
and **math students learn bottom-up, not top-down**.
That is: _start_ from atomic principles,
_then_ learn how to compose them or add abstraction layers.

For example, in this tutorial, I make the student edit files in Vim,
then do Git operations like add and commit in the command line.
That's because I want the student to understand that any text file
can be edited by any editor, and also Git isn't tied to any particular editor.
_Then_, when they are using VSCode three months later,
and VSCode is doing Git operations with mouse clicks on some giant menu,
they understand how VSCode is automating the operations they _used_ to do by hand,
because they actually typed `git commit` by hand before.

If you do this the other way around where you start the beginner in VSCode,
these poor souls don't get any chance to form first principles.
They just see a monolith that does everything from
editing to linting to committing to pushing.
I've seen the same thing with [Overleaf](https://overleaf.com)
which is so idiot-friendly that it also
[killed conceptual understanding of LaTeX](https://blog.evanchen.cc/auto-carrot/).
The following is a real question I got from such a victim:

> I'm confused, why do people use GitHub? Why not just use Overleaf?

### FAQ 4. AI??

¯\\\_(ツ)\_/¯
