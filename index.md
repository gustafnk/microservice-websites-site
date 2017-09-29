---
layout: default
---

Please give feedback and report issues on the [GitHub repository](https://github.com/gustafnk/microservice-websites-site/)

---

## HTTP/2

HTTP/2 is the enabler of building websites with autonomous teams, due to its multiplexing mechanism. We can use more JS and CSS references nowadays using HTTP/2. The following style of frontend architecture would not have possible with HTTP 1.1.

HTTP/2 implementations multiplex AJAX requests as well, making Client Side Includes more viable than before.

You need to respect that some users still use HTTP 1.1. However, they're not that many, they're getting fewer and fewer, and most of them are on desktop devices and have good network connections.

## Constraints
### Mobile performance

- Mobile performance is increasingly important...
- ...therefore, client-side JS diversity is *not* viable or wanted (regardless of Custom Elements, Web Assembly, AST-on-the-wire, etc)

### Autonomous web development teams (Scalable development)

- Autonomous teams are cross-functional
- Prefer verticals (self-contained systems) over horisontals
- Infrastructural services are ok, must not contain domain language
- It is free to choose technology for teams (and take responsibility for the choice)

<!-- - The web browser is infrastructure, HTML/CSS/JS is not infrastructure -->
<!-- - User journeys sometimes cross team boundaries and that's ok -->
<!-- - Feedback, quality, speed, mastery, autonomy, purpose -->
<!-- - The closer the UI the more specialised needs -->
<!-- - The ever-growing backlog of the centralised team -->
<!-- - The cycles of centralisation and decentralisation... But, of "what"? -->
<!-- - Multi-channels and native apps, where to split? (TODO)  -->

### Heterogenous system (Evolvability)

- Allowing for diversity of technology (including different versions of software) is critical for evolvability
- The architecture itself should not be a limit for diversity of technology. Instead, the limit should other organisational factors, like being able to quite easily move between teams, etc

<!-- - What's the business value of doing things differently? Innovation, standards, commodification, optimisations -->

## Transclude HTML fragments

### Pages + fragments + transclusion

- **Teams should produce pages and/or fragments**
- Pages may include fragments, using transclusion (either server-side or client-side)
- Pages own their own `<head>` and `<body>`
- The page is its own description of what fragments it includes
- **Use declarative transclusion techniques** (like [Edge-Side Includes](https://en.wikipedia.org/wiki/Edge_Side_Includes) and [hinclude](https://github.com/mnot/hinclude)/[h-include](https://github.com/gustafnk/h-include))

### *No shared libraries or frameworks in the client*

- ...because of coupling between teams, *Heterogenous system*, and *Mobile performance*
- Use a common base of JS polyfills and CSS resets. This should be regarded as infrastructure. Possibly have a small set of typography CSS rules as well.

### Fragment dependencies

- Fragments have a dependency on their CSS (and JS, optionally)
- For each fragment type, the consumer will need to include that fragment type's CSS and JS
- CSS and JS files are cached an infinitely long time and cache busted
- The fragment producer owns releases of the resources and thus the cache bust
- Because of this, the consumer must not depend on a specific cache busted version. The consumer must depend on files *without* cache busts in their names (controlled by the producer) that in turn refers to cache busted files.
- These files should be HTML and included by server-side transclusion, which will result in good performance (but they could also be JavaScript files and included by dynamic script loading, which will decrease user perceived performance)
- Include CSS in `<head>` and JS just before `</body>`
- The included JS should be small, so that the consuming pages are within budget

### Server-side vs client-side transclusion

- Prefer server-side transclusion over client-side transclusion, if you can
- There are a lot of trade-offs when choosing whether to transclude server-side or client-side: page load speed, visible in view port, server quality/performance, SEO, accessibility, no JavaScript, lazy loading
- You can use both techniques on the same page
- Make a release and iterate! It should be easy to change between server-side and client-side transclusion of fragments.

### Caching

- Cache fragments for a long time server-side (if they can actively be purged from the cache) and a short time client-side
- The result of server-side transclusions is cached server side (both assembled pages and fragments) with a short cache time
- In client-side transclusion, fragments resources are like any other resource, so use short client-side cache times
- Use client-side transclusion for personalized fragments, since pages containing personalised content are not cacheable server-side

## Maintaining the system

### Teams are responsible for their assembled pages

- Delegation of responsibility (chained). Compare with a weather service producing images that other pages use (regular client-side image transclusion using `<img src>`)
- Use a performance budget
- If a fragment is not good (operational, performance, etc), either it can no longer be used or it must immediately be fixed
- It's ok for a *page* to depend on a JS library, as long as the page is within budget and is following agreed policies around accessibility, etc
- It's not ok for a *fragment* to depend on a JS library

### Discovery of fragment types and fragment instances

- Teams should publish a catalog of their fragment type and fragment instances
- For each fragment type, associate one HTML resource for CSS and optionally on HTML resource for JS
- The catalog should be navigated with hypermedia controls (i.e. links)
- Optionally, catalogs of "slot types" and "slots" may also be published so that other systems know what is pluggable

### Styleguide

- Use a styleguide to communicate overall design
- Teams are encouraged to submit improvement suggestions to the styleguide and  suggestions for new components
