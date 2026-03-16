# 🩸 Blood on the Clocktower — Digital Grimoire

A single-file, browser-based Grimoire tool for **Blood on the Clocktower** Storytellers. No installation, no build step, no backend — just open the HTML file and run your game.

---

## Features

- **Full edition support** — Trouble Brewing, Bad Moon Rising, Sects & Violets, and Travellers (all editions)
- **Night order briefings** — step-by-step ST prompts with live game-state context for every character
- **Win condition engine** — automated good/evil win detection, including edge cases like Mastermind, Zombuul, Vortox, Evil Twin, and Klutz
- **Setup randomizer** — generates a valid player/character assignment with Baron and Drunk adjustments
- **Phase tracking** — Day/Night phase bar with per-round state; visual theme shifts between phases
- **Status tracking** — dead/alive, drunk, poisoned, protected, and character-specific flags (e.g. `_assassinUsed`, `_grandchildId`, `_sweetheartDrunk`)
- **Nomination & voting** — Witch curse enforcement, Flowergirl vote tracking, Town Crier minion-nomination detection
- **Execution logic** — Pacifist, Saint, Scarlet Woman, Minstrel, and Mastermind rules all handled
- **Character-specific card buttons** — inline action buttons on player cards for Assassin kills, Sailor/Innkeeper drunk-setting, Courtier, Goon, Witch curse, Tinker death, Seamstress, and more
- **Custom confirm modals** — no native `confirm()` calls; works correctly in all iframe/embedded contexts

---

## Editions & Characters

### Trouble Brewing
Townsfolk · Washerwoman, Librarian, Investigator, Chef, Empath, Fortune Teller, Undertaker, Monk, Ravenkeeper, Virgin, Slayer, Soldier, Mayor  
Outsiders · Butler, Drunk, Recluse, Saint  
Minions · Poisoner, Spy, Scarlet Woman, Baron  
Demon · Imp

### Bad Moon Rising
Townsfolk · Grandmother, Sailor, Chambermaid, Exorcist, Innkeeper, Gambler, Gossip, Courtier, Professor, Minstrel, Tea Lady, Pacifist, Fool  
Outsiders · Goon, Lunatic, Tinker, Moonchild  
Minions · Godfather, Devil's Advocate, Assassin, Mastermind  
Demons · Zombuul, Pukka, Shabaloth, Po

### Sects & Violets
Townsfolk · Clockmaker, Dreamer, Snake Charmer, Mathematician, Flowergirl, Town Crier, Oracle, Savant, Seamstress, Philosopher, Artist, Juggler, Sage  
Outsiders · Mutant, Sweetheart, Klutz, Barber  
Minions · Evil Twin, Witch, Cerenovus, Pit-Hag  
Demons · Fang Gu, Vigormortis, No Dashii, Vortox

### Travellers
Scapegoat, Gunslinger, Beggar, Bureaucrat, Thief, Butcher, Bone Collector, Harlot, Barista, Deviant

---

## Usage

1. Download `botc-grimoire.html`
2. Open it in any modern browser
3. Select your edition and player count, then randomize or manually assign characters
4. Use the phase bar to advance through Day and Night, following the briefing prompts

No server, no npm, no build. Everything runs locally in the browser.

---

## Technical Notes

- **Single file** — all HTML, CSS, and JS in one `botc-grimoire.html` (~3,300 lines)
- **No external dependencies** except Google Fonts (Cinzel Decorative, Cinzel, IM Fell English)
- **`render()` is the single source of truth** — all state changes end with a `render()` call
- **Player lookup always by ID** via `getPlayer(id)` — never by array index
- **No inline `onclick=` handlers** — all event listeners use `addEventListener` after DOM insertion

---

## Automation Status

Many character abilities are fully automated; some require manual ST judgement. A summary:

| Status | Examples |
|---|---|
| ✅ Fully automated | Assassin, Mastermind, Zombuul, Moonchild, Vortox, Evil Twin win-block, Grandmother auto-death, Witch curse, Klutz, Sweetheart, Sage, Minstrel, Innkeeper/Sailor/Goon drunk-setting |
| ⚠️ Briefing only | Fang Gu swap, Vigormortis neighbor poisoning, Snake Charmer swap, Pit-Hag reassignment, Philosopher drunking, Barber swap, No Dashii persistent poisoning |
| ❌ Manual / social | Cerenovus (madness), Mutant, Artist, Savant, Recluse, Juggler day-guesses, Mathematician |

---

## Design

| Element | Detail |
|---|---|
| Title font | Cinzel Decorative |
| UI / labels | Cinzel |
| Body text | IM Fell English |
| Color palette | Deep crimson/maroon (`#0a0608`), gold (`#c9943a`), cream (`#e8d9c0`) |
| Night theme | Darker, cooler background |
| Day theme | Warmer amber background |

---

## Contributing / Editing

This project uses a careful single-file edit discipline. Before making any changes:

1. **Always read the relevant line range** before any edit (`sed -n 'START,ENDp'`)
2. **Make `str_replace` targets unique** — add context lines if needed
3. **Run a syntax check after every edit:**
   ```bash
   sed -n '/<script>/,/<\/script>/p' botc-grimoire.html | sed '1d;$d' > /tmp/check.js && node --check /tmp/check.js
   ```
4. **Never edit `CHARACTERS` or `PLAYER_COUNTS` blocks** without verifying the closing `};` survived intact
5. **Do not recreate `updateSTToolDisplay`** — it was removed due to infinite recursion

See `botc-grimoire-context.md` for full dev context, architecture notes, and bug history.

---

## License

This is an unofficial fan tool. *Blood on the Clocktower* is a game by [Pandemonium Institute](https://bloodontheclocktower.com). This project is not affiliated with or endorsed by the official game.
