# code_aster tutorials

EDF *code_aster / salome_meca* course material, converted from the original
slide decks into readable web pages. Layout by
[Simvia](https://simvia.tech/fr); the content and the figures of each page are
the original course, unchanged.

**Read them at <https://simvia-tech.github.io/tutorials-code_aster/>.**

Same layout as the
[code_saturne tutorials](https://github.com/simvia-tech/tutorials-code_saturne):
one directory per tutorial, grouped by topic in numbered directories.

```
<NN_topic>/<Case_Name>/
├── FIGURES/              # the figures of the tutorial
└── README.md             # the tutorial itself
```

Figures are taken from the source deck rather than redrawn: bitmaps are
extracted as-is, and figures drawn with vector shapes are rendered and cropped.

## Tutorials

| Topic | Tutorial | Source deck |
|---|---|---|
| 00_foundations | [Comp_Local_Resolution](00_foundations/Comp_Local_Resolution/) | Local resolution of constitutive laws |

## Reading them offline

`index.html` is the whole site: it renders the READMEs with Markdown and math in
the browser, no build step. Serve the repository root and open it:

```bash
python3 -m http.server 8000
```

Then <http://127.0.0.1:8000>. Opening `index.html` straight from the filesystem
does not work — it fetches the Markdown files, which browsers block over
`file://`.

## License

The course material is distributed under the
[GNU Free Documentation License](http://www.gnu.org/copyleft/fdl.html), as is
this conversion.
