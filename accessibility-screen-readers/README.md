# Making ai2html accessible to screen readers

While [ai2html supports `alt` text](https://github.com/newsdev/ai2html/issues/94) (you can use `image_alt_text` to your settings block), it's actually pretty tragic for a screen reader. It'll read the alt text, then all the random labels all over your graphic! Not ideal.

## Our technique

Instead, we're going to make use of a common pattern:

1. Add some text to the page that describes the graphic
2. Hide the text offscreen so a visual browser won't see it
3. Hide the graphic from the screen reader

That way the screen reader will only see the text, not the graphic. This is the same way [DataWrapper](https://www.datawrapper.de/) does it so it can't be that bad!

## How we do it

Typically you just go straight from your HTML right into your ai2html.

```html
<p>This is some text</p>
<p>Here's more story text</p>

<!-- This is where you paste your ai2html -->

<p>Going back to the story text</p>
```

Instead, we're going to add a little more structure to hide the [well-written graphic description](https://medium.com/nightingale/writing-alt-text-for-data-visualization-2a218ef43f81) and hide the ai2html block from screen readers.

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

## Why it works

### The `sr-only` class

The `.sr-only` rule positions the text offscreen in a way that a visual browser cannot see it. If you'd like something a little more intense, [here's a gist that provides a lot more support to edge cases](https://gist.github.com/ffoodd/000b59f431e3e64e4ce1a24d5bb36034). Even though it's hidden from a visual browser, though, a screen reader will still read it.

### The `aria-hidden` attribute

When you add `aria-hidden=true` to a block, it [hides it from accessibility tools](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-hidden). This is often used for "close" buttons marked with an ✖ or other items that wouldn't be useful as read text.

In this case when we put our ai2html code inside of the `aria-hidden` block, the screen reader skips it entirely.
