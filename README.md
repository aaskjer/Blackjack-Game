---

<p align="center"><img src="https://github.com/aaskjer/Blackjack-Game/blob/main/Assets/logo.png?raw=true" alt="BJG banner" width="400" height="400"></p>

Blackjack Game is a chat game for streamer.bot. It runs on Twitch, Kick and YouTube, and draws itself on stream through OBS.

![Static Badge](https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-Twitch%2C_Kick%2C_YouTube-white?label=supported%20plattform)
![Static Badge](https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-streamer.bot-white?label=tool&color=%230b73ff)
![GitHub License](https://img.shields.io/github/license/aaskjer/Blackjack-Game)
![GitHub Release](https://img.shields.io/github/v/release/aaskjer/Blackjack-Game)
![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/aaskjer/Blackjack-Game/total)

---

# Overview

Three ways to play, all from the same set of rules:

- **Against the dealer** — `!blackjack <amount>` deals a hand. The amount takes a number, a shortcut (`all`, `half`, `quarter`, `random`, `max`, `1.5k`, `50%`), or nothing at all to use the configured default bet.
- **Against each other** — `!bjduel @user <bet>` puts two chatters head to head. Points move straight from one to the other, so the house never touches the economy.
- **At a table** — `!house` seats up to six players against one dealer hand, under Vegas Strip, Atlantic City or European rules.

And the rest of it:

- Twitch, YouTube and Kick. The game answers wherever its commands are set to fire, and works out which platform that was on its own.
- Full table play: `hit`, `stand`, `double`, `split` with resplits and a hand limit, `surrender`, `insurance`, and `quit` for a refund before you act.
- A real shoe: one per platform, shared by every hand and duel there, holding one to eight decks until it runs down to the reshuffle threshold. Played cards stay out until then.
- The dealer is yours to set: stands on 16 to 21, an optional peek for blackjack, a reveal delay, and whether a tie is broken by a single card or by drawing until it breaks.
- Bonus hands settle on the spot: a natural blackjack at your chosen payout, plus 5-card Charlie at 1.5x and three sevens at 2x.
- Duels take splits and doubles as *raises* the opponent has to agree to, with a side pot per hand, forfeits, and an optional double-or-nothing **sudden death** on a tie.
- Three authentic **House Mode** rule sets, each with the right peek, doubling, splitting, insurance and surrender behaviour.
- Every deadline looks after itself — the solo turn, a duel challenge, a duel turn, a raise, sudden death, the rule pick, the join window, a seat's turn. An abandoned hand refunds and closes itself instead of waiting for the next chat command.
- An "Addicted" list for individual viewers or whole streamer.bot groups: they play on, but their winnings are cut and their cooldown is doubled.
- Blocked viewers and blocked groups are dropped without a reply, so retrying cannot spam chat.
- Per-player and global cooldowns for each mode separately, plus a hands-per-hour ceiling.
- Free Game for wager-free hands, with its own set of chat lines so nothing mentions points that were never staked.
- Picks up [Currency Core by StreamUP](https://streamup.tips/product/currency-core) and [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points) on its own; anything else works by naming its variable.
- All 169 chat lines are editable in the settings window, grouped by phase, each with its placeholder list and a live chat preview.
- Statistics live in 50 persisted streamer.bot user variables (`bjg_*`), so any other action can read a player's record — `bjg_duelsWon`, `bjg_netProfit` — without involving this game at all.
- A daily log file of every settled hand, searchable and filterable in the built-in viewer.
- A WPF settings window with a flat page rail, dark and light themes, live rule summaries and a colour wheel for the overlay.

---

## 🎮 Against the dealer

Chat deals in with `!blackjack <bet>`. Cards go out, the player acts, the dealer plays, and the hand settles at once.

| Event | Trigger | Result |
|---------|---------|---------|
| 🃏 **Deal** | The blackjack command, with or without an amount | The stake is taken, unless Free Game is on, and two cards go to the player and one to the dealer |
| ✅ **Win** | The player beats the dealer, or the dealer busts | Pays 1:1 — double the stake comes back |
| 🔥 **Blackjack** | 21 on the first two cards | Pays the configured bonus on top of the returned stake |
| ⭐ **Bonus hand** | Five cards without busting, or three sevens | 5-card Charlie pays 1.5x, three sevens pays 2x, whatever the dealer holds |
| 💥 **Bust** | Over 21 | The stake is gone before the dealer plays |
| 🤝 **Level** | Player and dealer tie | The dealer draws to break it — once, or until it breaks, depending on the tie rule |
| 🚫 **Blocked or addicted** | A blocked group, or the Addicted list | Blocked viewers cannot play at all; addicted ones play normally with their winnings cut |
| ⏱️ **Cooldown** | Playing again too soon, or past the hourly limit | A cooldown reply instead of a hand |
| ⏰ **Walked away** | A hand left open | It closes itself: returned if it was never played, otherwise it stands and the dealer plays it out |

---

## ⚔️ Duel Mode

Two chatters, no dealer and no house. One challenges the other for a stake, the other accepts or declines, and the winner takes the pot.

| Command | What it does |
|---------|---------|
| `!bjduel @user <bet>` | Challenges a chatter. Same bet shortcuts as `!blackjack`, and the order does not matter |
| `accept` / `decline` | Answers whichever prompt is waiting on you |
| `hit` / `stand` | Plays your hand, as at the table |
| `double` / `split` | Asks for a raise — it lifts the stake for both, so the opponent has to agree |
| `quit` | Forfeits, and the opponent takes the whole pot |
| `repeat` | Posts both hands, the pot and whose turn it is |

Both hands are dealt face up from one shared shoe, the challenger acts first, and a natural blackjack stands by itself. Stakes are held from the moment of the challenge and returned on a decline, a timeout or a draw. Insurance and surrender are dealer rules, so a duel has neither.

---

## 🏠 House Mode

One dealer against a row of seats, all playing the same shoe. A moderator opens the table and picks the rule set, viewers sit down with stakes of their own, everyone plays in seat order, then the dealer plays once and every seat is settled against that single hand.

House Mode is played for real currency: while Free Game is on, the table refuses to open.

| Command | What it does |
|---------|---------|
| `!house [rules] [bet]` | Opens a table. Name the rule set to skip the menu (`!house vegas`), or answer the numbered menu with `!house 1`. A bet seats you at your own table |
| `join <bet>` | Takes a seat while the join window is open — same bet shortcuts as `!blackjack` |
| `deal` | Deals at once instead of waiting the window out |
| `hit` / `stand` | Plays your hand when it is your turn |
| `double` / `split` | Within whatever the rule set allows |
| `insurance` / `surrender` | Only where the rule set offers them |
| `quit` | Leaves the table. Before you have acted the stake comes back; after that the hand is played out |
| `repeat` | Posts the dealer's upcard, every seat's hand and whose turn it is |

Opening a table and dealing early are worth keeping to moderators — set that on the triggers in streamer.bot, since the script itself does not check who is asking.

### Rule sets

Each one is switchable, so you decide what the menu offers. With only one left the menu is skipped; with none, no table can open.

| Rule set | Shoe | Dealer | Double | Split | Extras |
|---------|---------|---------|---------|---------|---------|
| **Vegas Strip** | 4 decks | Stands soft 17, peeks | Any two cards, after a split too | Any two 10-value cards, up to 4 hands | Insurance |
| **Atlantic City** | 8 decks | Stands soft 17, peeks | Any two cards, after a split too | Any two 10-value cards, up to 4 hands | Insurance, late surrender |
| **European** | 2 decks | Stands soft 17, **no peek** | 9, 10 or 11 only, never after a split | Matching ranks only, 2 hands | No insurance, no surrender |

All three pay blackjack 3:2, give split Aces one card each, and do not count 21 on split Aces as a blackjack. The European dealer not peeking is the one that changes how a hand feels: a dealer blackjack there takes everything staked at a seat, money added by doubling or splitting included.

### At the table

- The table deals the moment the last seat fills, without waiting the window out. If nobody sat down it is simply dropped — nothing was staked, so nothing needs returning.
- The rule pick, the join window, the insurance window and each seat's turn all have their own deadline. A seat that runs its turn out stands automatically and play moves on.
- Its own minimum and maximum seat stake, separate from the table's.
- Taking a seat can count against hands per hour, and House Mode has global and per-player cooldowns of its own. The addiction cut applies to house wins, and addicted players wait twice the house user cooldown.
- The overlay curves the seats around the dealer, shows the open ones as dashed chips while the join window runs, then puts every hand side by side against the one dealer hand.
- `!stats` reports a House breakdown of its own, on top of the hands and wins it already counts.

---

## 📺 OBS Overlay

A self-contained browser source that draws the game live, installed into the overlay folder on its own — no copying files about. Point a Browser Source at `blackjack_overlay.html`; the background is transparent, so it sits over any scene.

- Real card faces, suits and colours for the dealer and every hand, with deal-in animations, totals, stakes and outcome badges.
- Duels put the two players either side of a VS badge with the live pot between them, split hands stacked per side pot.
- Prompt cards with a countdown ring for a pending challenge, a raise and a sudden-death offer. They count themselves down and disappear, so the overlay never sits on a dead prompt.
- Scale, font scale, card colours, backdrop colour and opacity, the dealer's name and per-element switches all live on the Overlay page.
- The folder can be a subfolder of the game directory or any full path on any drive, so the file can live with your other overlays.
- Works however OBS opens it: as a Local file, from a `file:///` address or off a web server, OBS 33 included. Set it up in OBS creates a 1300 × 1080 source.
- Version-stamped and self-updating, keeping your previous file alongside as a `.bak`.

<p align="center">
  <img alt="A House table on the overlay" src="Assets/screenshots/overlay-house.png" width="430">
  <img alt="A duel on the overlay" src="Assets/screenshots/overlay-duel.png" width="380">
</p>

---

## 🖥️ Settings window

Ten pages in a rail, in a dark and a light theme: the table rules, duels, House Mode, who may play, the overlay, every chat line with a live preview, statistics and leaderboard resets, a searchable log of every hand and a rule summary drawn from the current settings. When a new release is out, the window says so.

<p align="center">
  <img alt="The Table page" src="Assets/screenshots/settings-table.png" width="400">
  <img alt="The House page" src="Assets/screenshots/settings-house.png" width="400">
</p>

---

## 🏆 Leaderboard

`!allstats` ranks players on **combined lifetime net** — table profit plus duel winnings — so someone who only ever duels is on the board too. 🥇🥈🥉 for the top three, one to ten places, and the broadcaster and bot account can be hidden; they are matched by user id, so a rename cannot put them back on.

`!stats` reports one player's own record: hands, wins, blackjacks, busts, net, streaks, and the duel and House breakdowns.

Leaderboards reset on demand from the settings window, or by themselves **weekly or monthly** for running seasons.

---

# Get Started

[Setup Guide](https://github.com/aaskjer/Blackjack-Game/blob/main/Setup-Guide.md)

[Frequently Asked Questions](https://github.com/aaskjer/Blackjack-Game/blob/main/FAQ.md)

[Import String for streamer.bot](https://github.com/aaskjer/Blackjack-Game/blob/main/Import-String.md)

[Download](https://github.com/aaskjer/Blackjack-Game/releases)

---

# Credits & Links

[aaskjer on Twitch](https://twitch.tv/aaskjer)

[aaskjer.github.io](https://aaskjer.github.io/) 

[Blackjack Game on SB Discord](https://discord.com/channels/834650675224248362/1447648588694818836/1447648588694818836)

[Streamer.bot](https://streamer.bot) / [nate1280](https://www.patreon.com/c/nate1280/home)

YOU ♡
