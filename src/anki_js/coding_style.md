# Proper code styling

Given some of our scripts will only be a few lines in length it's tempting to
forget good coding practices and just write what works. While this won't appear
to be a problem at first, you'll regret it later

Writing properly styled code doesn't take much longer than writing a mess.
However, both the future you and others using your decks will waste plenty of
time trying to customize or debug poorly styled code

Even if you aren't authoring decks for others and never plan to edit the JS of a
deck after it's finished, anki updates have been changed how the HTML
interpreter works before which required fixing the card templates

### Key Guidelines

Here's a few things you should keep in mind, aside from the usual JS coding
guidelines:

 1. Use a new `<script></script>` tag for every "task". Script tags always go at
    the bottom of the Template. Place an HTML comment above every single script
    tag explaining what it currently does

    ```html
    <!-- Generate SVG for accent -->
    <script>
        // JS that generates an SVG for the accent
    </script>
    ```


 2. Always write functions, never declare variables in the global namespace. If
    it's too tempting, using an auto-executing function
    ```javascript
    (function main() {
        console.log("hello");
     })();
    ````

 3. Always use template literal string syntax. This resolves a lot of tricky
    bugs. See the [discussion here](advanced.md#template-strings) for more
    information

    ```javascript
    let my_string = `${hello_str} Word!`;
    ```

 4. Don't hard-code user-customizable values put them at the top of the
    highest-level function. This makes your scripts much more portable and nice
    to users who want to cusomize things

    ```javascript
    (function main() {
         let kana_element = document.querySelector("#kana");

         // More code down here
     }();
     ```

     For multiple values, use an object at the top instead. The name `g` or
     `global` is a good choice. See the point below for an example

 5. Don't hard code styles. Use the `class` attribute on any generated elements
    instead. Try to make the class names easily customizable with a global
    object

    ```javascript
    (function makeSvg() {
         const g = {
            svg_element_id: "svg_element",
            text_class: "svg-text",
            path_class: "svg-path",
         };

         // Code using `g` here
     }();
     ```

 6. Comment! It doesn't have to be everywhere, though at least at every
    top-level action explain what's going on. Too many comments can also be a
    bad thing, though it's pretty unusual to see that happen

### An Example

Here's an example of bad code that's tempting to write and will make you regret
things later:

```html
<script>
let el = document.querySelector("#English-Sentence");
if (el.textContent.includes("Hello"))
    el.innerHTML = `<strong>${el.textContent}</strong>`;
</script>
```

<span class="caption">An example of code you shouldn't write</span>

Here's the same example that's written cleanly and reusable. It does the exact
same thing as the above. Not only is it much more legible, extending this to
also bold `#Japanese-Sentence` with `おはよ` is so trivial it takes one line!

```html
<!-- Bold the English sentences that have "Hello" -->
<script>
    function boldSentenceWith(dom_selector, search_str) {
        let el = document.querySelector(dom_selector);

        if (el.textContent.includes(search_str)) {
            el.innerHTML = `<strong>${el.textContent}</strong>`;
        }
    }

    boldSentenceWith("#English-Sentence", "Hello");
</script>
```

Although it's not something you'll notice immediately, holding yourself to
strict style guidelines will make things much easier for you and anyone using
your scripts

If this still feels a bit confusing, all of the examples in the workshop adhere
to these guidelines. They'll become second nature pretty quickly

As a slight complaint, I've rarely encountered a pre-made anki deck that saw any
importance in clean code. Some of them have hundreds of lines of code, all of
which are completely illegible. This both wastes the time of users who want to
customize something and really annoying to fix when an anki update breaks the
deck. We'll be better than that
