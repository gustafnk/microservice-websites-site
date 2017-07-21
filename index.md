---
layout: default
---

# Micro&shy;service Websites

<em class="sub-heading">Building websites with multiple teams</em>

By Gustaf Nilsson Kotte ([@gustaf_nk](https://twitter.com/gustaf_nk/))<br/>
Please give feedback and report issues on the [GitHub repository](https://github.com/gustafnk/microservice-websites-site/)

---

# Microservice Websites

- Autonomous cross-functional teams
  - Free to choose technology, feedback (which leads to quality), speed, mastery, autonomy, purpose, responsibility
  - Prefer (self-contained) verticals over horisontals
  - Infrastructural services are ok, must not contain domain language
  - The web browser is infrastructure, the HTML/CSS/JS is not infrastructure
  - User journeys sometimes cross team boundaries and that's ok

  <!-- - The cycles of centralisation and decentralisation... But, of "what"? (TODO) -->
  <!-- - Multi-channels and native apps, where to split? (TODO)  -->

- HTTP/2 is the enabler of independent teams, due to its multiplexing mechanism
  - With HTTP/2, we can use more JS and CSS references, compared to HTTP 1.1
  - There's no longer any need of bundling resources between teams
  - This style of frontend architecture would not have possible with HTTP 1.1
  - HTTP/2 implementations multiplex AJAX requests as well, making Client Side Includes more viable than before
  - However, you need to respect that some users still use HTTP 1.1.
    But they're not that many, and they're getting fewer and fewer, and most of them are on desktop devices and have good network connections.

- The architecture should not be the limit for tech diversity
  - Instead, the limit should be the cognitive capacity of the organisation
  - Staff transfers between teams
  - What's the business value of doing things differently? Innovation, standards, commodification, optimisations

- Mobile performance is increasingly important, so client-side JS diversity not viable or wanted, no matter what "base" you have (Custom Elements, Web Assembly, AST-on-the-wire, etc)

- No shared libraries or frameworks in the frontend
  - ...because of coupling

- Use a common base of JS polyfills and CSS resets. This should be regarded as infrastructure. Possibly have a small set of typography CSS rules as well.

- Use a styleguide for communicating overall design. Communication goes both ways

- Teams may produce pages, fragments, or both
  - Pages may include fragments, using transclusion (either server-side or client-side)
  - Pages own their own <head> and <body>
  - The page is its own description of what fragments it includes
  - Use declarative transclusion techniques

- Teams are responsible for their assembled pages
  - Delegation of responsibily (chained)
  - Performance budget
  - If a fragment is not good (operational, performance, etc), either it can no longer be used or it must immediately be fixed
  - It's ok for a page to depend on a JS framework, as long as the page is within budget and is following agreed policies around accessibility, etc

- Fragments have a dependency on their CSS (and JS, optionally)
  - For each fragment type, the consumer will need to include that fragment type's CSS and JS
  - The fragment producer owns releases of the resources and thus the cache bust
  - Because of this, the consumer must not depend on a specific cache busted version. The consumer must depend on files *without* cache busts in their names (controlled by the producer) that in turn refers to cache busted files.
  - These files should be HTML and included by server-side transclusion, which will result in good performance. But they could also be JavaScripts and included by dynamic script loading, which will decrease user perceived performance.
  - Include CSS in <head> and JS just before </body>
  - The included JS should be small, so that the consuming pages are within budget

- Prefer server-side transclusion over client-side transclusion, if you can
  - There are a lot of trade-offs when choosing whether to transclude server-side or client-side: page load speed, visible in view port, server quality/performance, SEO, accessibility, no JavaScript, lazy loading
  - You can use both techniques on the same page
  - Make a release and iterate! It should be easy to change between server-side and client-side transclusion of fragments.

- Caching (TODO)
  - Server-side transclusion is cached server side (both assembled pages and fragments). Cache assembled pages a short time (both server-side and client-side) and cache the fragments for a long time, as long as they actively can be purged from the cache
  - Client-side transclusion works the same way, except that the assembled page never exists server-side.
  - Personalised content is not cacheable, be careful

- Discovery of fragment types and fragment instances (TODO)
