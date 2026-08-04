---

<p align="center"><img src="https://github.com/aaskjer/Blackjack-Game/blob/main/Assets/logo.png?raw=true" alt="BJG banner" width="400" height="400"></p>

Blackjack Game is a chat game built for streamer.bot, supporting obs, twitch, kick, and youtube.

---

# Overview

- Works on Twitch, YouTube, and Kick, each individually toggleable.
- Chat plays a real hand of Blackjack against the dealer — `!blackjack <amount>`, or an alias like `all`, `half`, `quarter`, `random`, `max`, `1.5k`, `50%`, or nothing at all to use the configured default bet.
- Full table rules: `hit`, `stand`, `double` down, `split` (with resplits and a max-hands limit), `surrender`, `insurance`.
- Configurable dealer behaviour — stands on 16–21, 1–8 decks, reshuffle threshold, draw delay, and whether a tie is broken by a single extra card or by drawing until it breaks.
- Special hands: natural Blackjack at a configurable payout, plus 5-Card Charlie (1.5x) and Triple 7s (2x) bonus wins.
- **Duel Mode** — `!duel @user <bet>` lets chatters play against each other instead of the dealer. Points move directly between the two players, so the house never touches the economy.
- Duels support splits and double downs as *raises* the opponent must accept, per-hand side pots, forfeits, and an optional double-or-nothing **Sudden Death** on a tie.
- Optional "Addicted" list (individual users or whole Streamer.bot groups) with a payout cut, applied to duels too.
- Exclude specific Streamer.bot groups from playing entirely.
- Per-user and global cooldowns, plus a Max Games/Hour limit — all of which apply to duels as well.
- Free Game mode for wager-free hands, with its own dedicated set of chat messages so nothing refers to points that were never wagered.
- Auto-detects [Currency Core by StreamUP](https://streamup.tips/product/currency-core) and [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points).
- Every chat message is editable directly in the settings GUI — over 150 of them, grouped by phase with placeholder lists in the tooltips.
- Statistics are written to 44 persisted Streamer.bot user variables (`bjg_*`), so any other action can read a player's record — `bjg_duelsWon`, `bjg_netProfit` — without involving this game at all.
- Persistent daily log file with every hand played, filterable and searchable in the built-in log viewer.
- Built-in WPF settings window with dark/light mode, tab navigation, live rule summaries, and a colour picker for the overlay.

---

## 🎮 How It Works

Chat starts a hand with `!blackjack <bet>` (or a bet alias). Cards are dealt, the player acts, the dealer plays out, and the hand settles instantly.

| Event | Trigger | Result |
|---------|---------|---------|
| 🃏 **Deal** | A viewer runs the blackjack command with a bet | The bet is deducted (unless Free Game) and two cards are dealt to the player and dealer. |
| ✅ **Win** | Player beats the dealer, or the dealer busts | The bet is paid 1:1 and credited to the viewer's balance. |
| 🔥 **Blackjack** | 21 on the first two cards | Pays the configured Blackjack multiplier on top of the returned stake. |
| ⭐ **Special** | 5 cards without busting, or three 7s | 5-Card Charlie pays 1.5x, Triple 7s pays 2x. |
| 💥 **Bust** | Hand goes over 21 | The bet is lost immediately. |
| 🤝 **Push** | Player and dealer tie | The dealer draws to break the tie, or the stake is returned, depending on the tie rule. |
| 🚫 **Blocked / Addicted** | Player is in a blocked group or on the Addicted list | Blocked players can't play at all; Addicted players play normally but with a payout cut. |
| ⏱️ **Cooldown** | Player plays again too soon, or hits Max Games/Hour | A cooldown message is sent instead of a hand. |

---

## ⚔️ Duel Mode

Instead of the dealer, chat plays each other. One player challenges another with a bet, the challenged player accepts or declines, and the winner takes the pot.

| Command | Info |
|---------|---------|
| `!duel @user <bet>` | Challenge a chatter — same bet shortcuts as `!blackjack`, and the order doesn't matter |
| `accept` / `decline` | Answer whichever prompt is currently waiting on you |
| `hit` / `stand` | Play your hand, same as a normal game |
| `double` / `split` | Request a raise — the opponent has to agree, since it raises the stakes for both |
| `quit` | Forfeit — your opponent takes the whole pot |

Both players are dealt face up from one shared deck, the challenger acts first, and a natural Blackjack stands automatically. Stakes are held from the moment of the challenge and refunded on decline, timeout, or a push. Insurance and surrender are dealer rules and are correctly unavailable here.

---

## 📺 OBS Overlay

A self-contained browser source that renders the game live on stream, installed automatically into the overlay folder — no manual file copying. Add a Browser Source pointing at `blackjack_overlay.html`; the background is transparent so it layers over any scene.

- Dealer and player hands with real card faces, suits, and colours, deal-in animations, hand values, bet amounts, and outcome badges.
- Duels show both players facing off with a VS badge and the live pot, split hands stacked per side pot.
- Prompt cards with an animated countdown ring for challenges, raises, and sudden-death offers — they expire on their own so the overlay never sits on a dead prompt.
- Card scale, font size, card colors, background color and opacity, dealer name, and per-element toggles are all configurable from the OBS Overlay tab.

---

## 🏆 Leaderboard

`!allstats` ranks a top list of players. 🥇🥈🥉 medals for the top three, configurable to 1–10 places, with optional hiding of the broadcaster and bot account.

`!stats` reports a player's own record, including their duel record, duel blackjacks, busts, and forfeits.

Leaderboards can be reset on demand from the settings window, or automatically **weekly or monthly** for running seasons.

---

# Get Started

[Setup Guide](https://github.com/aaskjer/Blackjack-Game/blob/main/Setup-Guide.md)

[Frequently Asked Questions](https://github.com/aaskjer/Blackjack-Game/blob/main/FAQ.md)

[Import String for streamer.bot](https://github.com/aaskjer/Blackjack-Game/blob/main/Import-String.md)

[Download](https://github.com/aaskjer/Blackjack-Game/releases)

---

# Credits & Links

[aaskjer on Twitch](https://twitch.tv/aaskjer)

[Slot Machine Game on SB Discord](https://discord.com/channels/834650675224248362/1446889756297527530/1446889756297527530)

[Streamer.bot](https://streamer.bot) / [nate1280](https://www.patreon.com/c/nate1280/home)

YOU ♡
