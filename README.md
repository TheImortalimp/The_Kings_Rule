# The King's Rule (1983) — Apple IIc Terminal Simulation

A playable HTML remake of *The King's Rule* (Sunburst Communications, 1983) — an educational logic puzzle game where you progress through 9 chambers, solving mathematical cypher puzzles to reach the King.

## How to Play

1. Extract `game.html` and the `assets/` folder to the same directory.
2. Open `game.html` in any modern web browser (Chrome, Firefox, Edge, Safari).
3. Click the window once to focus, then press **Enter** to begin.
4. Each guard gives you **3 cypher keys**. Type your guess for the **4th key** using the number keys **0–9**.
5. Press **Enter** to submit your guess.
6. Use **Backspace** to delete the last digit.
7. Press **?** for a hint (except against The King — he refuses).
8. You have **3 strikes per guard**. Fail 3 times and you are cast out.
9. Reach The King (Room 9) — but beware: **he denies every guess**. No one has ever passed him.

## Controls

| Key | Action |
|-----|--------|
| `0`–`9` | Type cypher guess digit (up to 3 digits) |
| `?` | Show hint for current guard (The King refuses) |
| `Backspace` | Delete last digit |
| `Enter` | Submit guess / advance screens |

**Keyboard only.** No mouse input required.

## Game Structure

- **9 Chambers** — 8 guards/sentinels, then The King as the final chamber
- **Randomized cyphers** — every playthrough generates new number sequences (addition, subtraction, multiplication, division, squares, Fibonacci, alternating +n/-m, primes)
- **Incremental difficulty** — Rooms 1–5 use 2-digit answers; Rooms 6–8 use 3-digit answers; Room 9 is The King
- **3-strike system** per guard
- **Unique fail text** for each guard
- **Failure animations** — kicked out of the castle, locked in the dungeon, executed by The King's rule (with blood/guts execution scene)
- **The King is unbeatable** — every guess is denied (discover through play, not stated in text)
- **No victory** — you cannot defeat The King; the game always ends in game over
- **Intro animation** — copper bar title sequence on launch
- **Outro credits** — full attribution shown after every game over, with your score

## Features

- Procedural canvas graphics — no external image dependencies for gameplay
- Procedural Web Audio soundtrack — melodic pads, bass, arpeggios, percussion
- Sound effects — correct, wrong, strike, transition, failure animations
- Apple IIc text-mode terminal aesthetic with Amiga-style copper bars
- High-fidelity guard portraits in `assets/`

## Files

| File | Description |
|------|-------------|
| `game.html` | Complete playable game (single HTML file) |
| `assets/background.png` | Amiga copper bar + Apple IIc terminal background |
| `assets/gate_guard.png` | Gate Guard portrait |
| `assets/hall_warden.png` | Hall Warden portrait |
| `assets/tower_sentry.png` | Tower Sentry portrait |
| `assets/chamber_keep.png` | Chamber Keep portrait |
| `assets/vault_warden.png` | Vault Warden portrait |
| `assets/keep_guardian.png` | Keep Guardian portrait |
| `assets/sentinel_prime.png` | Sentinel Prime portrait |
| `assets/dark_sentinel.png` | Dark Sentinel portrait |
| `assets/the_king.png` | The King portrait |
| `README.md` | This file |
| `.gitignore` | Standard ignore rules |

## Requirements

- Any modern web browser with JavaScript enabled
- No server, no build step, no installation required
- `game.html` loads images from the `assets/` folder — keep them together
- All sound is generated live using the Web Audio API (no audio files required)

## Attribution

**Original Concept:** *The King's Rule* — Sunburst Communications (1983)
Educational logic puzzle game for Apple II.

**HTML5 Remake & Programming:** Arena.ai Agent Mode
Procedural graphics, Web Audio engine, cypher generator, animations.

**Player / Owner:** River Lyle R.

All artwork and sound in this remake are original creations inspired by the 1983 original. No assets were extracted from the original disk image.

## License

Original concept: *The King's Rule* (Sunburst Communications, 1983).
This HTML remake: original work.