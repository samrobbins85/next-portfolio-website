---
categories:
  - web-dev
coders: []
date: 2020-11-29T00:00:00.000Z
description: Test contrasting colours against the WCAG standards
github:
  - "https://github.com/samrobbins85/colour-a11y-for-tailwind"
image: /img/portfolio_cover/colour.svg
site: "https://colour-a11y.vercel.app/"
title: Colour accessibility test for Tailwind CSS
tech:
  - logo: https://res.cloudinary.com/samrobbins/image/upload/v1597140224/tailwindcss_rnpshz.svg
    name: Tailwind CSS
    url: "https://tailwindcss.com/"
  - logo: https://res.cloudinary.com/samrobbins/image/upload/v1606660202/headless_f6rnbz.svg
    name: Headless UI
    url: "https://headlessui.dev/"
  - logo: https://res.cloudinary.com/samrobbins/image/upload/v1597140056/next-black_csivx6.svg
    name: Next.js
    url: "https://nextjs.org/"
---

I'm a big fan of Tailwind CSS, in part for it giving an already set up colour scheme. But one thing I've fallen foul of in the past is using the colours in a way that is accessible. The gradients are already set up in a way that gives you a good estimate as around 700+ on white will be accessible and around less than 500 on white.

However, this changes between the different colours, so I wanted to build a site which showed the accessibility scores for all the colours, making it easy to see which ones to use. This also provides the scope for testing the colours on a wide range of backgrounds.

The start of this was getting the `tailwindcss/colours` object, which provides hex values for all the colours, and this was as simple as

```js
const colors = require("tailwindcss/colors");
```

I could then map over this object, being careful to exclude black and white as they have just one value

```js
Object.keys(colors)
  .filter((color) => typeof colors[color] === "object")
  .map((color) => ())
```

And within this I could then use a similar method to map over the shades

I then used the npm package `wcag-contrast` which makes it easy to get a WCAG score from two colours like this

```js
score(hex(colour1, colour2));
```

And all these results were just displayed in a grid with varying columns for different viewport sizes. Setting a default width then using flex wrap may have given a better experience, but grid columns give a very reliable output.

Then came switching between black and white, for this I decided to try the new [headlessui](https://headlessui.dev/) library by Tailwind Labs. This made it easy to create a toggle for the black background and link this into React state. This could then be used to set the `dark` property on the outermost `div`, and then using the new Tailwind CSS dark theme, I could then change the text colours to ensure everything was visible. This also changed the colour with which the WCAG scores were being generated, so they were now correct against black.
