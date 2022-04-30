# Making ai2html accessible to screen readers

While [ai2html supports `alt` text](https://github.com/newsdev/ai2html/issues/94) (you can use `image_alt_text` to your settings block), it's actually pretty tragic for a screen reader. The screen reader will read the `alt` text, then **also read all the bits of text on your graphic!**

You've never felt shame until you've watched a screen reader sound off every single tick on your axes. This isn't ideal, so let's fix it.

## Our accessible ai2html technique

Instead of using `alt` text, we're going to make use of a common accessibility pattern:

1. Hide the graphic from the screen reader so it won't read off all of our axis tick marks, city labels, etc.
2. Add some "normal" text to the page that describes the graphic
3. Hide the descriptive text offscreen so *only the screen reader will see it*

A screen reader will only see the descriptive text, not the graphic, while visual browsers will only see the graphic, not the descriptive text. This is the same way [DataWrapper](https://www.datawrapper.de/) does it so it can't be that bad!

## How we do it

Typically you'd just go straight from your HTML right into your ai2html.

```html
<p>This is some text</p>
<p>Here's more story text</p>

<!-- This is where you paste your ai2html -->

<p>Going back to the story text</p>
```

Instead, we're going to add additional structure to hide [our well-written text description](https://medium.com/nightingale/writing-alt-text-for-data-visualization-2a218ef43f81) from visual browsers and prevent screen readers from reading the ai2html code.

```html
<p>This is some text</p>
<p>Here's more story text</p>

<div class="sr-only">
    A description of the graphic
</div>

<div aria-hidden="true">
    <!-- This is where you paste your ai2html -->
</div>

<p>Going back to the story text</p>
```

You'll also need some add some CSS to support the `sr-only` class.

```css
.sr-only:not(:focus):not(:active) {
  clip: rect(0 0 0 0); 
  clip-path: inset(50%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap; 
  width: 1px;
}
```

If you're using a CSS framework it might automatically support a similar class. For example, it's [`.sr-only` in Bootstrap](https://getbootstrap.com/docs/4.3/getting-started/accessibility/#visually-hidden-content) and [`.is-sr-only` in Bulma](https://bulma.dev/classes/helpers/is-sr-only).

## Why it works

### The `sr-only` class

The `.sr-only` rule positions the text offscreen in a way that a visual browser cannot see it. If you'd like something a little more heavier-duty, [here's a gist that provides more support to edge cases](https://gist.github.com/ffoodd/000b59f431e3e64e4ce1a24d5bb36034). Even though the descriptive text is hidden from a visual browser, though, a screen reader will still read it.

### The `aria-hidden` attribute

When you add `aria-hidden=true` to a block, it [hides the block from accessibility tools](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-hidden). This is often used for "close" buttons marked with an ✖ or other items that wouldn't be useful as read text.

In this case when we put our ai2html code inside of the `aria-hidden` block, the screen reader skips it entirely.
