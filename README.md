# THE KING'S RULE — SWF Edition

A single-file Flash (`.swf`) port of the HTML5 remake of *The King's Rule*
(Sunburst Communications, 1983) — the Apple IIc terminal simulation where you
solve nine cypher chambers to reach a King who denies every guess.

**Deliverables**

| File | Use |
|---|---|
| `game.html` + `assets/` | **Easiest, works offline.** The HTML5 game with the harder cyphers. Double-click `game.html`; keep `assets/` next to it. |
| `kings_rule.html` | **One self-contained page** — the SWF embedded as base64, Ruffle (CDN) plays it. Double-click into any modern browser. |
| `kings_rule.swf` | The actual 783 KB SWF — one file, everything embedded (9 portraits, 10 sounds, font, code). Open with Ruffle or a Flash Player projector, **not** an audio player. |
| `kings_rule_swf.zip` | All of the above + `HOW-TO-OPEN.txt`, ready to hand over. |

The SWF's checksum: `MD5 c105ee1b24f0d0c9ad90b827b6e9260c  kings_rule.swf` (782,819 bytes).

Tip: if double-clicking the `.swf` opens Winamp (a common Windows file
association), use `kings_rule.html` instead — it contains the identical SWF.

```bash
python3 tools/make_html.py      # rebuild kings_rule.html from kings_rule.swf
```

---

## Run it

| How | Notes |
|---|---|
| **Ruffle** (recommended, 2026-friendly) | Open `web/play.html` (loads Ruffle from a CDN) or drag the `.swf` onto any Ruffle-enabled page/desktop build. |
| **Flash Player projector** | Open the `.swf` directly with a standalone Flash Player / projector. |
| **Browser with Flash plugin** | Legacy environments only. |

Click once inside the player so it takes keyboard focus, then press **Enter**.

## Controls

| Key | Action |
|---|---|
| `0`–`9` | Type the 4th cypher key (max 4 digits) |
| `Enter` | Submit guess / advance screens |
| `Backspace` | Delete last digit |
| `?` (`Shift`+`/`) | Ask the guard for a hint — the King refuses |

## Screen flow (identical in `game.html` and the SWF)

Every screen is opaque and replaces the previous one — nothing is stacked,
dimmed or left underneath:

```
INTRO (copper bars)  --Enter-->
ENTER THE CASTLE (rules)  --Enter-->
CHAMBER 1..9  --correct answer-->
    ACCESS GRANTED  (reward screen, ~1.2 s: the solved 4th key shown green)
    CHAMBER CLEARED  --Enter--> next chamber
    3rd wrong guess -->
    CAST OUT (guard's failure screen)  --Enter--> kick-out / dungeon / execute animation (~2 s)
        --> GAME OVER  --Enter--> CREDITS  --Enter--> INTRO
```

Three strikes per guard and you are cast out. The King (Chamber 9) is
unbeatable: every guess is denied. There is no victory screen — that is the
original design, preserved here.

## What's in the box

- **9 chambers** — 8 guards/sentinels then The King.
- **Harder cyphers** (this edition) — values run to **9999** and answers can be
  **four digits** (type up to 4 digits). The difficulty curve is quadratic, and the
  hard patterns arrive much sooner:

  | Chamber | Patterns in play |
  |---|---|
  | 1 | add, subtract |
  | 2 | + multiply |
  | 3 | + divide |
  | 4 | + squares |
  | 5 | + cubes, Fibonacci |
  | 6 | + alternating (+n then −m) |
  | 7 | + growing steps (the step itself increases) |
  | 8 | + primes, compound (double, then subtract c) |
  | 9 | THE KING — unknown cypher, every guess denied |

  Two patterns are new in this edition: **growing steps** (second differences,
  e.g. `2514, 3276, 4089 -> 4953`) and **compound** (e.g. `977, 1910, 3776 ->
  7508`, solved as `x2 −44`). Multipliers, steps and prime ranges all scale up
  with the chamber.
- **3-strike system** with a freshly randomised cypher after each miss.
- **Failure animations** — kickout, dungeon, execution, and the King's own
  beheading scene, plus the GAME OVER sequence.
- **Amiga copper-bar intro** and ASCII-framed modal screens (castle, transition,
  cast out, game over, credits).
- **Portraits** for every guard and the King, framed in the wooden frame.
- **Soundtrack + SFX** — the medieval Dorian score (drone, lute arpeggio, lead,
  bell, drum) plus every sting, all rendered offline from the original Web Audio
  patch and embedded as MP3.
- **Scrolling marquee** and per-room procedural backdrop: forest fades into
  castle ramparts, with the throne room appearing in Chamber 9.

## Fidelity notes vs. the HTML original

The game logic is a line-for-line port of the original JavaScript: same cypher
generator, same room table, same fail texts, same animation timings, same
backdrop drawing code, same marquee copy, same credits.

Deliberate adaptations, all forced by the format or chosen for the retro look:

1. **Fixed 1024×768 stage** instead of a responsive full-window layout (Flash
   stages are fixed). The terminal is still an exact 64×24 character grid at
   640×480, and the portrait panel sits to its right rather than above it.
2. **Custom 10×20 pixel bitmap font** instead of the browser's Courier New. This
   guarantees identical rendering on every machine and makes the `╔══╗` panel
   borders tile perfectly. Box-drawing glyphs are drawn procedurally.
3. **Screen content redraws on change** rather than every frame (equivalent
   output, far less CPU in the plugin).
4. **The soundtrack is pre-rendered** to a 19.2 s seamless loop instead of being
   synthesised live: the SWF format has no Web Audio. The mix is generated from
   the same oscillator/envelope spec (`tools/make_assets.py`), so it sounds like
   the original.
5. **A dedicated reward screen.** The HTML original drew `ACCESS GRANTED` on the
   terminal but then repainted the frame immediately, so the message vanished
   and the player dropped straight into `CHAMBER CLEARED`. Both editions now
   hold a proper *CYPHER SOLVED* screen for ~1.2 s with the solved key
   highlighted before the cleared screen appears.
6. Portraits are downscaled to 640×388 JPEG (from ~2.5 MB PNGs each) to keep the
   SWF under 1 MB. They display at 272×165, so there is no visible loss.

## Rebuild from source

Requires only the [Haxe 4.3.x](https://haxe.org) compiler — no Flash or Flex SDK.

```bash
./build.sh            # -> kings_rule.swf (project root)
```

Asset regeneration (needs Python 3 with numpy, Pillow, lameenc, soundfile):

```bash
python3 tools/make_assets.py          # font + portraits + audio
python3 tools/make_assets.py font     # just the bitmap font -> src/BitFontData.hx
python3 tools/make_assets.py audio    # just music.mp3 + the 9 SFX
```

Local play server (serves `web/` and maps `/ruffle/` to a local Ruffle build):

```bash
python3 tools/serve.py   # http://localhost:8080
```

### Source layout

| File | Role |
|---|---|
| `src/Main.hx` | Stage layout, phase machine, input, marquee, portrait panel |
| `src/Cypher.hx` | Cypher/room generator (port of `generateRoom`) |
| `src/Backdrop.hx` | Procedural forest → castle backdrop (port of `drawBackdropTrees`) |
| `src/Anims.hx` | Failure animations (port of `drawAnim`) |
| `src/Modals.hx` | Intro / castle / transition / cast out / game over / credits + copper bars |
| `src/Term.hx` | 64×24 text terminal with dirty-row redraw |
| `src/BitFont.hx` | Bitmap font renderer (Graphics + BitmapData paths) |
| `src/BitFontData.hx` | Generated glyph data |
| `src/Res.hx` | Embedded bitmaps and sounds |
| `tools/make_assets.py` | Font, portrait and audio pipeline |
| `web/preview.html` | Ruffle wrapper (relative `./ruffle/`) |
| `web/play.html` | Ruffle wrapper (CDN, portable) |
| `tools/make_html.py` | Builds the single-file `kings_rule.html` |

## Technical

- SWF v10, AVM2/AS3 bytecode, 1024×768, 30 fps, zlib-compressed (`CWS`).
- 10 `DefineSound` (MP3) tags, 9 `DefineBitsJPEG2` bitmaps, one `DoABC` block.
- Uses only long-supported Flash APIs (`Sprite`/`Graphics`, `BitmapData`,
  `Bitmap`, `Sound`, `KeyboardEvent`, `Event.ENTER_FRAME`), so it plays in both
  Ruffle and Adobe Flash Player.

## Attribution

- **Original concept:** *The King's Rule*, Sunburst Communications (1983).
- **HTML5 remake:** Arena.ai Agent Mode.
- **SWF port:** Arena.ai Agent Mode.
- **Player / owner:** River Lyle R.

No assets were extracted from the original 1983 disk image; all artwork, sound
and code here are original creations inspired by it.
