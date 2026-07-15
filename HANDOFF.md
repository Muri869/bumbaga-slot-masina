# Pump Reels — integration hand-off

For whoever owns the **Bumbana Games Hub** (the React + Supabase app at `bumbana-racer.pages.dev`). This is everything needed to drop **Pump Reels** into the hub and publish it.

## What this is

- **`pump-reels.html`** — the game. One self-contained file: no libraries, no build step, no image files (canvas + emoji). Open it in any browser and it works.
- It's a **free-play social-casino slot**: 5×3 reels, 9 paylines, virtual chips only ("Nanas" 🍌). **No real money, no crypto, no purchases, no cash-out.** The "for entertainment only / no cash value" notice is built into the UI (SL + EN).

## Publishing options

1. **Static file (simplest):** copy `pump-reels.html` into the hub's static/public assets and link the PLAY button to it. Done.
2. **As a route/component:** the game is plain HTML/CSS/JS — either iframe `pump-reels.html`, or lift the `<script>` into a component. All tunable data lives in the `CONFIG`, `PAYTABLE`, `WEIGHTS`, and `PAYLINES` constants at the top of the script.

## Storage conventions (matches the live hub — verified from the deployed bundle)

- **Plays counter (feeds "N plays across all games"):** the game increments
  `localStorage["game_plays_pump_reels"]` by 1 on every spin — same `game_plays_<slug_underscored>` pattern as `game_plays_chart_jumper`, `game_plays_bull_swing`, etc.
- **Game slug:** `pump-reels`.
- **Game-local state** (balance, bet, best, biggest win, daily-bonus streak, mute) lives under the `pumpreels:` localStorage prefix, isolated from the hub namespace.
- **Best score** = highest Nanas balance reached (single number, like the other games' "Best NN").

### Wiring plays/best into Supabase

Right now plays are counted locally. To make Pump Reels count in the **server** aggregate and leaderboard like the other games, replace the two methods in the `Store` object (they're isolated on purpose):

- `Store.incPlays()` → call the hub's existing "increment `game_plays_pump_reels`" Supabase path.
- `Store.getPlays()` → read the same counter.

And, if you want a Pump Reels leaderboard, push `State.best` to `leaderboard_runs_pump_reels` using the hub's existing leaderboard helper. No other game code needs to change.

## Hub card markup

Add a card matching the others, tagline **"Spin the reels. Stack the bananas."**, PLAY → the game. `index.html` in this repo is a **local preview** of the hub (recreated to match the live design) showing exactly how the Pump Reels card looks — copy its structure/wording if useful. It is a preview only, not the production hub.

## Tuning (all at the top of the `<script>` in `pump-reels.html`)

- `CONFIG` — starting balance, bet options, free-spin multiplier, jackpot, big/mega-win thresholds, daily-bonus schedule, refill amount/cooldown.
- `WEIGHTS` — per-symbol reel-strip frequency (controls RTP / hit rate). Currently tuned toward ~94% RTP, ~30–40% hit frequency.
- `PAYTABLE` — payout multipliers per symbol for 3/4/5 of a kind.
- `PAYLINES` — the 9 lines.

## Not included (optional stretch, noted in the brief)

- Share-on-big-win (needs the hub's share/X infra).
- "Airdrop Bonus" pick-a-crate round (3× 🎁).
