+++
title = "SSG Tutorial"
sort_by = "weight"
+++

Welcome!
This is a short tutorial on how to set up a website the old-fashioned way,
working mostly from a terminal, using not too many moving parts.
This is specifically for a *static website*
(like a personal page or small blog),
which means that you just need to put some content out,
without having to deal with a server.

## Jumping in

To jump in, go to [Getting Started](/overview).

Also, to be true to ourselves,
this website is published using the same system that it describes.
You can see the source repository at
[GitHub on vEnhance/ssg.evanchen.cc](https://github.com/vEnhance/ssg.evanchen.cc),
as an example that you can follow.

## Motivation

Why would you want to use simple barebones technology
when there are lots of website builders?
The [Why SSG?](/why) page has a longer explanation,
but I like how Loris Cro describes it in the post
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
