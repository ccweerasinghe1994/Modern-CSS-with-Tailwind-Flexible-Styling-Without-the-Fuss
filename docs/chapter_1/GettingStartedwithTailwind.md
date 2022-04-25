- [Getting Started with Tailwind](#getting-started-with-tailwind)
  - [What the Tailwind CLI Does](#what-the-tailwind-cli-does)
  - [Using the Sample Code](#using-the-sample-code)
  - [Adding Tailwind to Your App](#adding-tailwind-to-your-app)
  - [Quick Start](#quick-start)
## Getting Started with Tailwind
Before going deep into `Tailwind`’s `utilities`, let’s take a quick tour to get a feel
for how Tailwind CSS works.

Tailwind is both a set of utility classes and a tool that `generates` `CSS` files
based on those classes. It also provides the `@apply` directive to allow you to
`compose` Tailwind `classes`. To get started with Tailwind, we need to install the
framework itself and then patch it into our `CSS processing tool chain`. First,
we’ll take a look at what the `Tailwind` `command-line interface` `(CLI)` does.
### What the Tailwind CLI Does
Tailwind, as a set of utility classes, provides a wide variety of `patterns` that
you can use to assign CSS classes to HTML elements in your code.

When you use Tailwind, you write CSS classes whose names match the `patterns` Tailwind defines. For example, you write <div class="m-4"> and Tailwind
defines that in CSS as .m-4 {margin: 1rem;}. Tailwind has the potential to define
`millions` of different `CSS classes` given that the patterns are both extensive
and can be `combined` with different `modifiers`, and can even in many cases
use arbitrary values. In fact, it’s `not` `feasible` to enumerate all the potential
classes Tailwind might define or use, and your project is likely to only use a
`small` `fraction` of those classes.

Defining all these potential classes—the vast majority of them `unused—and`
then sending them to the browser would be a huge `performance` `problem`. To
avoid that problem, Tailwind uses a `“just in time engine”` to detect the CSS
you are using and limit the amount of CSS that is defined, `generating` only
the CSS your `project` uses. A command-line interface (CLI) to that engine is
provided, and your front-end build tool can use that `CLI` to generate the `CSS`
needed for your `project`.

You provide Tailwind with a list of the files in your project that declare CSS
classes, Tailwind `scans` those files for text patterns that match `Tailwind`
`classes`, and then creates a CSS file that only contains the Tailwind classes
that are `actually used`.

The Tailwind command line is set up to run fast and err on the side of
including extra classes rather than try and guess whether the usage of, say,
m-4 is actually part of a CSS declaration. If the text m-4 is in the file anywhere,
Tailwind will add m-4 to the resulting CSS.

This is fine because Tailwind patterns are `odd` enough to only `rarely` occur
accidentally, and because including the `odd` extra CSS class is a `small price`
to pay for excluding `millions` of others while still including the `Tailwind`
classes you are actually using.
### Using the Sample Code
### Adding Tailwind to Your App
### Quick Start