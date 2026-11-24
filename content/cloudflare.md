+++
title = "§C. Cloudflare"
weight = 90
+++

## Connecting Cloudflare

In [§7 on GitHub Pages](@/github.md), we wired up an action that meant
GitHub would host our compiled output at `venhance.github.io`.
What if you want your own domain like `ssg.evanchen.cc`, though?

Well, first you have to buy a domain,
and then configure what are called
[DNS records](https://www.cloudflare.com/learning/dns/dns-records/) for it.

### §C.1. Purchasing a domain

The first thing you have to do is purchase a domain name.
My recommended vendor for that is Cloudflare, which does so at-cost.

Note it's more accurate to say you _rent_ a domain rather than purchase it.
You usually can't permanently own a domain.

Besides the classical `.com` and `.org`, there are a lot of new domains
out there that you can register for these days,
like `.cc` or `.page` or `.site`.
Some more hip programmers seem to move towards `.io`, `.sh`, and `.dev`.
(And then there's `.lol`, `.meme`, `.wtf`.)
(Myself, I would totally have taken `evanchen.dev` if I still could,
but `.dev` wasn't open for public purchasing at the time I started my own site.)

### §C.2. Updating the DNS

### §C.3. Updating `zola.toml` and GitHub
