## Adjustment: side-by-side scrollytelling

For **side-by-side scrollytelling**, you need to do one (or maybe two) steps.

First, find the line that that is the start of your scroller...

```html
<div id="scrolly">
```

...and add a class of `side-by-side` to it:

```html
<div id="scrolly" class="side-by-side">
```

**Refresh and see if it works!**

If you're using an older template that doesn't already have the rules for `side-by-side` you will need to add them. Scroll up to your style block and add the lines below, which will only affect the scroller if it has the `side-by-side` class.

```css
/* When it's less than 700 pixels wide, do normal scrollytelling */
@media only screen and (min-width: 700px) {
    #scrolly.side-by-side {
        display: flex;
    }

    #scrolly.side-by-side > div {
        flex-basis: 0;
        flex-grow: 1;
        flex-shrink: 1;
        padding: 0.75em;
    }

    #scrolly.side-by-side .scrolly-overlay {
        margin-top: 70vh;
        order: 0;
        /* Change these to adjust sizing options for the text */
        min-width: 15rem;
        max-width: 20rem;
    }

    #scrolly.side-by-side .sticky-thing {
        /* Change order to 0 if you want the text on the right */
        order: 1;
        flex-grow: 2;
    }
}
```

Read the comments to see which parts you can adjust to change the display!