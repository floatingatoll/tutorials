# CSP training

Last updated: July 2017

## Constructing a CSP header

* The header name is `Content-Security-Policy` or `Content-Security-Policy-Report-Only`.

  `Content-Security-Policy: …`

* Each header contains one or more directives, separated by a semicolon-space `; `

  `CSP: …; directive; …`

* Some directives have values, started by colon space `: `, separated by a comma-space `, `

  `CSP: …; directive: value, value, value; …`

* Quoting is mandatory for special CSP keywords, but prohibited for user-defined values.

  `'self'`, `'unsafe-inline'`, `'unsafe-eval'`, `'nonce-12345'`, `'sha256-12345'`

## Types of CSP directives

* "Fetch" directives control which sites the browser may download page resources from.

  `default-src`, `font-src`, `img-src`, `script-src`, `style-src`

* "Navigation" directives control how the browser behaves with navigating between pages.

  e.g. `frame-ancestors`, `form-action`

* "Reporting" directives instruct the browser to report CSP errors to a reporting server.

  e.g. `report-uri`

* Other useful directives exist as well, but are not categorized:

  e.g. `require-sri-for`, `block-all-mixed-content`, `upgrade-insecure-requests`

## CSP keywords that can be used when specifying sources.

* The `'none'` keyword prohibits fetching resources from anywhere.

* The `'self'` keyword permits fetching resources from the current site: `proto://hostname:port/`

* `'unsafe-inline'`, `'unsafe-eval'` permit classical JS/CSS behaviors that are now frowned upon.

* `'nonce-1234'`, `'sha256-1234'` permit marking 'unsafe' JS/CSS as 'safe' through metadata.

## "Fetch" directives

* All fetch directives are named `something-src` and share the same syntax.

  `CSP: something-src: 'self' http://insecure.com https: data: ;`

* `default-src` defines the policy to use for other fetch directives not listed in the header.

  `CSP: default-src: 'self' ; img-src: 'self' https: ; script-src: 'none' ;`

* Fetch directives may include URI schemes, hostnames, paths, and CSP keywords.

  `CSP: default-src: *.wildcard.com/stuff/ https: 'self' 'nonce-123' http://xyz.com/ data: ;`

* There are a vast array of `*-src` fetch directives, each controlling different webpage elements.

  `child-` `connect-` `font-` `frame-` `img-` `manifest-` `media-` `object-` `script-` `style-` `worker-`

## "Navigation" directives

* All navigation directives use the same syntax as the `*-src` directives above.
* `'self'` or `'none'` are common, as offsite framing and form submissions aren't.
* `frame-ancestors` defines which sources may frame this page in the browser.
* `form-action` defines which sources may receive form submissions from the browser.

## "Reporting" directives

* CSP offers two reporting directives, `report-uri` and `report-to`.
* Support for these directives is inconsistent and does not include Firefox.
* A reporting server is required to receive report submissions from browsers.
* Reports will be submitted only if the browsers encounter CSP errors on a site.

## Other directives

* These directives are less common, but it's useful to know that they exist.

  There are more directives not listed here, such as `base-uri`.

* `upgrade-insecure-requests`, when specified, automatically replaces `http://` with `https://`.

  For all resources, not just those on your `'self'` site alone.

* `block-all-mixed-content`, when specified, prohibits `http://` resources on `https://` pages.

  For all resources, not just those on your `'self'` site alone.

* `require-sri-for` defines whether SRI is mandatory for JS and CSS.

  Unfortunately, no browser supports it today.

## Understanding the CSP `nonce-` and `hash-` (e.g. `sha256-`) keywords.

* Otherwise unsafe JS/CSS can be made "safe" by the inclusion of nonces and hashes.

  `CSP: script-src: 'self' 'nonce-12345' 'sha256-12345' ;`

* Nonces are randomized for each request, written into the CSP header and page HTML.

  Nonce is protected from JavaScript, so an attacker can't inject scripts after page load.

* Hashes are SHA2 checksums, written into the CSP header and tested against page HTML.

  Hashes can't be added to the CSP header after page load, preventing injection attacks.

* Use nonces when the inline JS/CSS content varies, hashes when the inline content is static.
