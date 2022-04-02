## Adjustment: side-by-side scrollytelling

For **side-by-side scrollytelling**, add the following to the bottom of your style block, and then change your `<div id="scrolly">` to be `<div id="scrolly" class="side-by-side">`.

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
        order: 1;
        flex-grow: 2;
    }
}
```