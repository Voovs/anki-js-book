# Basic Javascript in Anki

## Interacting with the DOM

In HTML, the Document Object Model is just all those tag elements that appear in
the page. These are what's actually displayed on the card. For example, here's
an element that prints `Hello World!` on the card

```html
<div id="some-id" class="some-class">Hello World!</div>
```

<span class="caption">Div element</span>

Notice the `id` and `class` attributes on this element. They'll shortly be
useful

Our JS should usually culminate in somehow changing the DOM, since otherwise we
won't see any of the changes we're making. There are a few ways of interacting
with DOM elements through JS, all of which are good choices. Firstly we can
select existing elements and store them in a variable

```javascript
let hello_element1 = document.querySelector("#some-id");
let hello_element2 = document.querySelector(".some-class");
```

<span class="caption">Selecting the same div element in different ways</span>

Both of these methods select the same element, in this case the one from right
above. We can manipulate it using the `textContent`, `innerText`, `innerHTML`,
and `outerHTML` properties.

```javascript
document.querySelector("#some-id").textContent = "New content!"
document.querySelector("#some-id").innerHTML   = "<strong>New</strong> content!"
document.querySelector("#some-id").outerHTML   = "<div id="new-id"><strong>New</strong> content!</div>"
```

`outerHTML` is particularly important, since we can effectively entirely
overwrite the element. It's position in the document is retained, which makes it
useful for placeholder elements, that you'll always change with JS

Another valid way to manipulate the DOM is by creating completely new elements,
then appending them to the current document. We won't use this approach very
much, though it's good to know it exists

```javascript
let new_el = document.createElement("div");  // Create <div></div> element
new_el.textContent = "My own element!";      // Fill it with some fun text
document.body.appendChild(new_el);           // Place it on the DOM
```
