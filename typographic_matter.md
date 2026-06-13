# On What Cornice Is Made Of

Cornice was never really ASCII. ASCII was a placeholder — a way of saying *this
world is built from text, not images*. But the constraint we were invoking
belongs to the terminal: a fixed grid, one monospaced face, characters as the
only available paint. We kept the spirit and outgrew the substrate.

The reframing is this. The terminal asked: **what can we make a world *look* like
using only the affordances of text?** Cornice asks the same question twice —
once backward, once forward. Backward, toward the terminal, we inherit the
discipline: the grid, the glyph-as-unit, world interiority relocated from image
to character. Forward, toward where text actually lives in 2026, we inherit the
browser's whole typographic apparatus: a character can carry a background, an
opacity, a blend, a weight, a face. A press of the space bar is no longer
nothing — it is a cell with a colored ground and no figure. Silence that still
occupies the grid. Text that has stopped speaking but not stopped being text.

So the surface is no longer a contradiction with the thesis; it *is* the thesis,
updated. CSS is how text is rendered on the web now — the background-color of a
glyph is as much a property of that glyph as its shape. When `mall_marble`
becomes mostly polished ground with the occasional `▫` surfacing like grout,
nothing has become an image. A space character is still a character. The tile is
*mostly quiet*, not *imageless*. We hold the line not by refusing the
background, but by treating the blank as a glyph in the rotation — the floor of
Cornice never fully stops being writing, it just learns to whisper.

And once the grid is held by the **cell box** rather than the **typeface**, the
monospace assumption falls away. The lattice is structural; any face can sit
inside it, centered, gridded by the box. That opens the writing system itself.
Cornice's imaginary is Southeast Asian and Japanese — and now its matter can be
too. `signal_moss` grows in CJK radicals (生, 氵, 茻) because conductive,
receptive moss should read as *encoded*, as script we are not quite parsing,
rather than as decorative `%`. Devanagari, Noto Symbols, the whole Unicode plane
become available as terrain matter — not as flavor, but as the claim that the
text-world is a *multilingual* text-world, that the urban imaginary it models
was never monoscriptal to begin with. Zalgo, combining marks, symbol fonts: all
of it is now in the material vocabulary, the same way a painter has more than one
pigment.

We don't have a settled name for this yet. "ASCII worldtoy" undersells it;
"typographic terrarium" is closer. What's precise is the operation: **every cell
is a typographic event** — a face, a glyph (possibly blank), a color, a ground,
an opacity — and the world is the accumulation of those events on a grid the
terminal taught us to respect and the browser lets us exceed.

---

## What changed in the build (technical)

- **Tile styles are generated once at boot**, not written per-cell. `injectTileStyles()`
  walks `TILE_DB` after each `buildDB()` and emits one CSS rule per tile id
  (`.cell[data-tile="…"]`), with the glyph color, glyph opacity (baked into the
  alpha channel), and background matter all precomputed in JS to plain `rgba()`.
  Thousands of cells now share one computed style; `refreshCell` writes a single
  precomputed class string (`spec._cls`) instead of four-plus inline custom
  properties and a live `color-mix()` per cell. Cheaper to paint, cheaper to
  recompute.
- **Fills re-tiered** along a field / figure / boundary / veil scheme. Field
  tiles (`mall_marble`, `lost_pavement`, `ceramic_pool`, `sand_drift`,
  `gravel_bed`, `frost_crust`) are background-dominant: the fill carries the
  surface, the glyph drops to low opacity and gains a blank `" "` in rotation so
  most cells read as quiet ground with occasional surfacing texture. Figures
  (flowers, relics, entities) stay glyph-only — no fill, negative space
  preserved. `escalator_teeth` lost its fill because the `≡` *is* the teeth: it's
  a figure pretending to be a floor. `shallow_pool`'s near-invisible 5% fill was
  committed to a readable 20% rather than paying for the machinery for nothing.
- **Gradients reserved** for liquids (`water`) and walls (`moss-wall`,
  `bruised-stone`), where the directional fade does real work. Field fills are
  flat — indistinguishable from a gradient at 1ch, faster to render.
- **Multi-script terrain enabled.** A per-tile `script` field (`"cjk"`, `"dev"`,
  `"sans"`, `"sym"`) routes a cell to a Noto face via a font class, sized to sit
  in the grid. `signal_moss` is the first tile to use it. Extending it is one
  field plus, if needed, one line in the font `<link>`.
