# Bumbana — free-play slot for the Bumbana Games Hub

**Bumbana** — a free-play social-casino slot machine game built for the [Bumbana Games Hub](https://bumbana-racer.pages.dev/).

> Spin the reels. Stack the bananas. 🍌🎰

## What's here

| File | What it is |
|---|---|
| `bumbana.html` | **The game.** One self-contained file — no libraries, no build, no images (canvas + emoji). Open it in a browser and play. |
| `index.html` | **Local hub preview** — recreates the live hub design so the Bumbana card can be seen and tested. Not the production hub. |
| `HANDOFF.md` | Integration notes for the hub owner (storage keys, publishing, Supabase wiring, tuning). |

## Play locally

Open `index.html` (the hub) or `bumbana.html` (straight to the game) in any modern browser. No server needed.

## The game

- **5 reels × 3 rows, 9 paylines.** Match 3+ identical **crypto coins** left-to-right; wins pay `symbol multiplier × bet`. Highest → lowest: Bitcoin, Ethereum, BNB, Solana, XRP, Cardano, Dogecoin, Shiba.
- 🍌 **WILD** is the Bumbana banana mascot — substitutes any coin except the Moon; 5× banana WILD = the 100× jackpot.
- 🎁 **Airdrop Bonus** — land 3+ crates to open a pick-a-crate round for extra virtual Nanas (no cash value).
- **Las Vegas look** (gold cabinet + blinking bulb marquee) and a procedural WebAudio soundtrack (looping groove, brighter during free spins) — all synthesized, no audio files; off by default, toggle with 🔊.
- 🌙 **SCATTER** — 3 / 4 / 5 anywhere → 10 / 15 / 20 free spins ("Pump Session"), all wins ×2.
- **Nanas 🍌** are virtual chips: start with 1000, top up free with the daily bonus (grows over a 7-day streak) or a free refill when low.
- Sequential reel stops with **anticipation** on the last reel, win-line highlights, coin shower, and Big/Mega Win celebrations.
- Bet selector, Max Bet, Autospin, mute toggle (silent by default).

## Responsible design (built in, not bolted on)

**For entertainment only. Virtual chips (Nanas), no cash value. Not real-money gambling.**
No real money, no crypto, no chip purchases, no deposits, no cash-out. The daily bonus is a gift, not a "pay to continue." A gentle session reminder appears after long play.

## Conventions

Matches the deployed hub: spins increment `game_plays_bumbana` (feeds "N plays across all games"); slug `bumbana`; game-local state under the `bumbana:` namespace. See `HANDOFF.md` for details.

## Development

Everything is tunable from the `CONFIG` / `PAYTABLE` / `WEIGHTS` / `PAYLINES` constants at the top of the script in `bumbana.html`. Each reel cell is an independent weighted draw (`weightedPick`), so symbol/scatter/crate odds match `WEIGHTS` exactly. Tuned via a 1M-spin Monte Carlo to **~92% RTP, ~32% hit frequency, free spins ~1 in 106, airdrop ~1 in 265**.
