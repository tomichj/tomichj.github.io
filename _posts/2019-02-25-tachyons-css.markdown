---
layout: post
title:  "Tachyons CSS"
date:   2019-02-25 16:35:00 -0800
hero: /images/th.jpg
categories: blog
---

[Tachyons.io] is a small, readable css toolkit that packs a huge amount of functionality into a tiny framework. I've been aware of tachyons for a couple of years, but never quite got around to using it. But now that I've started sing tachyons.io, I love it, and it's changed the way I think about CSS. I'll explain below what Tachyons does that's different, and why I think this approach has value.

### Typical CSS

Most of us write CSS by defining a class and then redefining that class with media queries to redefine the class at different break points. Like so:

```css
h1 { ... }
.some-section { ... }
.another-section { ... }

@media (min-width:640px) {
  .some-section { ... }
  .another-section { ... }
}
```

Typically you'll change your margins, padding, and font-sizes repeatedly for different breakpoints. This is common, and it becomes increasingly difficult to keep track of different sizes and breakpoints as your CSS grows. You work hard to apply your sizing notions consistently across breakpoints, and the maintenance burden grows exponentially as your application grows in complexity.

### The tachyons methodology




[https://tachyons.io/](https://tachyons.io/)

[tachyons.io]: https://tachyons.io/
