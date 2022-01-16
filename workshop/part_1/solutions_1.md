# Conditional DOM Solutions

## Exercise 1 - Centering Kanji

```html
<!-- Center Kana or Kanji when only one of the two is present -->
<script>
    (function centerKanaOrKanji() {
        let kana = document.querySelector("#kana-kanji > .kana");
        let sep = document.querySelector("#kana-kanji > .separator");
        let kanji = document.querySelector("#kana-kanji > .kanji");

        if (kanji.textContent === "") {
            sep.remove();
            kanji.remove();
        else if (kana.textContent === "") {
            sep.remove();
            kana.remove();
        }
    })();
</script>
```

## Exercise 2 - Centering Kanji

```html
<!-- Center Kana or Kanji when only one of the two is present -->
<script>
    (function centerKanaOrKanji() {
        let kana_field = `{{ Kana }}`;
        let kanji_field = `{{ Kanji }}`;
        let kana_kanji_el = document.querySelector("#kana-kanji");

        if (kanji_field === "") {
            kana_kanji_el.textContent = kana_field;
        else if (kana_field === "") {
            kana_kanji_el.textContent = kanji_field;
        }
    })();
</script>
```

## Exercise 3 - Styling Centered Kana

In Back Template:

```html
<!-- Center Kana or Kanji when only one of the two is present

Styles centered element with class `.centered-kana`
-->
<script>
    (function centerKanaOrKanji() {
        let kana_field = `{{ Kana }}`;
        let kanji_field = `{{ Kanji }}`;
        let kana_kanji_el = document.querySelector("#kana-kanji");

        if (kanji_field === "" || kana_field === "") {
            let centered_field = (kana_field === "") ? kanji_field
                                                     : kana_field;

            kana_kanji_el.textContent = centered_field;
            kana_kanji_el.classList.add("centered-kana");
        }
    })();
</script>
```

In Styling pane:

```css
#back #kana-kanji.centered-kana {
    color: var(--red);
}
```
