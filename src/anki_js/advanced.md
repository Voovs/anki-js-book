# Advanced edge cases

Where Anki gets... edgy

## Declaring variables in the global namespace

```html
<script>
    const options = {
        color: "red",
        font_family: "lovebeat",
    };

    function main() {
        // Doing things here
    }
</script>
```

In normal JS, this is a perfectly good way to create a pseudo-config file
that'll be very easy to change, particularly for others using your decks.
However, anki doesn't clear its memory when re-rendering a card. This means
`const options` will be redeclared in the same scope, causing an error, and stopping
the rest of the script from executing.

In short, don't pollute the global namespace. Here's a better version of the
above, that gets us the best of both worlds:

```html
<script>
    function main() {
        const options = {
            color: "red",
            font_family: "lovebeat",
        };
        // Doing things here
    }
</script>
```

## Template strings

This one is quite tricky. The way Anki processes our code makes for an unsafe
edge case when querying fields directly

Say we have a field called English-Sentence with the content `That's right!`.
Now let's see what happens when we try using a normal ES5 compatible JS string

```javascript
let my_field = '{{ English-Sentence }}';
```

Nothing wrong here so far. Now upon reading the double braces `{{}}` anki will
expand the field literally. It'll end up like this

```javascript
let my_field = 'That's right!';
```

Oh no, the syntax highlighting is broken. Since we had an apostrophe in the
sentence, JS will now interpret `let my_field = 'That'` then what follows is
syntax it can't parse, so JS will naturally error and not run the script

You CAN use any type of JS string, though `"` and especially `'` are very common
to see in regular text. Template strings use `\u0060` backticks, which are rare
to see as punctuation, so it's less likely to cause a conflict. Of course if you
do end up with a field containing backticks, JS will break in the same way

The simple solution is globally replacing the backticks in all fields with `'`
apostrophes. However if this isn't possible, or you want your code to be more
durable, we can use a work-around by actually inserting the field into the DOM
first

```html
<div id="font">
    <!-- Stuff that's displayed on the card -->
</div>

<span id="uncertain-field" style="display: none">{{ Uncertain-Field }}</span>

<script>
    let uncertain_field = document.querySelector("#uncertain-field").textContent;
</script>
```

This version of the solution should work with any type of punctuation, since JS
will wisely escape all types of quotation in the `textContent` string

## Literal HTML strings

When expanding fields, anki converts characters to be HTML-compatible. This
means symbols like `<` and `>` are converted away to their HTML interpretation.
It's pretty easy to convert them back

```javascript
let field = `{{ My-Field }}`.replaceAll("&lt;", "<").replaceAll("&gt;", ">");
```

This is especially important when the field contains literal HTML you want to
insert into the DOM. Say `<span>Hello</span>`

## Print debugging

In short, you can get anki to do print debugging with `console.log()`, though
setting it up is only recommended for users who are comfortable in a shell

 1. `git clone --depth=1 'https://github.com/ankitects/anki'`
 2. Make sure you don't have anki running right now, then `./run` in the root of
    the repository. This took about 10mins on a Mac Mini to compile for the first
    time. Recompilations after that will take much less time, more like 10s

Now `console.log()` will print to the terminal with `./run`. If you have plugins
installed, this printout may become a mess, though it's certainly going to be
there

`console.log("Hello World")` will produce a message that looks like

```
JS Info :4 Hello World
```

where `:4` indicates its the fourth line. Lines are counted from the start of
the current `<script></script>` tag, not the actual line in the template

Errors look similar, in this case the variable `svg_element` is not defined on
line 54:

```
JS error /_anki/legacyPageData?id=5309672608:54 Uncaught ReferenceError: svg_element is not defined
```

If you aren't comfortable setting this up, the best solution is to code
elsewhere. All browsers have a console that executes the same JS as anki. You
can usually access these by right clicking on a webpage and going into
"Inspect". You won't be able to access fields, though everything else should
work pretty much identically. On Chrome, you can start a blank tab to experiment
by [searching up `about:blank`](about:blank)
