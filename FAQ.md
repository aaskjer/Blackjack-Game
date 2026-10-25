# ❓ Blackjack Game — FAQ

---

## 🛠️ Setup

**Q: Which actions do I import?**

Three:

| Action | What it is |
|--------|---------|
| **Blackjack Game** | The game. Every gameplay command points here |
| **Blackjack Game - Settings** | Opens the settings window |
| **Blackjack Game - Stats** | Handles `stats` and `allstats` |

Point `stats` and `allstats` at the Stats action and everything else at the game action. The script matches on command ID, not on the word you type, so the trigger can be anything you like as long as the ID matches.

---

**Q: Where does everything live?**

Under your Streamer.bot base directory:

```
Chat Games/Blackjack Game/BJG-Settings.json              ← settings and chat messages
Chat Games/Blackjack Game/Overlay/blackjack_overlay.html ← the OBS browser source
Chat Games/Blackjack Game/Overlay/blackjack_state.json   ← the live state the overlay reads
Chat Games/Logs/BJG-log-yyyy-MM-dd.txt                   ← one log file per day
```

The overlay folder is the one piece you can move; see the Overlay section below. Nothing here needs editing by hand — the settings window writes all of it.

---

**Q: The settings window will not open.**

Look in the Streamer.bot log for the error, and [report it](https://github.com/aaskjer/Blackjack-Game/issues) if it looks like a defect.

A damaged `BJG-Settings.json` does not stop the window: it says so and opens with the defaults instead. Saving from there writes a fresh file — which also means anything that was still readable in the damaged one is overwritten, so move it aside first if you want to pick anything out of it.

---

**Q: How do I put everything back to defaults?**

The ↺ button at the bottom left of the settings window. It asks first, then rewrites `BJG-Settings.json` with the factory values and reopens. Your light or dark choice is kept.

That covers settings and chat messages only. Player statistics live in user variables and are untouched — the Stats page has its own reset for those.

Next to it, ⟳ resets only the page you are looking at, and writes nothing until you save.

---

**Q: I saved, but the game still behaves the old way.**

It should not. The game reads its configuration fresh on every command, so the next hand already has your changes. No restart.

The one exception is a hand that is already on the table: it keeps the rules it was dealt under.

---

**Q: Where is dark mode?**

The ☀️ / 🌙 button at the bottom left. The window reopens in the other theme, since the whole tree is rebuilt in the new palette rather than recoloured in place.

---

**Q: Any keyboard shortcuts?**

Ctrl+S saves. Ctrl+1 to Ctrl+9 jump to the first nine pages in the rail's order.

---

## 🎮 Playing

**Q: What are the commands?**

| Command | What it does |
|---------|--------------|
| `!blackjack <bet>` | Deals a hand |
| `hit` | Draw another card |
| `stand` | Stop and keep your total |
| `double` | Double the bet, take exactly one card, then stand |
| `split` | Split a starting pair into two hands |
| `insurance` | Cover half your bet against a dealer blackjack |
| `surrender` | Fold and keep half the bet |
| `quit` | Cancel and take the stake back - before your first move |
| `repeat` | Post your last game message again |
| `!duel @user <bet>` | Challenge another chatter |
| `accept` / `decline` | Answer whichever prompt is waiting on you |
| `!house [rules] [bet]` | Open a table |
| `join <bet>` / `deal` | Take a seat, or deal the table early |
| `stats` / `allstats` | Your own record, or the leaderboard |

Those are the defaults — your streamer may have bound them to other words.

---

**Q: What bet shortcuts are there?**

| Typed | Bets |
|-------|------|
| `all` / `allin` | The whole balance |
| `half` | Half of it |
| `quarter` / `25%` | A quarter |
| `50%`, any percentage | That much of the balance |
| `double` / `2x` | Twice the minimum bet |
| `random` / `rand` | Somewhere between a tenth of the balance and all of it |
| `max` / `maximum` | As much as the balance and the maximum bet allow |
| `1.5k` / `2m` | 1,500 and 2,000,000 |

`!blackjack` on its own — or with a word that is not one of these — uses the **default bet**.

Each shortcut confirms itself in chat with the figure it worked out, already cut to fit the minimum and maximum, so the number chat sees is the number that gets played.

---

**Q: Someone bet under the minimum, or over the maximum.**

Nothing breaks. Below the minimum is raised to it, and the player still needs the balance to cover it. Above the maximum is cut down.

Set a maximum of `0` for no ceiling at all. The same goes for the duel and seat maximums, and for hands per hour.

---

**Q: Can one player have two hands running?**

No. State is keyed per platform *and* per viewer, so one hand each — but any number of viewers can be mid-hand at once, and a Twitch hand and a YouTube hand never touch each other.

---

**Q: Someone dealt in and walked off. Is their stake stuck?**

No. **Turn timeout** on the Table page, 180 seconds by default and `0` for no limit, closes the hand out without anybody typing anything. The clock restarts on every action, so it only catches someone who really left.

| What they had done | What happens |
|-----------|--------|
| Nothing at all | The hand is cancelled and the stake returned, exactly as `quit` would. Controlled by **Refund a hand nobody played**, on by default |
| Anything at all | The hand stands and the dealer plays it out. There is a real hand on the table by then |

Every other deadline works the same way: an unanswered challenge, a raise, a sudden-death offer, the rule pick, the join window and a seat's turn all resolve themselves.

---

**Q: What if Streamer.bot closes, or I edit the code, mid-hand?**

Everything on the table is handed back. Hands live in the code's own memory and go with it when Streamer.bot tears it down, so the script settles up on the way out: unsettled solo hands, both sides of a live duel, the escrow behind an unanswered challenge and every occupied seat are credited back, and the overlay is cleared rather than left frozen on a hand nobody can finish.

Nothing is said in chat about it — the connection may already be gone — but the Streamer.bot log records what went back to whom.

---

**Q: What are the bonus hands?**

Both settle the moment they happen, whatever the dealer is holding:

- **5-card Charlie** — a fifth card without busting pays a 1.5x bonus.
- **Three sevens** — a third seven without busting pays a 2x bonus.

Neither waits for you to stand, so a sixth card cannot take one away again.

---

**Q: Insurance or surrender says it is not available.**

Both are **off by default**. Turn them on under **🃏 Table → Optional rules**. Beyond that:

- **Insurance** is only offered against an Ace, and has to be taken before hitting, standing, doubling or splitting.
- **Surrender** needs a fresh two-card hand, before any other move and before insurance.
- A **duel** has neither. They are dealer rules, and a duel has no dealer.

---

**Q: How does the tie rule work?**

**Break a tie with one card only**, on the Table page:

- **On** — the dealer draws exactly one card, and a tie that survives it goes to the dealer.
- **Off** — the dealer keeps drawing until the tie breaks one way or the other.

---

## ⚔️ Duels

**Q: How does a duel run?**

`!duel @user <bet>` calls someone out. They type `accept` or `decline`. Both stakes are held from the moment of the challenge and returned on a decline, a timeout or a draw. Both hands are dealt face up from one shared shoe, the challenger acts first, then the opponent, then the showdown. No dealer and no house: the points move from one viewer to the other.

The bet takes the same shortcuts as the table, and the order does not matter — `!duel 500 @user` works too.

---

**Q: Chat has scrolled past the hands. Can players see them again?**

`repeat` works inside a duel. Either player, any time: it posts both hands — listed per hand if someone has split — the pot, and who everyone is waiting on. It is read-only and never touches the turn clock.

Outside a duel the same command posts your last game message again.

---

**Q: Why does splitting or doubling in a duel need the opponent's permission?**

Because it lifts the stake for *both* of them, not just the one doing it. The opponent gets an accept or decline prompt, and declining refuses the raise only — it never ends the duel. A split that does not add a pot needs no agreement and happens at once.

---

**Q: What is sudden death?**

Off by default, in which case a tied duel simply returns both stakes. Switched on, a tie goes to double or nothing: both players have to accept, both stake again, and each draws one more card. A decline, a timeout or a shortfall returns everything. It repeats up to the round limit, then splits the pot.

It is only offered on an unsplit duel with a single pot. Duels with splits always push on a tie.

---

**Q: Someone walked off mid-duel.**

Set a **turn timeout** on the Duel page and the abandoned player stands automatically, on time, with nobody typing anything. The same goes for an unanswered challenge (stakes returned), an unanswered raise (play continues unchanged) and an unanswered sudden-death offer (everything returned).

The script runs a one-second clock while anything is live and stops it again once the last hand ends, so nothing ticks while the game is idle.

---

**Q: Does the addiction cut apply to duel winnings?**

Not by default. The points in a duel come from the other player rather than the house, so the cut would delete them from circulation instead of moving them. **Cut an addicted player's duel winnings** turns it on anyway.

---

## 🏠 House Mode

**Q: How does a table run?**

A moderator opens one with `!house`. If more than one rule set is on offer they pick from a menu — or name it directly, as in `!house vegas`. The join window opens, viewers take seats with `join <bet>`, and the table deals when the last seat fills or the window runs out. Whoever opened it can skip the wait with `deal`.

Then it plays like a real table: the dealer shows one card, each seat plays in turn, and once everyone is done the dealer plays a single hand that every seat is settled against.

---

**Q: Why will my table not open?**

Three reasons, in the order the game checks them:

- **House Mode is off** on the House page.
- **Free Game is on.** House Mode is played for real currency and refuses to open a table otherwise.
- **No rule set is ticked**, so there is nothing to play under.

---

**Q: What do the three rule sets change?**

| | Vegas Strip | Atlantic City | European |
|---|---|---|---|
| Shoe | 4 decks | 8 decks | 2 decks |
| Dealer peeks for blackjack | yes | yes | **no** |
| Double | any two cards, after a split too | any two cards, after a split too | 9, 10 or 11 only, never after a split |
| Split | any two 10-value cards | any two 10-value cards | matching ranks only |
| Hands from splitting | up to 4 | up to 4 | 2 |
| Insurance | yes | yes | no |
| Surrender | no | late surrender | no |

All three stand on soft 17, give split Aces one card each, and pay 3:2 on a blackjack. Click a name on the House page to read the full list.

The European dealer not peeking is the one that bites: a dealer blackjack there takes everything staked at a seat, money added by doubling or splitting included. That is how the real rule works, and it is why the other two are kinder.

---

**Q: Can a seat leave once the cards are out?**

`quit` returns the stake while the seat has not acted. After that the hand has to be played out, and the seat's turn is simply passed over.

---

**Q: Are House hands in my `!stats`?**

Yes, twice over. They count towards the hands, wins and net at the front of the line, and they get their own breakdown — `house 3W/1L at 4 tables` — so a regular at the table can see how much of the record came from there.

---

**Q: Who can open a table?**

Whoever the Streamer.bot trigger lets. The script does not check for itself, so set the permission on the `!house` and `deal` commands if you want them kept to moderators.

---

## 💰 Points and currency

**Q: Which points system does it use?**

It looks for one, in this order:

1. **StreamUP Currency Core** — reads the `currency_ProductSettings` global.
2. **tawmae LOYALTY POINTS** — reads the `tawmae_Settings_LOYALTY POINTS` global.
3. **Whatever you named** — the points variable and points name on the Access page.

Either of the first two overwrites the name on the Access page automatically. For anything else, name its variable there yourself.

---

**Q: How do people play for nothing?**

**Free Game** on the Table page. Nothing is staked and nothing is won, and every line about points switches to its Free Game wording so nothing mentions money that was never on the table — the duel challenge, accept, decline, expiry and sudden-death lines included.

House Mode stays shut while it is on.

---

**Q: What does the blackjack payout change?**

The bonus on a natural blackjack — 21 on the first two cards — paid on top of the returned stake:

| Option | What it means |
|--------|---------|
| **3:2** | Casino standard, the default |
| **6:5** | House edge |
| **2:1** | Generous |
| **1:1** | No bonus |

An ordinary win always pays 1:1, so double the stake comes back.

---

## 👥 Who can play

**Q: What is the Addicted list?**

Viewers on it play on, with two strings attached:

- **Their winnings are cut** by the payout cut on the Access page. Capped at 99%, so a win can never turn into a loss.
- **Their cooldown is doubled** — if a user cooldown is set, they always wait twice as long.

Add names under **Addicted viewers**, or whole Streamer.bot groups under **Addicted groups**. Either one is enough to flag someone.

---

**Q: How do I keep someone out entirely?**

**Access → Kept out.** Two lists, and being in either is enough:

| Field | Matches |
|-------|---------|
| **Blocked viewers** | Names, without regard to case. A leading `@` is ignored, so pasting `@someuser` works |
| **Blocked groups** | Streamer.bot user groups, matched exactly |

Unlike the Addicted list, a blocked viewer cannot start a hand, cannot start a duel, cannot accept one and cannot take a seat. Their commands are dropped without a reply, so retrying cannot spam chat.

---

## 🛡️ Limits and cooldowns

**Q: What is the difference between the two cooldowns?**

- **Everyone** — after any hand, nobody can start another until it expires.
- **Per player** — applies only to the viewer who just played.

There is a pair for each mode, six in all, and each mode counts separately: a duel never blocks someone from dealing in at the table. All six are handed to Streamer.bot's own cooldown system, and `0` turns one off.

---

**Q: What does hands per hour do?**

Caps how many hands one viewer may start within a rolling 60 minutes. `0` for no limit. Whether duels and seats count towards it is a switch on each of their pages.

---

**Q: The cooldown replies come from the wrong account.**

The Messages page has its own **bot account** and **send as /me** switches for cooldown replies, separate from the game's own messages. The leaderboard has a third pair, on the Stats page.

---

## 💬 Messages

**Q: Can I change what the game says?**

All 169 lines, on the Messages page. Pick one from the list, edit it, and the preview underneath shows how it will read in chat — filled in with sample values, sent from the account and in the style your switches are set to.

The chips under the editor are that message's placeholders; clicking one drops it in at the cursor. A placeholder the game does not fill for that particular line is not offered, so the preview cannot promise something chat will never see.

---

**Q: What happens if I empty a message?**

Nothing is sent for it. The preview says so.

---

**Q: Why do a couple of messages have `{0}` instead of a name?**

The two cooldown replies number their placeholders instead of naming them, and the order matters. Every `{` and `}` in them has to belong to a placeholder — a stray brace stops the message going out at all.

---

## 📺 OBS Overlay

**Q: How do I get it on stream?**

Two ways:

1. **Automatic** — **Overlay → Set it up in OBS**. Give it your OBS connection number, a scene name and a source name, then press the button. It does nothing if a source of that name is already there.
2. **By hand** — add a Browser Source in OBS pointing at `blackjack_overlay.html` as a **Local File**.

The background is transparent, so it layers over any scene.

---

**Q: Can the overlay live somewhere else?**

Yes. **Overlay folder** takes either form:

| You type | Where it goes |
|----------|---------------|
| `Overlay` | `Chat Games\Blackjack Game\Overlay\` |
| `D:\Streaming\Overlays\Blackjack` | Exactly that, on any drive |

The line under the field shows the full path it resolves to, and the ↗ button opens it. The folder is created if it is missing, and the overlay file is written there the next time the game runs. Repoint your browser source afterwards.

---

**Q: The overlay is blank, or will not update.**

- Check the browser source points at the **local file**, not a URL.
- Check the path under **Overlay folder** against what the browser source points at. If you changed the folder, the old file is still sitting where it was.
- The overlay reads `blackjack_state.json` out of its own folder, so both files have to sit together. Change the setting rather than moving the HTML by hand.
- Refresh the browser source cache in OBS after the first install.

---

**Q: Can I hide it during Free Game?**

**Hide during Free Game**, on the Overlay page. There are switches for the totals, stakes, outcome badges, insurance line and animations too, plus scale, font scale, card colours, backdrop colour and opacity, and the dealer's name.

---

**Q: A new version shipped. Do I lose my overlay changes?**

The overlay file itself is version-stamped and rewritten when a newer one ships, with your previous file kept alongside as a `.bak`. Your *settings* live in `BJG-Settings.json` and an overlay update never touches them.

---

## 🏆 Statistics and leaderboard

**Q: Where are the statistics kept?**

In 50 persisted Streamer.bot **user variables**, all named `bjg_*`. There is no stats file. Any other Streamer.bot action can read a player's record straight out of them — `bjg_duelsWon`, `bjg_netProfit`, `bjg_longestWinStreak` — without going through this game.

---

**Q: How is `!allstats` ranked?**

On **combined lifetime net**: table profit (`bjg_netProfit`) plus duel winnings (`bjg_duelNet`), so someone who only ever duels is on the board too. One to ten places, with optional 🥇🥈🥉 for the top three.

---

**Q: How do I keep myself and my bot off the board?**

**Stats → Who shows up**: **hide the broadcaster** and **hide the bot account**. Both are matched by user id on the platform the command came from, so a rename cannot put them back on. Their statistics are still tracked — only the listing leaves them out.

---

**Q: How do I start a new season?**

Set **a reset clears** first:

| Scope | What goes |
|-------|--------|
| **The leaderboard totals only** | Bets, winnings, net, duel points, biggest win and loss. Hands played, blackjacks, busts, streaks and the duel and House records stay |
| **Every stat there is** | Every `bjg_*` variable for every viewer, with nothing to recover |

Then either press **reset the leaderboard now**, which spells out exactly what it is about to clear, or set **automatically** to weekly or monthly.

---

**Q: Does turning on automatic resets wipe everything straight away?**

No. The current period is noted first and the reset waits for the next boundary. Streamer.bot gives the script no scheduler, so the rollover is noticed the first time a Blackjack or stats command runs after it — not on the stroke of midnight.

---

## 🌐 Platforms

**Q: Which ones work?**

Twitch, YouTube and Kick. There is nothing to switch on here: the game answers wherever its commands are set to fire, and works out
which platform that was on its own, so points and statistics land on the right account.

Which platforms a command fires on is Streamer.bot's own setting, on the trigger. Leave Kick off there and no Kick viewer can
deal in — the game never needs to know about it.

---

**Q: Can it run on several at once?**

Yes. Hands, points, statistics and leaderboards are all tracked per platform, so a Twitch hand and a Kick hand can be live together without touching each other.

---

**Q: `/me` is not working on YouTube or Kick.**

Neither has it. **Send as /me** is Twitch only; everywhere else the message goes out as plain chat regardless.

---

## 📜 Logging

**Q: What goes in the log?**

Every settled hand — win, loss, blackjack, bust, surrender, insurance, quit, timeout, and the duel and House results — with the time, viewer, platform, trigger, stake, result and the balance before and after. One file per day: `Chat Games/Logs/BJG-log-yyyy-MM-dd.txt`.

Turn it off with **write a log file per day**, on the Log page.

---

**Q: Can I read it without leaving Streamer.bot?**

The **📜 Log** page shows it in a sortable table, with a search box and filters for the result, the mode, the platform and the day. Picking a row shows the whole line it came from, and ↗ opens the folder.

---

# Is this AI slop?

Partly, and it is easier to say so than to pretend otherwise. The script was written with help from AI and with input from the Streamer.bot community, and I am not a developer by trade.

What I can say is that I put real time into it. The goal was a chat game that holds up under a live chat rather than a demo that falls over, and I enjoyed building it — I used it to learn, and put the AI to something I actually wanted to exist.

If the use of AI puts you off the project, I understand that completely, and I would rather be straight about it than hide it.

---

AI writes bugs, and so do I. I test and fix what I find, but things get through. If you hit one, or you have something to say about it, I would rather hear it: [report anything that looks wrong](https://github.com/aaskjer/Blackjack-Game/issues).
