# Getting Setup

## Setting up an editor

Since anki's editor was never intended for extensive coding, we'll write our
code elsewhere then copy/paste it into anki's template editor. If you don't have
a text editor, there are plenty of easy-to-use free editors out there.
[Atom](https://atom.io/) and [VScode](https://code.visualstudio.com/) are quite
popular options. You can also use any online editor without needing to download
anything. [Codepen](https://codepen.io) and [JSfiddle](https://jsfiddle.net) are
more than enough for the scripts we'll be writing

You may also want an environment to test your script out. As we'll explain
later, this is a real pain to do through anki. Luckly your browser already has a
JS console built in. You can access this console by right clicking and choosing
inspect element. Navigating to [`about:blank`](about:blank) in Chrome
will give you a blank webpage to work with

For those more terminal-inclined, use NodeJS. Actually it's easier to just write
the script and shebang `#!/usr/bin/env node` on the top line. Remember though,
anki doesn't support any NodeJS modules, so `require()` won't work

You can use external JS libraries by downloading them into Anki. Check out this
[blog for a
tutorial](https://forums.ankiweb.net/t/how-to-include-external-files-in-your-template-js-css-etc-guide/11719)

Getting print-debugging working in anki is somewhat more complicated. Refer to
the [advanced section](anki_js/advanced.html#print-debugging) if you're interested

