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

This book has a small amount of sample code associated with it. It’s delivered
as a set of static HTML files that read a `static` `CSS` file. The CSS file was
generated using the `Tailwind CLI`, and you can install that CLI locally if you
want to experiment.

You can `experiment` with the sample code as-is using the `standalone` version
of the `Tailwind CLI`, which does not require NodeJs to run. This may not be
the mechanism you use on a full front-end app, so take a look at the next
section as well.

To get started with the standalone Tailwind CLI, download the latest release
for your operating system at https://github.com/tailwindlabs/tailwindcss/releases and
place the file at the top level of book’s code directory, adjacent to the `tailwind.config.js` file.

You might want to `rename` that download to just `tailwindcss`. For me, that
command was mv tailwindcss-macos-arm64 tailwindcss. Your `command` will `vary` based
on what file you downloaded. You’ll also need to make the file executable with
something like chmod +x tailwindcss, again your operating system might vary.

With the `standalone` `CLI` in place, you can tinker with the existing code or
add new `HTML` files in the html directory. Then, when you run the CLI with
`npx tailwindcss -i ./css/input.css -o ./css/output.css --watch`, it will re-parse the HTML files and regenerate the
css/output.css file. Don’t worry, we’ll talk about everything that command is
doing later in the book.

### Adding Tailwind to Your App

The process of `installing` `Tailwind` to a front-end app depends on how your
project is managing `client-side assets`. As such, a complete guide to installing
Tailwind is outside the scope of this book and would quickly become outdated.
The `golden source` is the `Tailwind documentation` itself.2 Please check there
if you have difficulty installing Tailwind in your specific setup. This section
gives a general overview of how Tailwind is installed for most projects.

The Tailwind developers `recommend` that the easiest way to install Tailwind
for most projects is via the `NodeJS` version of the Tailwind command line tool.
Start by installing Tailwind itself:

```bash
$ npm install -D tailwindcss
```

Next, run the following command to create a Tailwind configuration file:

```bash
$ npx tailwind init
```

`tailwind.config.js`

```js
module.exports = {
  content: ['./html/*.html'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

You need to add one piece of information to the `configuration` file for the
Tailwind CLI to work. You need to `tell` `Tailwind` all the files that might use a
`CSS` class. The Tailwind CLI uses this information as input to the CSS generation process we talked about at the beginning of the chapter.

The information goes in the content property of the configuration, and uses
standard file matching syntax, where `*` matches `any text`, and `**` matches `any and all subdirectories`.

A standard `React` app might look like this, where Tailwind is directed to look
at `all` files in any `subdirectory` under `src` that end in `html, js, or jsx`.

```js
module.exports = {
  content: ['./src/**/*.{html,js,jsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

A basic `Ruby` on `Rails` setup might look like this, matching all view files with
`html.erb`, all helper files with `.rb` and all `JavaScript` files with `.js`.

```js
module.exports = {
  content: [
    './app/views/**/*.html.erb',
    './app/helpers/**/*.rb',
    './app/javascript/**/*.js',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

our application will likely have some slightly different setup, but the goal is
to have all files that might have `CSS` information passed to the `Tailwind CLI`.

If you’re using `Visual Studio Code`, the Tailwind extension uses the existence
of the configuration file to determine if the project uses Tailwind. Other integrated development environments (IDEs) and editors also have various plugins
and other forms of Tailwind support.

Finally, we need to add `Tailwind` to our `CSS files`. In general, you put the following lines in a CSS file that’s being imported. The exact location of the file
depends on your tooling, but we want it to look like this:

Here we’re `importing` `Tailwind` in `three` `layers`. The `base contains Tailwind’s reset classes`, `components` is a `small layer` containing `Tailwind’s component class`, and most of what I’ll be talking about in this book is in the `utilities` `layer`.
The layers become important as you customize Tailwind—`if you want to compose your classes with Tailwind modifiers, the classes need to be defined before the utilities layer`.

Other build systems might require you to use `@import` instead of `@tailwind` as
the command, check the official docs as a final source.

That should get you started. Let’s now see what Tailwind can do.

### Quick Start

We’ll quickly run through styling a hero segment for a sample page for a
concert series called NorthBy. The sample page in the code shows all the
versions one after the other. This is only a page in the public `HTML` of our
server app, so there’s no server-side information needed to explain this. (If
you’re running the sample code, the page should be visible by opening the
`intro.html` page in a browser.)
Here’s our first version:

```html
<h1>Welcome to NorthBy</h1>
```

You should see `no` `styling` applied to the text at all, not even the normal `size`
and `bold` styling you’d usually associate with an HTML `h1` tag. This is a good
test of whether Tailwind is installed. If you see any styling applied to the text,
then Tailwind isn’t loading and you should walk through the installation steps
again.

Let’s go back and forth between the `code` and the `view` to start adding features
here with `Tailwind`. I’m not going to explore the syntax or other options in
depth. All I want is to give a sense of what it’s like to work with `Tailwind` as
best as I can in a book format.

And, I have to add up front that I’m not a designer.

Here’s a first pass at getting a basic layout with text, subtext, and a logo:

```html
<div class="flex">
  <div>
    <img src="../media/music.svg" size="100*100" alt="" />
  </div>
  <div>
    <h1>Welcome to NorthBy</h1>
    <h2>A premium in sight and sound</h2>
    <button>Learn More</button>
  </div>
</div>
```

This code gives us the following result:

![](../img/1.png)

This isn’t too different from our first version. There’s still no styling applied
to the text, and there’s no spacing or anything.

Let’s add more changes. We can center the text, put a little bit of `distance`
between the two parts, `vertically` `center` the text against the logo, and put the
logo on the right. The Tailwind classes I’m using do a pretty good job of representing my intent:

```html
<div class="flex justify-center">
  <div class="mx-4 order-last">
    <img src="../media/music.svg" size="100*100" alt="" />
  </div>
  <div class="mx-4 self-center">
    <h1>Welcome to NorthBy</h1>
    <h2>A premium in sight and sound</h2>
    <button>Learn More</button>
  </div>
</div>
```

![](../img/2.png)

We’ve added `classes` here. The outer div now has two Tailwind classes: `flex justify-center`. The image has another two classes, `mx-4 order-last`, and the text block
also has `mx-4 self-center`. The `mx-4` classes specify `horizontal` `margin`, while the
rest of the classes all deal with layout using the `CSS flexbox` structure, which
we’ll look at later in `Flexbox`, on page 48.

Now, let’s go after that text. Let’s make the `header` big, the `subhead` less big,
and all the lines `centered`. And let’s give the whole thing a background:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>TailwindCode</title>
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <link rel="stylesheet" type="text/css" href="../css/output.css" />
  </head>

  <body>
    <!-- <h1>Welcome to NorthBy</h1> -->
    <div class="flex justify-center bg-gray-300">
      <div class="mx-4 order-last">
        <img src="../media/music.svg" size="100*100" alt="" />
      </div>
      <div class="mx-4 self-center text-center">
        <h1 class="text-6xl font-bold text-blue-700">Welcome to NorthBy</h1>
        <h2 class="text-3xl font-semibold text-blue-300">
          A premium in sight and sound
        </h2>
        <button>Learn More</button>
      </div>
    </div>
  </body>
</html>
```

This adds a new class to the outer div, `bg-gray-300`, which specifies a background
`color`. We’ve added a bunch of classes to the text elements, including a `textcenter` class surrounding them. The title element is now marked with `text-6xl`
`font-bold` `text-blue-700`, which specifies `text` `size`, `font` `weight`, and `color`. The subhead is smaller, less bold, and a lighter shade of blue: `text-3xl` `font-semibold` `textblue-300`.

![](../img/3.png)

Next, let’s make the `button` look more like a button, realign the image, and
while we’re at it, make the `image` `rounder`, too:

````html
<button
  class="my-4 px-4 py-2 border-2 border-black rounded-lg text-white bg-blue-900"
>
  Learn More
</button>
```
````

The image tag now has a class of `rounded-full`, which makes the whole thing
appear in a `circle` (admittedly quite a subtle effect on this image). The `button`
has grown a lot of `classes`: `my-4` `px-4` `py-2` specifies a vertical margin and horizontal and vertical padding; `border-2 border-black rounded-lg` specifies the `size`, `color`,
and `shape` of the `border`; and `text-white bg-blue-900` gives us the `text` and `background colors`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>TailwindCode</title>
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <link rel="stylesheet" type="text/css" href="../css/output.css" />
  </head>

  <body>
    <!-- <h1>Welcome to NorthBy</h1> -->
    <div class="flex justify-center bg-gray-300">
      <div class="mx-4 order-last self-center">
        <img
          src="../media/music.svg"
          size="100*100"
          alt=""
          class="rounded-full"
        />
      </div>
      <div class="mx-4 self-center text-center">
        <h1 class="text-6xl font-bold text-blue-700">Welcome to NorthBy</h1>
        <h2 class="text-3xl font-semibold text-blue-300">
          A premium in sight and sound
        </h2>
        <button
          class="my-4 px-4 py-2 border-2 border-black rounded-lg text-white bg-blue-900"
        >
          Learn More
        </button>
      </div>
    </div>
  </body>
</html>
```

![](./../img/4.png)
