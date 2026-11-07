# The King's Rule (1983) — Apple IIc Terminal Simulation

A playable HTML remake of *The King's Rule* (Sunburst Communications, 1983) — an educational logic puzzle game where you progress through 9 chambers, solving mathematical cypher puzzles to reach the King.

## How to Play

1. Extract `game.html` and the `assets/` folder to the same directory.
2. Open `game.html` in any modern web browser (Chrome, Firefox, Edge, Safari).
3. Press **Enter** to begin.
4. Each guard gives you **3 cypher keys**. Type your guess for the **4th key** using the number keys **0–9**.
5. Press **Enter** to submit your guess.
6. Use **Backspace** to delete the last digit.
7. You have **3 strikes per guard**. Fail 3 times and it's Game Over.
8. Reach The King (Room 9) — but beware: he denies every guess.

## Controls

| Key | Action |
|-----|--------|
| `0`–`9` | Type cypher guess digit |
| `Backspace` | Delete last digit |
| `Enter` | Submit guess / advance screens |

**Keyboard only.** No mouse input required.

## Game Structure

- **9 Chambers** — 8 guards/sentinels, then The King as the final chamber
- **Random cyphers** — each playthrough generates new number sequences (addition, multiplication, squares, Fibonacci, alternating patterns)
- **Increasing complexity** — Rooms 1–5 use 2-digit answers; Rooms 6–8 use 3-digit answers
- **3-strike system** per guard
- **Unique fail text** for each guard
- **The King is unbeatable** — every guess is denied (discover through play, not stated in text)

## Files

| File | Description |
|------|-------------|
| `game.html` | Complete playable game (single HTML file) |
| `assets/background.png` | Amiga-style copper bar + Apple IIc terminal background |
| `assets/gate_guard.png` | Gate Guard portrait |
| `assets/hall_warden.png` | Hall Warden portrait |
| `assets/tower_sentry.png` | Tower Sentry portrait |
| `assets/chamber_keep.png` | Chamber Keep portrait |
| `assets/vault_warden.png` | Vault Warden portrait |
| `assets/keep_guardian.png` | Keep Guardian portrait |
| `assets/sentinel_prime.png` | Sentinel Prime portrait |
| `assets/dark_sentinel.png` | Dark Sentinel portrait |
| `assets/the_king.png` | The King portrait |

## Requirements

- Any modern web browser with JavaScript enabled
- No server, no build step, no external dependencies beyond the `assets/` folder

## Notes

- `game.html` loads images from the `assets/` folder — keep them together.
- All sound is generated live using the Web Audio API (no audio files required).
- Inspired by the 1983 Sunburst Communications original. All artwork and audio are original creations.

## License

Original concept: *The King's Rule* (Sunburst Communications, 1983).
This HTML remake: original work.