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
| `meta.yaml` | Package metadata: version, artifacts + MD5 manifest, requirements, verification, changelog. |
| `game.yaml` | Game metadata: rules, screen flow, the nine chambers, cypher patterns, difficulty table. |

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
| `?` (`Shift`+`/`) | Ask the warden for the rule + the working (free, toggles) — the King refuses |

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

Three strikes per warden and you are cast out; the keys stay put between the
three, so a strike costs you a try and never the puzzle. The King (Chamber 9)
is unbeatable: every guess is denied. There is no victory screen — that is the
original design, preserved here.

## What's in the box

- **9 chambers** — 8 wardens then The King.
- **One rule per chamber, on a fixed ladder** — the pattern is *not* drawn at
  random, so the run always climbs the same way:

  | Chamber | Warden | The rule (hidden until you press `?`) | Answer band |
  |---|---|---|---|
  | 1 | GATE GUARD | add a fixed step to each key | 100–155 |
  | 2 | HALL WARDEN | subtract a fixed step | 190–294 |
  | 3 | TOWER SENTRY | double each key | 368–552 |
  | 4 | CHAMBER KEEP | halve each key | 685–1063 |
  | 5 | VAULT WARDEN | each key is the next square | 1296–1936 |
  | 6 | KEEP GUARDIAN | each key is the sum of the two before | 2476–3837 |
  | 7 | SENTINEL PRIME | the keys are consecutive primes | 4721–7297 |
  | 8 | DARK SENTINEL | double each key, then subtract a fixed number | 8938–9999 |
  | 9 | THE KING | his own rule — every guess denied | — |

- **Exponential difficulty.** The answer band is multiplied by ~1.9 every
  chamber (100 → 190 → 368 → 685 → 1296 → 2476 → 4721 → 8938), so each room is
  roughly twice the arithmetic of the one before without ever leaving the
  4-digit keypad. Chamber 1 is a two/three-digit warm-up; chamber 8 sits at the
  top of the scale. The second axis — the rule itself — climbs with it.
- **The rule is the warden's secret.** Nothing is printed on the wall. `?` asks
  the warden for a hint: it shows the rule *and* the working (the rule applied
  to your three keys, with the answer left for you). It is free, it never costs
  a strike, and pressing `?` again hides it. That is the fairness valve: no
  room is ever unwinnable, because the rule is always one keystroke away.
- **The keys never change mid-chamber.** A wrong guess costs a strike, the same
  three keys stay on screen, and you can finish your arithmetic. Three strikes
  on the same keys and the warden casts you out.
- **Each warden has a voice.** Three taunts (one per strike) and a two-line
  cast-out speech per warden, plus the King's own denial.
- **Failure animations** — kickout, dungeon, execution, and the King's own
  beheading scene, plus the GAME OVER sequence.
- **Amiga copper-bar intro** and ASCII-framed modal screens (castle, transition,
  cast out, game over, credits).
- **Portraits** for every warden and the King, framed in the wooden frame.
- **Soundtrack + SFX** — the medieval Dorian score (drone, lute arpeggio, lead,
  bell, drum) plus every sting, all rendered offline from the original Web Audio
  patch and embedded as MP3.
- **Scrolling marquee** and per-room procedural backdrop: forest fades into
  castle ramparts, with the throne room appearing in Chamber 9.

## Fairness and soundness (3.1.0)

The cypher engine is verified rather than assumed. Both builds share the same
generator and the same rules:

- every room is re-derived before it is shown — `Cypher.validate()` / 
  `validateRoom()` apply the room's own rule to its own three keys and reject the
  room if the result is not the accepted answer;
- all values are generated inside `1..9999` by construction — nothing is clamped,
  so the accepted answer can never disagree with the arithmetic on screen;
- keys are always distinct and monotone, so no room is self-contradictory;
- a wrong guess never re-rolls the cypher, so a player can always finish what
  they started.

Audit: **72,000 generated rooms** (32,000 through the SWF engine, 40,000 through
the HTML5 engine) — zero failures, zero rooms where the stated rule disagrees
with the accepted answer. A scripted playthrough then walks chambers 1 → 9,
solves each one from its own hidden rule, and confirms the King denies
everything: all checks pass.

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
