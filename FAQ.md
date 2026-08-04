# ❓ Blackjack Game — FAQ

---

**Q: Where are the files stored?**

Everything lives under your Streamer.bot base directory:

```
Chat Games/Blackjack Game/BJG-Settings.json              ← all settings and messages
Chat Games/Blackjack Game/Overlay/blackjack_overlay.html ← OBS browser source
Chat Games/Blackjack Game/Overlay/blackjack_state.json   ← live state the overlay reads
Chat Games/Logs/BJG-log-yyyy-MM-dd.txt                   ← daily action log
```

You should never need to edit these by hand — use the built-in Settings GUI instead.

---

**Q: The Settings GUI won't open / crashes immediately. What do I do?**

- Check the Streamer.bot log for error details and please report it [here](https://github.com/aaskjer/Blackjack-Game/issues).
- If `BJG-Settings.json` is malformed, the window will fail to open. Delete the file and restart — it is recreated with defaults on the next run.

---

**Q: How do I reset all settings to defaults?**

Open the Settings GUI and click **Reset to Defaults** (red button, bottom-left). It asks for confirmation, then rewrites `BJG-Settings.json` with factory values and reopens the window. Your dark/light mode choice is kept.

Note: this resets *settings and messages only*. Player statistics are stored separately in user variables and are not touched — use **Reset Now** on the Leaderboard tab for those.

---

**Q: I saved settings but nothing changed in-game. Why?**

Clicking **Save** writes the JSON to disk. The game reads its config fresh on every command, so the next hand picks up your changes immediately. No Streamer.bot restart needed.

---

**Q: Where do I turn on dark mode?**

The 🌙 / ☀️ button next to the Save row at the bottom of the settings window. The window reopens with the new theme applied.

---

## 🎮 Playing

**Q: What are the in-chat commands?**

| Command | Who | What it does |
|---------|-----|--------------|
| `!blackjack <bet>` | Everyone | Start a new hand |
| `hit` | Everyone | Draw another card |
| `stand` | Everyone | Lock in your hand |
| `double` | Everyone | Double your bet, take exactly one card, then stand |
| `split` | Everyone | Split a starting pair into two hands |
| `insurance` | Everyone | Insure half your bet against a dealer blackjack |
| `surrender` | Everyone | Forfeit half your bet and end the hand |
| `quit` | Moderator | Cancel and get refunded — Moderators only for testing purposes |
| `repeat` | Everyone | Resend your last game message |
| `!duel @user <bet>` | Everyone | Challenge another chatter |
| `accept` / `decline` | Everyone | Answer whichever prompt is waiting on you |
| `stats` / `allstats` | Everyone | Personal stats / leaderboard |

Command names depend on what you configured in Streamer.bot — the above are the defaults.

---

**Q: What bet shortcuts can players use?**

| Input | Bets |
|-------|------|
| `all` / `allin` | Entire balance |
| `half` | Half the balance |
| `quarter` / `25%` | A quarter of the balance |
| `50%` (any %) | That percentage of the balance |
| `double` / `2x` | Double the minimum bet |
| `random` / `rand` | A random amount within a sensible range |
| `max` / `maximum` | As much as allowed, capped by balance and Maximum Bet |
| `1.5k` / `2m` | 1,500 / 2,000,000 |

Typing `!blackjack` with no bet — or with something unrecognised — uses the configured **Default Bet**.

---

**Q: A bet below the minimum / above the maximum was placed. What happens?**

Nothing breaks. Bets below **Minimum Bet** are raised up to it (the player still needs the balance to cover it), and bets above **Maximum Bet** are clamped down. Set Maximum Bet to `9999999` for effectively unlimited.

---

**Q: Can a player have two hands going at once?**

No. Game state is keyed per platform *and* per user, so each player has one active hand at a time — but any number of players can be mid-hand simultaneously, and a Twitch round and a YouTube round never interfere with each other.

---

**Q: What are the special wins?**

Both are automatic and independent of the dealer's hand:

- **5-Card Charlie** — 5 cards without busting, pays a 1.5x bonus.
- **Three 7s** — three or more 7s without busting, pays a 2x bonus.

---

**Q: Insurance / surrender says it's unavailable. Why?**

Both are **off by default**. Turn them on in the Settings GUI → **🃏 Game → Optional Rules**. Beyond that:

- **Insurance** is only offered when the dealer's face-up card is an Ace, and must be taken before hitting, standing, doubling, or splitting.
- **Surrender** is only available on a fresh 2-card hand, before any other action.
- Neither is available in a **duel** — they are dealer rules, and a duel has no dealer.

---

**Q: How does the tie rule work?**

Two behaviours, toggled by **Tie System: draw only 1 tie-break card** on the Game tab:

- **On** — the dealer draws exactly one card on a tie and the round ends there. A tie that isn't broken counts as a dealer win.
- **Off** — the dealer keeps drawing until the tie is actually broken.

---

## ⚔️ Duels

**Q: How does a duel work?**

`!duel @user <bet>` challenges a chatter. The opponent types `accept` or `decline`. Both stakes are held from the moment the challenge is made and refunded on decline, timeout, or a push. Both hands are dealt face up from one shared deck, the challenger acts first, then the opponent, then a showdown. There is no dealer and no house — points move directly between the two players.

The bet accepts all the same shortcuts as `!blackjack`, and order doesn't matter — `!duel 500 @user` works too.

---

**Q: Why does splitting or doubling in a duel need the opponent's permission?**

Because it raises the stakes for *both* players, not just the one doing it. The opponent gets an accept/decline prompt — declining refuses the raise only, it never ends the duel. A split that doesn't increase the pot count happens instantly with no prompt.

---

**Q: What is Sudden Death?**

Off by default, in which case a tied duel simply refunds both players. When enabled, a tie instead offers double-or-nothing: both players must accept, both stake another bet, and each draws one more card. Any decline, timeout, or shortfall refunds everything. Repeats up to the configured round limit, then splits the pot.

It's only offered on an unsplit duel with a single pot — duels involving splits always just push on a tie.

---

**Q: Someone walked away mid-duel. Does it stay stuck forever?**

No. Set a **Turn Timeout** on the Duel tab and the abandoned player is automatically stood. Streamer.bot gives the script no scheduler, so timeouts are checked whenever any Blackjack command runs — an abandoned duel resolves on the next command rather than exactly on the second.

---

**Q: Does the Addicted payout cut apply to duel wins?**

Not by default. In a duel the points come from the other player rather than the house, so the cut would quietly delete points from circulation instead of moving them. You can turn it on with **Apply Addiction Fee to Duel Wins** if you want it anyway.

---

## 💰 Points & Currency

**Q: What points system does the game use?**

The game auto-detects your currency in this priority order:

1. **StreamUP Currency Core** — reads the `currency_ProductSettings` global.
2. **tawmae LOYALTY POINTS** — reads the `tawmae_Settings_LOYALTY POINTS` global.
3. **Config fallback** — uses `Points Var` / `Points Name` from the Game tab.

If you use a custom currency with a different variable name, set **Points Var** and **Points Name** in Settings GUI → **🃏 Game → Points**. Points Name is overwritten automatically if Currency Core is detected.

---

**Q: How do I let people play without wagering anything?**

Enable **Free Game** on the Game tab. Nobody wins or loses currency, and the game switches to a separate set of Free Game messages so nothing refers to points that were never wagered — including the duel challenge, accept, decline, expiry, and sudden-death lines.

---

**Q: What does the Blackjack Payout setting do?**

It sets the bonus multiplier for a natural blackjack (21 on the first two cards), paid on top of the returned stake:

| Option | Meaning |
|--------|---------|
| **3:2** | Standard casino (default) |
| **6:5** | House advantage |
| **2:1** | Generous |
| **1:1** | No bonus |

A regular win always pays 1:1 — the player gets double their bet back.

---

## 👥 Role Management

**Q: What is the "Addicted" list?**

Users on it can still play, but with two penalties:

- **Payout cut** — their winnings are reduced by **Addicted Payout Cut %** (hard-capped at 99%, so a win can never become a net loss).
- **Doubled cooldown** — if a User Cooldown is set, they always wait twice as long between games.

Add individual usernames under **Addicted Users** (comma-separated, matched case-insensitively) or whole Streamer.bot groups under **Addicted Groups** (must match the group name exactly). Matching either is enough to flag someone.

---

**Q: How do I stop certain users from playing at all?**

Settings GUI → **⚙️ General → Blocked Groups**. Comma-separated list of Streamer.bot user groups, matched exactly. Unlike the Addicted list, blocked users can't start a game or a duel at all.

---

## 🛡️ Limits & Cooldowns

**Q: What is the difference between Global Cooldown and User Cooldown?**

- **Global Cooldown** — after any `!blackjack`, nobody can start another game until it expires.
- **User Cooldown** — applies only to the individual player who just played.

Both are set on the **⏱️ Cooldown** tab and applied directly to the Streamer.bot command via `CPH.CommandSetGlobalCooldownDuration` / `CPH.CommandSetUserCooldownDuration`. `0` disables either one.

---

**Q: What does Max Games/Hour do?**

It caps how many games a single player can start on a rolling 60-minute window. `0` = unlimited. Whether duels count toward it is a separate toggle on the Duel tab.

---

**Q: Cooldown messages come from the wrong account. How do I change that?**

The Cooldown tab has its own **Use Bot for Cooldown** and **Send As Action** toggles, independent of the general ones on the General tab. The leaderboard has its own pair too, on the Leaderboard tab.

---

## 📺 OBS Overlay

**Q: How do I get the overlay on stream?**

Two ways:

1. **Automatic** — Settings GUI → **📺 OBS Overlay → OBS Auto-Setup**. Enter your OBS connection ID, scene name, and source name, then click **🔧 Setup Overlay in OBS**. It does nothing if a source with that name already exists.
2. **Manual** — add a Browser Source in OBS pointing at `blackjack_overlay.html` as a **Local File**.

The background is transparent, so it layers over any scene.

---

**Q: The overlay is blank / not updating.**

- Make sure the browser source points at the **local file**, not a URL.
- The overlay reads `blackjack_state.json` from the same folder — if you copied the HTML somewhere else, it can't find it. Keep both files together, or just change the **Overlay Folder** setting instead of moving files.
- Refresh the browser source cache in OBS after the first install.

---

**Q: Can I hide the overlay during Free Game?**

Yes — **Hide On Free Game** on the OBS Overlay tab. There are also individual toggles for hand values, bet amounts, outcome badges, insurance display, and animations, plus scale, font scale, card colours, background colour/opacity, and the dealer's display name.

---

**Q: A new version shipped — do I lose my overlay customisations?**

No. The overlay file is version-stamped and rewritten automatically when a newer version ships, and your previous file is preserved as a `.bak` alongside it. Your *settings* live in `BJG-Settings.json` and are never touched by an overlay update.

---

## 🏆 Statistics & Leaderboard

**Q: Where are player stats stored?**

In persisted Streamer.bot **user variables** — 44 of them, all prefixed `bjg_`. There is no stats file. This means any other Streamer.bot action can read a player's record directly, e.g. `bjg_duelsWon`, `bjg_netProfit`, or `bjg_longestWinStreak`, without involving this game at all.

---

**Q: How is the `!allstats` leaderboard ranked?**

On **combined lifetime net** — dealer-game profit (`bjg_netProfit`) plus duel winnings (`bjg_duelNet`) — so players who only ever duel are included. Size is configurable from 1 to 10 places, with optional 🥇🥈🥉 medals for the top three.

---

**Q: How do I keep myself and my bot off the leaderboard?**

Settings GUI → **🏆 Leaderboard → Who Appears**: **Hide Broadcaster** and **Hide Bot Account**. Both are matched by user id on the platform the command came from, so a name change won't put them back on the board. Their stats are still tracked — they're only hidden from the list.

---

**Q: How do I reset the leaderboard for a new season?**

Set **What a Reset Clears** first:

| Scope | Clears |
|-------|--------|
| **Leaderboard totals only** | Bets, winnings, net, duel points, biggest win/loss. Games played, blackjacks, busts, streaks and duel records are kept. |
| **All stats (full wipe)** | Every `bjg_*` variable for every user. Nothing is recoverable. |

Then either click **Reset Now** (asks for confirmation and spells out exactly what will be cleared), or set **Automatic Reset** to weekly or monthly.

---

**Q: Does turning on automatic resets wipe everything immediately?**

No. The current period is recorded first and the reset happens at the next boundary. Streamer.bot gives the script no scheduler, so the rollover is detected the first time any Blackjack or stats command runs after the boundary — not exactly at midnight.

---

## 🌐 Multi-Platform

**Q: Which platforms are supported?**

Twitch, YouTube, and Kick. Enable the ones you stream on in Settings GUI → **⚙️ General → Select Platform(s)**. Commands from unticked platforms are ignored, and the game determines the platform automatically from the incoming event.

---

**Q: Can the game run on several platforms at once?**

Yes. Game state, points, stats, and leaderboards are all tracked per-platform, so a Twitch hand and a Kick hand can be live at the same time without interfering.

---

**Q: `/me` messages aren't working on YouTube or Kick.**

**Send As Action** is a Twitch-only feature. On other platforms the message is sent as plain chat regardless of the setting.

---

## 📜 Logging

**Q: What gets written to the log?**

Every resolved action — win, loss, blackjack, bust, surrender, insurance, quit, and duel results — with time, user, platform, trigger, bet, result, and the balance before and after. One file per day: `Chat Games/Logs/BJG-log-yyyy-MM-dd.txt`.

Turn it off with **Write Log** on the General tab.

---

**Q: How do I read the log without leaving Streamer.bot?**

Settings GUI → **➕ More → 📜 Action Log**. It shows the current day's entries in a sortable table with search and filtering, and can open the Logs folder directly.

---

## 🔔 Update Notifications

**Q: How do I know when a new version is available?**

Opening the Settings GUI checks GitHub for the latest release tag. If it's newer than what's installed, a popup offers to open the releases page.

**Q: Can I check for updates without opening the Settings window?**

Partially — You still have to trigger the `Test` trigger but if you press `Yes` in the update notification popup, the check won't open the GUI but the GitHub page instead.

---

# Is Blackjack Game an AI Slop?
Partially it is. This script has been developed with input from the streamer.bot community and is support by AI.
But i spend a lot of time putting heart and soul in it and my goal was to create a robust and valid moderation tool for everyone and easy to use.
I understand that people, especially IT savvy people, will dislike the project because of the use of AI and i absolutely understand and support their point of view.
But i had a lot of fun making it as with all my other projects, so i used it to "learn" coding and used AI for something valuable.

---

AI can create bugs and i am not a developer in classical terms. But i spend a reasonable amount of time fixing any bugs that occured while testing.
If you still find bugs or have something to say, please let me hear it :)
Report anything that looks wrong [here](https://github.com/aaskjer/Blackjack-Game/issues).
