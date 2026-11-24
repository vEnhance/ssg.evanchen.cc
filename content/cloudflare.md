+++
title = "§C. Cloudflare"
weight = 90
+++

## Connecting Cloudflare

In [§7 on GitHub Pages](@/github.md), we wired up an action that meant
GitHub would host our compiled output at `venhance.github.io`.
This section covers how to use a customized domain like `ssg.evanchen.cc` instead.

### §C.1. Purchasing a domain

The first thing you have to do is purchase a domain name.
My recommended vendor for that is Cloudflare, which does so at-cost.
You should register an account at <https://cloudflare.com>
and then follow the obvious instructions
to purchase a domain, which should be self-explanatory.

Unlike all the other parts of this tutorial (which used free technology),
you do have to actually pay money here; there is no way around this.
There is some good news and bad news.

- The good news is that you only pay for the apex domain
  and can have many subdomains `*.example.com`.
  For example, in real life,
  [web](https://web.evanchen.cc), [ssg](https://ssg.evanchen.cc),
  [blog](https://blog.evanchen.cc), [wall](https://wall.evanchen.cc),
  and [list](https://list.evanchen.cc) are five different unrelated projects,
  but I only pay annually for `evanchen.cc` itself.
- The bad news is that you can probably only _rent_ a domain.
  You usually can't permanently own a domain;
  instead, you have to keep paying annual costs to renew it.

But anyway, picking a domain, right.
Besides the classical `.com` and `.org`, there are a lot of new top-level domains
out there that you can register for these days:

- Generic domains include `.cc`, `.page`, `.site`, `.xyz`, etc.
- Meanwhile, `.dev`, `.io`, `.sh` seem to be trendy among programmers.
  (I would totally have taken `.dev` if it had been available,
  but `.dev` only became purchasable in 2019; I registered my site in 2017.)
- And then there's `.lol`, `.meme`, `.wtf` too now, apparently.
  Don't ask me why.

In this tutorial we'll assume you purchased `example.com`.

### §C.2. Pick the apex or subdomain, once and for all

Now, at this point, you have to make up your mind on something:
what part of `example.com` do you want your project to live under?
There are three kinds of choices:

- The apex domain `https://example.com`.
- The www subdomain `https://www.example.com`.
- Another subdomain like `https://blog.example.com`, say.

The first two options are, from a technical standpoint,
actually completely unrelated — there isn't anything special about `www`.
If you're deciding between apex and `www`,
you should firmly pick one now and never change it.
These days, in 2026, apex seems to be more popular.
In that case, you may want to also
[make www just redirect to the apex][cf-www].

(For some reason, when setting up my site I decided to make both the apex
and `www` redirect to [web.evanchen.cc](https://web.evanchen.cc).
This is not standard.
I don't remember why I decided on this, but I'm stuck with it forever now.)

Once you finally choose,
double-check that your `base_url` in `zola.toml` has the choice you decided on.
(You were required to choose a value with `zola init` back in [§5 on Zola](@/zola.md).)
If this value is out-of-date, update it, commit, and push.

```toml,linenos,name=zola.toml,hl_lines=2
# The URL the site will be built for
base_url = "https://example.com"
```

(Optionally, you might also want to rename your GitHub repository at this point
to match the domain name, as I did with
[ssg.evanchen.cc](https://github.com/vEnhance/ssg.evanchen.cc).
I think my life became easier once I decided that every repository,
if it corresponded to a live website,
must be named exactly the same as the website URL.
Helped me with organizing my head to have only one possible name.)

[cf-www]: https://developers.cloudflare.com/pages/how-to/www-redirect/

### §C.3. Updating the DNS

With the domain purchased, Cloudflare gives you access to its
[DNS records](https://www.cloudflare.com/learning/dns/dns-records/).
You can have many of these, which do a lot of different things.
For us, we just want a single `CNAME` record to start.

Hit whatever button says "Add record", and choose CNAME for the type.
The target should be `venhance.github.io`, and:

- For an apex domain, pick name `@`.
  (This is a [nice Cloudflare feature](https://developers.cloudflare.com/dns/cname-flattening/)
  that makes this work here; not all DNS providers allow this!).
- For a subdomain like `www` or `blog`, use that for the name.

Finally, when you add this record, Cloudflare will ask whether the proxy status should be
"Proxied" (orange cloud) or "DNS only" (gray cloud).
**You typically actually want the DNS only (gray cloud)**, at least at first.
(The orange cloud can support some extra features by making
everything go through Cloudflare,
but last I checked they don't play well with GitHub Pages,
and can cause issues with later steps in the process.)

As long as you're here,
you should also add a GitHub Pages challenge TXT record.
[See instructions here for this verification process][gh-docs-verify].
(Note that this is _Account Settings_ rather than repository settings.)
This is a security measure that ensures that `example.com` belongs only to you,
so another GitHub user can't hijack your domain.

[gh-docs-verify]: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages

Officially, people have always told me DNS changes can take up to 24 hours.
But I've never had to wait more than a few minutes before.
Still, it may take a while for these changes to take effect.

### §C.4. Finishing up

Finally, go back to your GitHub repository, go to "Pages"
type your domain into the "Custom domain" box, and hit "Save".
If the DNS is working (and again, you may have to wait a bit),
you'll get a green check mark.
You should also make sure "Enforce HTTPS" is on.

That's it!
