# Inserting a Literal SVG

SVG, Scalable Vector Graphics, are an alternative way to represent images.
Unlike the raster images which encode pixel data, SVG encode vectors and
geometric shapes. Most images can't be represented well using geometric shapes,
though when they do work, SVG are smaller and have infinite resolution

For this section, we'll try to add a pitch-accent image to our cards. There's
already a [great addon](https://ankiweb.net/shared/info/148002038) that does
this. We'll use [the website](https://illdepence.github.io/SVG_pitch/pitch.html)
the author provides to create our SVG images

Let's get set up by creating our SVG using their [excellent
website](https://illdepence.github.io/SVG_pitch/pitch.html)

 1. Copy/paste `よろこぶ` into the text field and `hhhl` into the pitch field
 2. Right click -> "Insect" -> Console -> `document.querySelector("svg")`
 3. Right click on the output -> "Edit as HTML" -> Copy/paste into `{{ SVG }}`
    in Anki

Here's what you should have copied if the above doesn't work:

```html
<svg width="137px" height="75px" viewBox="0 0 137 75"><text x="5" y="67.5"
style="font-size:20px;fill:#000;">よ</text><text x="40" y="67.5"
style="font-size:20px;fill:#000;">ろ</text><text x="75" y="67.5"
style="font-size:20px;fill:#000;">こ</text><text x="110" y="67.5"
style="font-size:20px;fill:#000;">ぶ</text><path d="m 16,5 35,0"
style="fill:none;stroke:#000;stroke-width:1.5;"></path><path d="m 51,5 35,0"
style="fill:none;stroke:#000;stroke-width:1.5;"></path><path d="m 86,5 35,25"
style="fill:none;stroke:#000;stroke-width:1.5;"></path><circle r="5" cx="16"
cy="5" style="opacity:1;fill:#000;"></circle><circle r="5" cx="51" cy="5"
style="opacity:1;fill:#000;"></circle><circle r="5" cx="86" cy="5"
style="opacity:1;fill:#000;"></circle><circle r="5" cx="121" cy="30"
style="opacity:1;fill:#000;"></circle></svg>
```

Unlike raster images, which Anki knows how to insert properly into the DOM, SVG
images are simply HTML declarations. Try placing `{{ SVG }}` at the bottom of
your Front Template to see what happens

Anki is being too smart and converting the field to be HTML-safe, which in this
case we don't want. Inserting it with JS is pretty easy though

Start by adding a placeholder element at the bottom of your Back Template

```html
<span id="svg-accent-placeholder" style="display: none"></span>
```

Although we could append directly to the DOM, having a placeholder element makes
things visually clear. After selecting the element, we'll replace it with the
SVG element we've typed into `{{ SVG }}`

```javascript
document.querySelector("#svg-accent-placeholder").outerHTML = `{{ SVG }}`;
```

Problem solved right? Well no. For the same reason simply dropping `{{ SVG }}`
into our DOM directly won't work, this approach is still missing a step. If you
have print-debugging set up and try console logging `{{ SVG }}`, you'll get this
output:

```
&lt;svg width="137px" height="75px" viewBox="0 0 137 75"&gt;&lt;text x="5"
y="67.5" style="font-size:20px;fill:#000;"&gt;よ&lt;/text&gt;&lt;text x="40"
y="67.5" style="font-size:20px;fill:#000;"&gt;ろ&lt;/text&gt;&lt;text x="75"
y="67.5" style="font-size:20px;fill:#000;"&gt;こ&lt;/text&gt;&lt;text x="110"
y="67.5" style="font-size:20px;fill:#000;"&gt;ぶ&lt;/text&gt;&lt;path d="m 16,5
35,0" style="fill:none;stroke:#000;stroke-width:1.5;"&gt;&lt;/path&gt;&lt;path
d="m 51,5 35,0"
style="fill:none;stroke:#000;stroke-width:1.5;"&gt;&lt;/path&gt;&lt;path d="m
86,5 35,25"
style="fill:none;stroke:#000;stroke-width:1.5;"&gt;&lt;/path&gt;&lt;circle r="5"
cx="16" cy="5" style="opacity:1;fill:#000;"&gt;&lt;/circle&gt;&lt;circle r="5"
cx="51" cy="5" style="opacity:1;fill:#000;"&gt;&lt;/circle&gt;&lt;circle r="5"
cx="86" cy="5" style="opacity:1;fill:#000;"&gt;&lt;/circle&gt;&lt;circle r="5"
cx="121" cy="30" style="opacity:1;fill:#000;"&gt;&lt;/circle&gt;&lt;/svg&gt;
```

Anki escaped all the `<` and `>` to stay HTML-safe, though that also means our
field won't render as HTML anymore. To fix this we'll replace them back

```javascript
let svg_el = document.querySelector("#svg-accent-placeholder");
let svg_field = `{{ SVG }}`.replaceAll('&lt;', '<').replaceAll('&gt;', '>');
svg_el.outerHTML = svg_field;
```

Now it all works. We can render HTML fields literally. With [some
cleanup](/anki_js/coding_style.md) the final code looks like

```html
<span id="svg-accent-placeholder" style="display: none"></span>

<!-- Render SVG field literally -->
<script>
    function renderLiteralHTML(field_str, placeholder_selector) {
        let element = document.querySelector(placeholder_selector);
        let field_html = field_str.replaceAll('&lt;', '<')
                                  .replaceAll('&gt;', '>');

        element.outerHTML = field_html;
    }

    renderLiteralHTML(`{{ SVG }}`, "#svg-accent-placeholder");
</script>
```

### Exercise - Custom styles

What if we want to apply customized styles to the SVG? If your Anki is in night
mode, the SVG is really hard to see: It's entirely black!

As a first step, try changing the entire image into white, without using a CSS
`important!` declarative. Then, change the element such that the following CSS
code from the Styling pane works

```css
#back svg text.accent-text {
    font-size: 20px;
    fill: white;
}
#back svg circle.accent-circle {
    opacity: 1;
    fill: white;
}
#back svg path.accent-path {
    fill: none;
    stroke: white;
    stroke-width: 1.5;
}
```

[See the solution here](solutions_2.md)
