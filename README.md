# NO CHOICE FC

**NO CHOICE FC** is a static, mobile-first football career simulator built with only HTML, CSS and vanilla JavaScript.

You play a young footballer with a disability from age 16 through retirement. The game offers choices constantly: training plans, interviews, transfers, penalties, sponsorships, tactical decisions and more.

There is one problem.

**Your choices do not affect the underlying outcome.**

Every meaningful resolution is calculated from the player's current stats, a difficulty value and a seeded random roll. The selected option changes the flavour text and the career log, but the resolution function deliberately never receives the choice.

The joke is aimed at football's obsession with narratives of agency: managers, pundits, sponsors, agents, tabloids and corporate diversity campaigns. The protagonist is never the punchline.

## Files

```text
index.html
style.css
script.js
README.md
```

No build process, npm packages, frameworks, CDNs, fonts or external assets are required.

## How to play

1. Open `index.html` in a modern browser.
2. Start a new career.
3. Roll the d12. The result determines the character's disability and its stat modifiers.
4. Enter the academy.
5. Each season contains three narrative decisions followed by a small simulated run of matches.
6. Seasons advance automatically until retirement.
7. The game autosaves to `localStorage` after meaningful actions.
8. **Continue** appears on the title screen when a save exists.
9. The save button also allows an explicit save at any time.
10. The retirement screen gives you the career arc, totals, timeline, seed and the final agency reveal.

## Stat system

There are ten visible stats:

- Pace
- Stamina
- Passing
- Shooting
- Dribbling
- Defending
- Strength
- Mental Resilience
- Fitness
- Luck

At character creation, each stat receives a partially randomised base value. The d12 disability result then applies a fixed set of modifiers. Stats are clamped to 1–99.

Age progression is formula-driven. Physical attributes peak earlier and decline more quickly in the 30s. Technical attributes develop for longer and decline more gently. Mental Resilience gradually improves. Fitness becomes increasingly important as the career ages.

**No event choice directly changes a stat.**

### Disability table

| d12 | Disability | Modifiers |
|---:|---|---|
| 1 | Below-knee prosthetic leg | Pace -7, Dribbling -4, Mental Resilience +6 |
| 2 | Mild cerebral palsy | Dribbling -6, Fitness -2, Mental Resilience +7 |
| 3 | Deaf | Passing +4, Defending -3, Mental Resilience +5 overall |
| 4 | Visual impairment | Passing -5, Shooting -6, Mental Resilience +8 |
| 5 | Missing arm | Strength -5, Dribbling -2, Fitness -2 |
| 6 | Chronic fatigue condition | Stamina -10, Fitness -7, Passing +5 |
| 7 | Dwarfism | Strength -8, Defending -4, Dribbling +5 |
| 8 | Epilepsy | Fitness -5, Mental Resilience +6 |
| 9 | Cystic fibrosis | Stamina -10, Fitness -9, Mental Resilience +7 |
| 10 | Spinal condition | Pace -7, Strength -6, Passing +6 |
| 11 | Autism spectrum | Mental Resilience +4, Passing +3 |
| 12 | Multiple minor conditions | Three random stats -3, two random stats +3 |

The game does not treat disability as a joke or as a measure of a person's worth. Narrative satire is directed at the systems surrounding the player.

## Resolution system

The central resolver is:

```js
resolveOutcome(stats, difficulty, rng, weights)
```

Conceptually it does this:

```js
ability = weightedStats(stats)
roll = seededRandom() * 100
score = ability * .78
      + mentalResilience * .07
      + luck * .15
      + roll
      - difficulty

success = score >= 0
```

Different event categories use different stat weightings. A match favours things such as Shooting, Passing, Dribbling, Pace and Stamina. Injury events strongly favour Fitness and Stamina. Media events favour Mental Resilience and Luck.

### The important bit

The choice is **not** an argument to `resolveOutcome`.

```js
// The chosen option is deliberately absent. Narrative choices can change
// flavour, never the underlying numbers or odds.
function resolveOutcome(stats, difficulty, rng, weights) {
    // ...
}
```

This makes the central gimmick a property of the game architecture rather than merely a claim made by the narrator.

## Seeds and replayability

Each career has a numeric seed. The seed determines the character-generation randomness and contributes to later deterministic resolution streams.

The seed is displayed on the retirement screen. Keeping the same seed and the same career progression makes the underlying career reproducible.

The visible dice animation uses an ordinary visual roll, but the actual character result is already determined by the seeded career roll. This prevents the animation from becoming an accidental hidden choice mechanic.

## Content

The game includes more than 30 distinct events covering:

- Training
- Penalties and matches
- Scouting
- Transfers
- Contract negotiations
- Agents
- Manager sackings
- Injuries
- Media interviews
- Tabloid narratives
- Sponsor campaigns
- Diversity PR
- Rivalries
- Ownership scandals
- Community events
- Tactical changes
- Captaincy
- Retirement
- Hall of Fame recognition

Recurring personalities include:

- **Dorian Quill** — cynical, expensive agent
- **Colin Rusk** — clueless laminated-diagram manager
- **The Bellmouth Gazette** — inspirational headline factory
- **Brand Authenticity** — the sponsor philosophy that means whatever marketing needs it to mean

## GitHub Pages deployment

### Option A: GitHub website

1. Create a new GitHub repository, for example `no-choice-fc`.
2. Upload:
   - `index.html`
   - `style.css`
   - `script.js`
   - `README.md`
3. Commit the files to the repository's default branch.
4. Open **Settings → Pages**.
5. Under **Build and deployment**, choose **Deploy from a branch**.
6. Select your default branch (usually `main`) and the `/ (root)` folder.
7. Click **Save**.
8. GitHub will provide the Pages URL after deployment finishes.

### Option B: Git command line

```bash
git init
git add index.html style.css script.js README.md
git commit -m "Initial NO CHOICE FC release"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/no-choice-fc.git
git push -u origin main
```

Then enable GitHub Pages using **Settings → Pages → Deploy from a branch → main → /(root)**.

## Local testing

You can simply double-click `index.html` in a browser. No local server is required.

For a local HTTP server, any static server works. For example, with Python:

```bash
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Save behaviour

The game stores its current career in browser `localStorage` under:

```text
ironbound-save-v1
```

The save is local to that browser/device. GitHub Pages does not store player saves on the server.

Using **New Career** asks for confirmation before overwriting the current save.

## Future expansion ideas

- More detailed positions and tactical roles.
- A full league table with promotion/relegation.
- Cup competitions and European campaigns.
- Transfer market with fictional clubs bidding against each other.
- More disability-specific accessibility options and football adaptations.
- Player relationships and recurring NPC arcs.
- News feed generated from the actual career statistics.
- Procedural press conferences.
- Rival player careers and generated generations of academy players.
- A post-retirement coaching/media route.
- Export/import career seeds and save codes.
- An optional "hard mode" with less visible information about the resolution system.
- A more elaborate final statistical reveal showing every choice alongside the identical counterfactual result.
