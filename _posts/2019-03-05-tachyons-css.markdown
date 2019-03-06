---
layout: post
title:  "Tachyons CSS"
date:   2019-03-05 17:37:00 -0800
hero: /images/th.jpg
categories: blog
---

[Tachyons.io] is a css toolkit that packs a huge amount of functionality into a tiny framework. I've been aware of tachyons for a couple of years, but never quite got around to using it. But now that I've started sing tachyons.io, I love it, and it's changed the way I think about CSS. I'll explain below what Tachyons does that's different, and why I think this approach has value.

### Normal CSS

Most of us write CSS by defining a class and then redefining that class with media queries to redefine the class at different break points. Like so:

```css
.some-section { padding: 2rem 0; }
.another-section { padding: 1rem 0; }

@media (min-width: 640px) {
  .some-section { padding: 4rem 0; }
  .another-section { padding: 2rem 0; }
}
```

Typically you'll change your margins, padding, and font-sizes repeatedly for different breakpoints. This is common, it's how I've written CSS for years. It has downsides: it becomes increasingly difficult to keep track of different sizes and breakpoints as your CSS grows. It's hard work to apply your sizing notions consistently across breakpoints, and the maintenance burden grows exponentially as your application grows in complexity.

### The tachyons methodology

Tachyons does things differently. Tachyons:
- defines classes for an individual or small sets of properties ("padding", "padding-left", etc) where possible
- uses very concise names, especially for often-repeated classes (margins, padding, etc)
- properties with many variations (padding, margins) get an integer added to the name
- __defines classes identically__ for each breakpoint

Tachyons base classes are __mobile by default__, and adds 3 additional breakpoints. Classes in a breakpoint have an abbreviation appended to their name:
- -ns 'not small', media query is `screen and (min-width: 30em)`
- -m 'medium', media query is `screen and (min-width: 30em) and (max-width: 60em)`
- -l 'large', media query is `screen and (min-width: 60em)`

So for any given class, there will be a default class, and also `-ns`, `-m,` and `-l` variations of the class for different screen sizes. 

Put all those rules together, and you end up with classes like:
- `pa4-l` padding all, 4th variant, for the "large" breakpoint
- `mb2-ns` margin botton, 2nd variant, for the "not small" breakpoint
- `br4` border radius, 4th variant, no breakpoint so it's applied in all viewport sizes

Take a quick glance at the [tachyons css] and you'll see all of these classes in their full glory.

#### An example: padding

Here's a (very) small sampling of tachyon's "padding all" classes.

Reminder: The classes and  properties will be defined with __identical sizes__ for each set breakpoint.

```css
.pa0 { padding: 0; }
.pa1 { padding: .25rem; }
.pa2 { padding: .5rem; }
.pa3 { padding: 1rem; }
.pa4 { padding: 2rem; }
(omiting remaining pa classes for brevity)...
```

And the same rules for the -ns, -m, and -l breakpoints:

```css
@media screen and (min-width: 30em) {
  .pa0-ns { padding: 0; }
  .pa1-ns { padding: .25rem; }
  .pa2-ns { padding: .5rem; }
  .pa3-ns { padding: 1rem; }
  .pa4-ns { padding: 2rem; }
}

@media screen and (min-width: 30em) and (max-width: 60em) {
  .pa0-m { padding: 0; }
  .pa1-m { padding: .25rem; }
  .pa2-m { padding: .5rem; }
  .pa3-m { padding: 1rem; }
  .pa4-m { padding: 2rem; }
}

@media screen and (min-width: 60em) {
  .pa0-l { padding: 0; }
  .pa1-l { padding: .25rem; }
  .pa2-l { padding: .5rem; }
  .pa3-l { padding: 1rem; }
  .pa4-l { padding: 2rem; }
}
```

The class names have the breakpoint abbreviation appended but are otherwise identical. This lets you apply the padding you want for each breakpoint without interfering with other breakpoints.

#### Applying padding for instant responsiveness

Let's put our padding example classes to use. Here's a simple h1:

```html
<h1 class="pa4-l pa3-m pa2">Hello!</h1>
```

This applies `2rem` padding for at 60em and wider viewports, `1rem` for 30em to 60em widths, `.5rem` of padding for everything smaller.

This technique has some real benefits. No more stumbling over hand-rolled CSS classes that might be out of alignment for a certain media query. If padding, or a font size, or a margin needs to be smaller or larger at a breakpoint, you just add a tachyons class. The downside: you use a lot of css classes in HTML elements. But you can instantly see what's happening with an element, and you can set up a responsive UI almost instantly.

### Reuse the technique

Even in projects that are not tachyon-based, you can still reuse the pattern. You can quickly factor all of the most repetitive parts of your CSS with this technique: margins, padding, font sizes. It's a great pattern, and one I hope to use more in the future.


[https://tachyons.io/](https://tachyons.io/)

[tachyons.io]: https://tachyons.io/
[tachyons css]: https://github.com/tachyons-css/tachyons/blob/master/css/tachyons.css
