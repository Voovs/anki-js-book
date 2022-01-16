# Part 1 - Querying the DOM for Fields

In Japanese there are two ways to write the same word. There's the "spelling"
which we have in our `{{ Kana }}` field and the written-form which is in our `{{
Kanji }}` field. However, many common and rare words are usually written in
their "spelling" form. In other words `{{ Kanji }}` is an empty string. If we
try deleting the input from `{{ Kanji }}` the resulting card looks pretty
unbalanced

<!--TODO: ![Card without kanji](images/rust-logo-blk.svg) -->

The conventional static solution to this would be making a second, nearly
identical, card type that has only one field `{{ Kana }}` and styles itself
differently. As you may suspect, keeping track of two cards is unnecessarily
laborious for this. Let's do it in JS!

We'll start by selecting all the relevant variables

```javascript
let kana = document.querySelector("#kana-kanji > .kana");
let sep = document.querySelector("#kana-kanji > .separator");
let kanji = document.querySelector("#kana-kanji > .kanji");
```

We're being a little more verbose than necessary by specifying the `#kana-kanji`
tag, though it's best to specify paths with an ID somewhere, since ID tags
should be unique in the document

Next we need to check if the `{{ Kanji }}` field is empty. It follows our
`kanji` element will have no text inside it, so we'll check for that

```javascript
if (kanji.textContent === "") {
    // {{ Kanji }} field is blank!
}
```

We can now properly handle this case. Here, we'll just delete the other two
elements, so that `kana` will be the only remaining element


```javascript
sep.remove();
kanji.remove();
```

That's perfect. We'll use some [proper coding habits](/anki_js/coding_style.md)
to finish things up. Your final code should look like:

```html
<!-- Center Kana when no Kanji is present -->
<script>
    (function centerKana() {
        let kana = document.querySelector("#kana-kanji > .kana");
        let sep = document.querySelector("#kana-kanji > .separator");
        let kanji = document.querySelector("#kana-kanji > .kanji");

        if (kanji.textContent === "") {
            sep.remove();
            kanji.remove();
        }
    })();
</script>
```

As an exercise, try extending this code to handle the opposite case, where `{{
Kana }}` is blank and `{{ Kanji }}` has text. See the [solutions
section](solutions_1.html#exercise-1---centering-kanji) to verify

# Part 2 - Querying Fields Directly

The situation above was rather idealistic, as each field had its own
corresponding element in the DOM. We can still deal with situations where fields
are mixes with one another

To start, replace the code of the Back Template to match this

```html
{{ FrontSide }}

<div id="back">
    <hr class="breaking-line">

    <div id="kana-kanji" class="plain-vocab japanese">
        {{ Kana }}　ー　{{ Kanji }}
    </div>
</div>
```

As you can see, there's no way to check if `{{ Kanji }}` is empty directly here.
The `#kana-kanji` element is guaranteed to be non-empty, since that `　ー　` is
always there. Instead we'll query the fields directly

```javascript
let kana_field = `{{ Kana }}`;
let kanji_field = `{{ Kanji }}`;
```

This is [less preferable](/anki_js/advanced.md#template-strings) than the
solution is part 1, so use that one if you can, though here it's not going to
cause any issues

Now we can use a similar solution as part 1 for the logic

```javascript
if (kanji_field === "") {
    // {{ Kanji }} field is blank!
}
```

To handle this, we can't simply remove the element. Instead we'll change the
interior text content to be only our `{{ Kana }}` string

```javascript
let kana_kanji_el = document.querySelector("#kana-kanji");
kana_kanji_el.textContent = kana_field;
```

That's it. Your final code should look like:

```html
<!-- Center Kana when no Kanji is present -->
<script>
    (function centerKana() {
        let kana_field = `{{ Kana }}`;
        let kanji_field = `{{ Kanji }}`;
        let kana_kanji_el = document.querySelector("#kana-kanji");

        if (kanji_field === "") {
            kana_kanji_el.textContent = kana_field;
        }
    })();
</script>
```

Again, you can try extending the above for the case where `{{ Kana }}` is empty
and `{{ Kanji }}` is present. As a more challenging exercise, try turning the
text for `{{ Kana }}` red when it's the only field being displayed. [See
solutions here](solutions_1.html#exercise-2---centering-kanji)

<strong>Hint:</strong> for the second exercise, you should use the Styling pane
as well! The color definition for red is in the `--red` variable
