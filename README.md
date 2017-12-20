# Hello.

I'm Richard Soderberg, and this is my training content repository.

## Content:

### DNS tutorial

Early in 2017, one of our certificate issuers began enforcing `CAA` records.
They required a valid response, and for arcane and subtle reasons we were not
providing such on a specific single subdomain. We were able to resolve the
issue, but the intracacies of the issue (`SERVFAIL` vs. `NXDOMAIN`) exposed a
need for further training. This tutorial was presented in story form
paralleling the document, with a short segment of time with an editor open on a
BIND zone file to explain various nuances.

Link: [DNS tutorial](dns/dns.md) ([PDF](dns/dns.pdf))

### TLS tutorial

WebOps issues a wide variety of SSL certificates. We felt it appropriate to do
a top-to-bottom understanding of TLS from an operational standpoint. Most
tutorials focus on details not strictly necessary for everyday ops work, so I
wrote one that instead can be used to introduce just those aspects of TLS
necessary to improve general understanding.

Link: [TLS tutorial](tls/tls.md)

### CSP tutorial

I attempted to teach CSP during a one-hour time window during an on-site week
for WebOps. Unfortunately I misunderstood the time available and ended up
overlapping the training with our brief lunch. It was a failure, and so I
haven't had an opportunity to present this content in full to an audience.
The tutorial may still be valuable someday, so it's archived here just in case.

Link: [CSP tutorial](csp/csp.md)

After several other tutorials, I realized that perhaps the approach used by the
other training sessions was not truly appropriate for CSP. So when we repeated
the session later in the year, I approached it differently. Instead of writing
a complete history and description of CSP, I used a simple summary guide, the
Laboratory extension, and ninety minutes of Firefox on a shared screen to
define a security policy for a single simple site. I hope it was well-received.

Links:

* [Foundeo's CSP Reference](https://content-security-policy.com/)
* [Mozilla's Laboratory Extension](https://addons.mozilla.org/en-US/firefox/addon/laboratory-by-mozilla/)

## FAQ:

### What is the audience for these tutorials?

As of December 2017, these are generally targeted for the WebOps team at Mozilla.

### What does that mean?

"Systems Administrators", with a special focus on websites and SaaS providers.

### You made a technical/factual/typo error.

I'm sorry. Please open an issue and/or pull request for me to consider merging.

### I disagree with your approach/style/judgement.

I'm happy to consider other points of view. Please open an issue so we can discuss.

### You shouldn't use the CC-BY-SA license.

This content is "website content", which per our [licensing policies] is generally CC-BY-SA.

[licensing policies]: https://www.mozilla.org/en-US/foundation/licensing/

### Some of these documents use smart quotes!

Indeed. Smart quotes are restricted to body copy. Please open an issue if you find any in code.
