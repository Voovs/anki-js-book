# Setup

To get started in this section of the workshop, copy/paste the code below into a
new card type. It's a good idea to try to understand the general idea of what
this code is saying too

You can verify that your card looks like the image below. If you don't like the
colors in light mode, feel free to change the style variables at the very top of
[Styling](#styling)

### Fields

Create these fields and fill them in:

 * English: `to be delighted`
 * Kanji: `喜ぶ`
 * Kana: `よろこぶ`
 * Word-type: `Verb`
 * Caution: `Intransitive`

### Front Template

```html
<div id="front">
    <div class="top-english plain-vocab">{{ English }}</div>

    <div class="subtext japanese">
        <span class="left-subtext">{{ Word-type }}</span>
        <span></span>
        <span class="right-subtext">{{ Caution }}</span>
    </div>
</div>
```

### Back Template

```html
{{ FrontSide }}

<div id="back">
    <hr class="breaking-line">

    <div id="kana-kanji" class="plain-vocab japanese">
        <span class="kana">{{ Kana }}</span>
        <span class="separator">　ー　</span>
        <span class="kanji">{{ Kanji }}</span>
    </div>
</div>
```

### Styling

```css
:root {  /* Feel free to change these colors. The default ones are for dark mode */
    --primary-text:  rgb(250, 189, 47);
    --secondary-text: rgb(235, 219, 178);
    --green: rgb(142, 192, 124);
    --red: rgb(251, 73, 52);
}

.plain-vocab {
    font-size: 35px;
    color: var(--primary-text);
}

.japanese {
    font-family: Hiragino Sans;
}


#front .top-english {
    text-align: center;
}
#front .subtext {
    display: flex;
    justify-content: center;
}
#front .subtext > * {
    flex-grow: 1;
    font-size: 30px;
}
#front .subtext .left-subtext {
    color: var(--green);
    text-align: right;
}
#front .subtext .right-subtext {
    color: var(--red);
}


#back .breaking-line {
    border: 1px solid var(--secondary-text);
}
#back #kana-kanji {
    display: flex;
    justify-content: center;
}
#back #kana-kanji .kana {
    width: 40%;
    text-align: right;
    white-space: nowrap;
}
#back #kana-kanji .separator {
    width: 20%;
}
#back #kana-kanji .kanji {
    width: 40%;
    text-align: left;
}
```
