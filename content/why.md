+++
title = "Why SSG?"
weight = 5
+++

## Why static sites?

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

**You shouldn't pay for complexity that you don't need**.
If you are going to build a big multi-user application or an online marketplace,
then yeah, get a server.
But most people's first website shouldn't be something that complicated.
If you just want a personal blog, or a website for your small business,
then there's a good chance all you need to do is put some text on the Internet.
In that case, you can use a static site.

Besides, if you are reading this guide, you're probably new to this kind of thing.
In that case, I think your learning will go much better
starting from an empty repository with parts added in one at a time,
so you can understand every step of the process.
