
# Insert SVG Fields Solution

Back Template:

```html
<span id="svg-accent-placeholder" style="display: none"></span>

<!-- Render SVG field literally -->
<script>
    (function renderLiteralSVG() {
        const g {
            svg_field: `{{ SVG }}`,
            svg_el: document.querySelector("#svg-accent-placeholder"),
            text_class: "accent-text",
            path_class: "accent-path",
            path_class: "accent-circle",
        };

        let field_html = g.svg_field
           .replaceAll('&lt;', '<')
           .replaceAll('&gt;', '>')
           // Determines the element type from the style attribute
           .replace(/style="[^"]*font-size[^"]+"/g, `class="${g.text_class}"`)
           .replace(/style="[^"]*stroke[^"]+"/g, `class="${g.path_class}"`)
           .replace(/style="[^"]*opacity[^"]+"/g, `class="${g.circle_class}"`);

        g.svg_el.outerHTML = field_html;
    })();
</script>
```

Styling pane:

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
