---
layout: default
---

# Micro&shy;service Websites

<em class="sub-heading">Building websites with autonomous teams</em>

Please give feedback and report issues on the [GitHub repository](https://github.com/gustafnk/microservice-websites-site/)

---

## Enabler: HTTP/2

  - HTTP/2 is the enabler of building websites with autonomous teams, due to its multiplexing mechanism
  - With HTTP/2 multiplexing, we can use more JS and CSS references, compared to HTTP 1.1
  - There's no longer any need of bundling resources between teams
  - This style of frontend architecture would not have possible with HTTP 1.1
  - HTTP/2 implementations multiplex AJAX requests as well, making Client Side Includes more viable than before
  - However, you need to respect that some users still use HTTP 1.1.
    But they're not that many, and they're getting fewer and fewer, and most of them are on desktop devices and have good network connections.

## Constraints

- Autonomous web development teams
  - Autonomous teams are cross-functional
  - Prefer verticals (self-contained systems) over horisontals
  - Infrastructural services are ok, must not contain domain language
  - Free to choose technology (and take responsability for the choice)
  - The web browser is infrastructure, the HTML/CSS/JS is not infrastructure
  - User journeys sometimes cross team boundaries and that's ok

  <!-- - Feedback, quality, speed, mastery, autonomy, purpose -->
  <!-- - The closer the UI the more specialised needs -->
  <!-- - The ever-growing backlog of the centralised team -->
  <!-- - The cycles of centralisation and decentralisation... But, of "what"? -->
  <!-- - Multi-channels and native apps, where to split? (TODO)  -->

- Heterogenous system
  - Allowing for diversity of technology (including different versions of software) is very valuable for evolvability
  - The architecture should not be a limit for diversity of technology
  - Instead, the limit should rather be the "cognitive capacity" of the organisation, i.e. being able to move between teams quite easily
  - What's the business value of doing things differently? Innovation, standards, commodification, optimisations

- Mobile performance
  - Mobile performance is increasingly important...
  - ...so client-side JS diversity is *not* viable or wanted (regardless of Custom Elements, Web Assembly, AST-on-the-wire, etc)

## Transclude HTML fragments

- Pages + fragments + transclusion
  - **Teams should produce pages and/or fragments**
  - Pages may include fragments, using transclusion (either server-side or client-side)
  - Pages own their own `<head>` and `<body>`
  - The page is its own description of what fragments it includes
  - **Use declarative transclusion techniques**

- **No shared libraries or frameworks in the client**
  - ...because of coupling between teams, *Heterogenous system*, and *Mobile performance*
  - Use a common base of JS polyfills and CSS resets. This should be regarded as infrastructure. Possibly have a small set of typography CSS rules as well.

- Fragments have a dependency on their CSS (and JS, optionally)
  - For each fragment type, the consumer will need to include that fragment type's CSS and JS
  - The fragment producer owns releases of the resources and thus the cache bust
  - Because of this, the consumer must not depend on a specific cache busted version. The consumer must depend on files *without* cache busts in their names (controlled by the producer) that in turn refers to cache busted files.
  - These files should be HTML and included by server-side transclusion, which will result in good performance. But they could also be JavaScripts and included by dynamic script loading, which will decrease user perceived performance.
  - Include CSS in `<head>` and JS just before `</body>`
  - The included JS should be small, so that the consuming pages are within budget

- Prefer server-side transclusion over client-side transclusion, if you can
  - There are a lot of trade-offs when choosing whether to transclude server-side or client-side: page load speed, visible in view port, server quality/performance, SEO, accessibility, no JavaScript, lazy loading
  - You can use both techniques on the same page
  - Make a release and iterate! It should be easy to change between server-side and client-side transclusion of fragments.

- Caching (TODO)
  - Server-side transclusion is cached server side (both assembled pages and fragments). Cache assembled pages a short time (both server-side and client-side) and cache the fragments for a long time server-side (if they can actively be purged from the cache)
  - Client-side transclusion works the same way, except that the assembled page never exists server-side. However, from a client-side perspective, fragments resources are like any other resource, so these can't be cached for a long time in the client
  - Personalised content is not cacheable, be careful

## Maintaining the system

- Teams are responsible for their assembled pages
  - Delegation of responsibily (chained). Compare with a weather service producing images that other pages use (regular client-side image transclusion using `<img src>`)
  - Performance budget
  - If a fragment is not good (operational, performance, etc), either it can no longer be used or it must immediately be fixed
  - It's ok for a page to depend on a JS framework, as long as the page is within budget and is following agreed policies around accessibility, etc
  - It's not ok for a fragment to depend on a JS framework

- Discovery of fragment types and fragment instances (TODO)

- Use a styleguide for communicating overall design. Communication goes both ways

---

Please give feedback and report issues on the [GitHub repository](https://github.com/gustafnk/microservice-websites-site/)

By Gustaf Nilsson Kotte ([@gustaf_nk](https://twitter.com/gustaf_nk/))<br/>