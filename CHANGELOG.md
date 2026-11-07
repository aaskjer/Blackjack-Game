# Changelog

Everything worth knowing about, newest first.

## [17.3.0] — Unreleased

### 📺 Try the overlay without playing

- **Try the demo** on the Overlay page opens the overlay in your browser and plays made-up hands by itself, without
  Streamer.bot: a solo hand, a blackjack, a duel challenge with its countdown, a duel and a House table, in the look
  your saved settings give it. In OBS, `&demo=1` at the end of the source's URL does the same, to place and size the
  source before anyone plays.
- **Show a sample round** plays one made-up hand on the overlay for about ten seconds, the one in OBS included, then
  clears it again. Nobody's points move, and while a hand, duel or House table is running the overlay is left to it.
- The overlay is version 15 and replaces an older copy on the next command, keeping that one as a `.bak`.

### Changed

- **The settings action is UIFramework's stub 0.2.0.** When the game action hangs, the settings window waits 30
  seconds for its answer, then carries on and can be closed; before, it waited for good. A game that has not
  described itself for a minute no longer locks the settings until Streamer.bot restarts: the next run opens the
  window.
- The host block is UIFramework's current one (0.10.0). It names every type in full, so it no longer depends on the
  action's `using` lines, and it can declare a tour of the window. Nothing changes in the window.

## [17.2.0] — 2026-11-06

### 🆓 Free Game from chat

- **`!SetFreeBlackjack` and `!EndFreeBlackjack`.** Two new commands in the import (moderators, disabled until you
  enable them) switch Free Game from chat. The switch is written into the settings file, so the window's pill shows
  it, and chat gets the new Started and Ended lines (Messages page, Free Game). An action of your own can do the same
  with an Execute C# Method on `StartFreeGame` or `EndFreeGame` of the game action.
- **Two more custom triggers.** `BJG: Free Game Start` and `BJG: Free Game End` fire when Free Game is switched,
  from chat or by those methods.

### Fixed

- The Stats page no longer names a `statsTable` field that no setting matches, which the window reported as a
  schema problem.
- The overlay's duel prompt names the commands chat types, `!accept` / `!decline`, instead of `accept` / `decline`.
  The overlay is version 14 and replaces an older copy on the next command, keeping that one as a `.bak`.

### Changed

- The host block in `Blackjack Game` and `Blackjack Leaderboard` is UIFramework's current one (0.9.7). Nothing
  changes in the window; the block can now declare masked fields and read-only list columns.

## [17.1.0] — 2026-11-06

### 🔔 For your own actions

- **Custom triggers.** Every settled hand fires a trigger (Custom > Chat Game > Blackjack Game): `BJG: Hand Won`,
  `BJG: Hand Lost` or `BJG: Hand Push` for a hand at the table, `BJG: Blackjack` on top of Hand Won for a natural,
  `BJG: Duel Settled` once per duellist, `BJG: House Hand Settled` once per seat, and `BJG: Hand Settled` for every
  one of them, insurance, surrender, quit and timeout included. The trigger carries the hand as arguments:
  `handOutcome` (the log's result: WIN, BUST, DUEL PUSH, HOUSE BLACKJACK, …), `handUser`, `handPlatform`, `handMode`
  (table, duel or house), `handTrigger`, `handBet`, `handChange`, `handPreviousPoints`, `handNewPoints`,
  `handCurrency`. `GetLastHandResult`, run with an Execute C# Method sub-action on the game action, hands the same
  arguments to any other action.

### 🪟 Settings window

- **German.** With the window's language set to `de` (the pencil, then *Sprache*), every setting, section, page
  and message of the window reads in German. The help and about pages stay English.
- **The settings action is UIFramework's generic stub.** `[BJG] - Settings` carries no layout and no code of its
  own any more: the game action declares its window (pages, the log, help and about, the cooldowns of `!blackjack`,
  `!duel` and `!house`) beside its settings table and answers the window itself. *Reset the leaderboard now* and
  *Set it up in OBS* now answer with what happened instead of "queued". Needs UIFramework 0.9.2 or newer; the
  stub fetches it.

## [17.0.0] — 2026-11-01

The settings window moved into [UIFramework for streamer.bot](https://github.com/aaskjer/UIFramework-Streamer.bot),
a window shared by all of these chat games. The game plays exactly as before; what changed is how its settings are
shown and stored.

### 🪟 Settings window

- **One DLL, downloaded on first open.** `[BJG] - Settings` is now a small stub. The first time you open the
  window it asks whether to download `UIFramework.dll` from GitHub (about 190 KB, checked against its published
  SHA-256) or to show the folder it belongs in, `UIFramework\lib\<version>\` next to Streamer.bot. Nothing is
  fetched without that click.
- **New releases are offered, not forced.** When a newer UIFramework is published, opening the window asks once:
  *Yes* downloads it (used right away, or from the next start of Streamer.bot if the old one is already loaded),
  *No* asks again next time, *Cancel* skips that version. `"autoUpdate": false` in `uiframework.layout.json`
  turns the check off.
- **The window is built from the game's own settings table.** The game declares every setting once and answers
  the window's `uiframework.describe` request with it, so the window and the game can never disagree about a key,
  a default or a range again.
- **Customise mode.** The pencil at the bottom left lets you rename, reorder, hide and move pages and fields by
  drag and drop or from the context menu, pick the accent colour and the language of the convention labels. All of
  it lives in `uiframework.layout.json` next to `BJG-Settings.json` and survives every import of the game.
- **Help and About** sit in their own group at the bottom of the rail.

### ⚙️ Settings file

- `BJG-Settings.json` keys are camelCase now (`useBot`, `minBet`, `addictedPayoutCut` as a percent). An existing
  file keeps working: the old names are read through aliases, and the first save writes the new ones.
- Light/dark and the rainbow mode live in `uiframework.layout.json` and are shared with every other window built on
  UIFramework. An existing `DarkMode` is taken over on the first open.
- Values typed out of range are pulled back into range when the game reads them, not only when the window saves.

### Removed

- The window's own WPF code and update check (about 4,800 lines) left the import; the DLL carries them now.

## [16.3.1] — 2026-10-31

### 💬 Chat

- **A solo win posts its final summary once.** When the dealer bust, when the player's hand was
  higher, and when the player won a tie-break, chat got "Final - {user}: … vs dealer …" twice in a
  row: once on its own, then again in front of the win message and the balance. Only the second
  line is left, in points mode and in a free game alike. No message text or setting changes.

### 🪟 Closing the window

- **The title bar's X asks before it discards.** Closing the window with the X threw away every
  unsaved change without a word; only the Cancel button asked. Both now ask the same question,
  "Discard N unsaved changes?", and No keeps the window open. Save & Exit, Reset all and the theme
  switch close as before.
- **A recompile or shutdown closes an open window.** Streamer.bot disposes the action's code when it
  recompiles or exits; the settings window used to stay up, still writing `BJG-Settings.json` from a
  build nothing else runs. It is now closed on the way out, without the question - nobody is there
  to answer it.

## [16.3.0] — 2026-10-27

### 🎨 A new colour picker

- A click on the swatch next to an overlay colour, or on the 🎨 button beside it, opens the picker right
  under the field, instead of a separate window with a colour wheel: a field for saturation and
  brightness and a hue bar, both to click or drag, sliders for red, green and blue (the mouse wheel
  nudges them by one), and a row of ready colours. Every change goes straight into the hex box, and
  typing a hex code moves the picker along. Done or a click elsewhere closes it.

## [16.2.1] — 2026-10-25

### 💾 The settings file

`BJG-Settings.json` was written straight over itself, and a read that failed put up a dialog from
whatever thread happened to be running. Both are fixed; no setting changes, and the file is read as
it stands.

- **Saving is atomic.** The file is written beside the live one and swapped in, so a crash halfway
  through can no longer leave a truncated `BJG-Settings.json` behind — the game reads it on every
  hand. Every save also leaves `BJG-Settings.json.bak`, the file as it was before that save.
- **A damaged file is reported in the window** and kept as `BJG-Settings.json.broken`. The window
  shows the settings from before the last save, or the defaults when there is no readable backup.
  Until now a read error raised an "Error loading settings" box from Streamer.bot's own thread —
  a modal dialog with no window behind it, holding the action until somebody found it — and then
  carried on with the defaults, which the next save wrote over the damaged file without a word.
- **A file that cannot be opened at all no longer opens the window.** It said "damaged" and showed
  the defaults, so a save would overwrite a perfectly good file that was merely locked.
- A settings file the game script is holding open is waited out instead of failing on the first try.

### ⏱️ Cooldowns and the window

- The cooldowns of `!blackjack`, `!duel` and `!house` were set from the settings window's own
  thread. All six are now handed to Streamer.bot's thread while the window is open, as one set, so
  they can never be applied as a mix of two saves.
- **Only one settings window opens.** A second trigger while it was up opened another one with its
  own copy of the settings, and whichever was saved last won. It now brings the open window to the
  front.
- Switching pages redraws only the page in front, instead of every control in the window.

## [16.2.0] — 2026-10-25

### 📺 The overlay and OBS 33

OBS 33 tightened how a browser source may read files from disk. Its local-file handler — the one a
"Local file" source runs through — now refuses any request unless the page itself was opened
through it, and the OBS team has said they may remove that handler entirely later on. The overlay
fetched its table state through exactly that route.

- **The state now arrives as a script.** The game writes `blackjack_state.js` next to the JSON, and
  the overlay loads it with a script tag. That works however OBS opened the page — as a Local File,
  from a `file:///` address, or off a web server — where `fetch()` managed only some of those. The
  JSON is still written, and still read as a fallback wherever the script cannot be.
- **Both files are written whole.** The game writes beside the file and swaps it in, so OBS never
  catches one half-written; if OBS happens to be holding the old copy open, it writes in place and
  the overlay simply reads again a moment later.
- **The overlay catches up the instant it is shown.** OBS slows a hidden source's timers right
  down; the overlay now listens for OBS telling it the source became visible or went live and
  reads the state straight away.
- **When it cannot read the state, the OBS log says so** — once after ten seconds, then at most once
  a minute, with the address the page was loaded from. It is the only kind of message OBS keeps
  from a browser source.
- **Set it up in OBS creates a 1300 × 1080 source** instead of 700 × 500.
- The overlay is version 13 and replaces an older copy on the next command, keeping it as a `.bak`.

### 🃏 A shoe that carries on (new)

**Decks in the shoe** and **Reshuffle at** have been settings for a long time, and until now neither
did much: every hand built its own shoe, dealt three or four cards from it and threw it away. Eight
decks played exactly like one, and the reshuffle threshold never came near being reached.

Now there is one shoe per platform, shared by every hand and duel running there, the way a table
deals every seat from the same shoe. A card that has been played goes to the discard pile and stays
out until the shoe runs down to the threshold, at which point the played cards go back in and it is
shuffled. What is left genuinely shifts as the night goes on, and anyone paying attention can tell.

- Cards sitting in a hand are in neither pile, so a reshuffle mid-round cannot deal the same card
  twice. A duel hands its cards back the same way a table hand does.
- Change the deck count or the threshold and the next hand starts a fresh shoe. A hand already in
  progress keeps the one it was dealt from, so its cards go back where they came from.
- A House table still has its own shoe: the rule set picks the deck count there, not the Table page.
- The old "the deck couldn't deal a hand" check is gone. It counted the cards left and refused a
  hand when the shoe was nearly out — which a shoe that reshuffles on demand never is. That message
  now covers the one case it can still describe, a deal that threw.

### 🎨 Settings window, rebuilt

Twenty-eight entries in six collapsible groups are now ten pages in a flat rail, the shape Classic
Chat Gamble, the Slot Machine, Trivia and Hangman all use.

- The fifteen message pages are one **Messages** page: pick a line from a grouped list, edit it, and
  a **live chat preview** shows how it reads — filled in with sample values, from the account and in
  the style your switches are set to. Placeholder chips insert at the cursor, and a placeholder the
  game does not fill for that line is not offered.
- What used to be a General page now sits with the rules, the viewers or the messages it governs.
- **How to play** reads the live controls rather than the saved file, so its rule summary matches
  what the window currently says.
- The **Log** page filters by result, mode, platform and day, and shows the raw line behind a row.
- Ctrl+S saves, Ctrl+1 to Ctrl+9 jump between pages, and the footer counts unsaved changes.
- ⟳ resets a single page without writing anything.

### 🎨 Settings window and overlay

- **The colour wheel takes a click.** Pick a hue and a saturation straight off it, and drag to keep
  picking; the three sliders follow along instead of sitting on stale numbers.
- **The page icons were nearly invisible on the dark theme.** The label beside each one was given a
  colour and the glyph was not, so it fell back to black.
- **The overlay groups its amounts** the way chat does, with the separator from the settings window,
  rather than printing `1234567` next to chat's `1.234.567`.
- **A display name can no longer break the overlay's layout.** Names and the currency name end up
  inside the page's own markup; a YouTube name may hold any character at all, and an unescaped
  angle bracket took the hand apart. The challenge prompt, which names both players while a duel
  waits for its answer, had the same gap.
- **The settings window's rail fits the window again.** Ten entries were a little too tall for the
  default height, so the rail scrolled - with a scroll bar the dark theme does not colour.

### 🔧 Fixed

- **The duel help named a command that does not fire.** The command the import creates is
  `!bjduel`, but the reply to a challenge without a name and the Duel page both said `!duel`. A
  saved copy of that reply keeps its wording; reset it on the Messages page to get the new one.
- **Stakes no longer vanish when the code is torn down.** Hands live in the script's own memory, so
  closing Streamer.bot or editing the code used to take every open stake with it. The game now
  settles up on the way out: unsettled solo hands, both sides of a live duel, the escrow behind an
  unanswered challenge and every occupied seat are credited back, the overlay is cleared instead of
  freezing on a hand nobody can finish, and the one-second clock stops rather than being left
  running alongside the new copy's.
- **A maximum bet can be lifted again.** The ceilings showed `long.MaxValue` as `9999999`, and
  saving wrote that number back — so "no ceiling" was gone for good after the first save. The three
  maximums and hands per hour now read `0` as no limit, the way they always should have.
- **Three default messages said `@@username`.** The settings window and the game disagreed on
  whether the text or the code supplies the `@`. On a fresh install, before anything had been saved,
  the cooldown reply, the failed-refund notice and the no-stats reply all doubled it up.
- **House statistics survived a full wipe.** The variable list exists in three places, and two
  copies were missing House Mode's six, so "every stat there is" quietly left them standing.
- **`!stats` ignored House Mode.** A viewer who had only ever taken a seat was told they had never
  played, and none of the House figures were reported.
- **Bet shortcuts announced the wrong figure.** `all` posted the whole balance before the maximum
  bet cut it down, so chat was told a number the hand was never played for.
- **Kick fell back to Twitch.** Where Streamer.bot does not supply the command source, the platform
  is worked out from the event — and `KickChatMessage` was not among the ones it knew, so a Kick
  viewer's points and statistics were booked against Twitch. The stats action's version of that
  fallback could never match anything at all.
- **A 5-card Charlie could be lost again.** It was only checked when the player stood, so a sixth
  card took the bonus away. Both bonus hands now settle the moment they happen — including three
  sevens made by doubling, which was missed entirely.
- **Saving trimmed three messages.** A space at either end of a message is deliberate, because the
  game glues several of them together; opening the window and saving ran two sentences into each other.
- **The command IDs in this file were wrong**, so anyone building the duel commands from the setup
  steps below got three triggers that never fired.
- **The migration note named the wrong file.** The importer reads `CBJG-PlayerStats.json`.
- **A 5-card Charlie no longer waits for the shoe.** The deal checked for two cards left and then
  drew three or four of them.
- **Quitting a seat left the insurance stake behind.** It goes back with the hand it was taken on.
- **A player with a challenge still waiting could also take a seat at a table.**
- **The addiction cut rounded two ways.** A solo win and a House win could differ by a point on the
  same figure.
- **The tie-break line named a hand that was never split** - a solo player was told about "Hand 1".
- **The duel log always recorded a balance of 0 before the hand.**
- **`{seats}` meant two different things.** In the deal line it is the seats actually taken, which
  now has its own `{seated}`; `{seats}` still works there for anyone who had already edited the text.

### 💬 Changed

- Every one of the 169 chat lines has been rewritten. Same meaning, same placeholders, shorter and
  less stilted — `(Value: 20)` is now `(20)`, and the handful of lines the game built in code rather
  than from a setting read the same way as the rest.
- The leaderboard and `!stats` lines were reworded to match.
- The update check compares version numbers instead of strings. Any tag that merely differed from
  this build used to announce itself as an update, an older one included.

### 🗑️ Removed

- **The platform switches.** Twitch, YouTube and Kick had a tick each, and a command arriving from
  an unticked one was answered with a refusal. Streamer.bot already decides which platforms a
  trigger fires on, so the switches only gave the same decision a second, quieter place to live —
  and a mismatch between the two looked like the game ignoring chat. The game now answers wherever
  its commands fire and works out which platform that was on its own, which is what the other chat
  games here do. `MsgPlatformDisabled` went with them, leaving 169 chat lines.

---

## [16.1.1] — 2026-08-11

- A redesigned settings window.
- Works with Streamer.bot 1.0.7.

---

## [16.0.1] — 2026-08-06

- **The overlay hands over instead of going dark.** When a game ends, the next one still running
  takes the screen, in this order: a live House table, a live duel, a pending challenge, a live solo
  hand.

---

## [16.0.0] — 2026-08-05

### 🏠 House Mode (new)

One dealer against a row of seats, all playing the same shoe — the shape a real table has, which
neither the solo game nor a duel does.

A moderator opens a table with `!house`, picks the rule set from a menu or names it outright
(`!house vegas 500`), and the join window opens. Viewers take seats with `join <bet>`, each with a
stake of their own. The table deals when the last seat fills or the window runs out, or at once if
whoever opened it says `deal`. Every seat plays in turn, and then the dealer plays one hand that all
of them are settled against.

**Three rule sets**, each switchable, each a real casino's:

| | Vegas Strip | Atlantic City | European |
|---|---|---|---|
| Shoe | 4 decks | 8 decks | 2 decks |
| Dealer peeks | yes | yes | **no** |
| Double | any two cards, after a split too | any two cards, after a split too | 9, 10 or 11 only, never after a split |
| Split | any two 10-value cards | any two 10-value cards | matching ranks only |
| Hands from splitting | up to 4 | up to 4 | 2 |
| Insurance | yes | yes | no |
| Surrender | no | late surrender | no |

The European dealer not peeking is the one that changes how a hand feels: a dealer blackjack there
takes everything staked at a seat, money added by doubling or splitting included.

**Also in House Mode**

- Two to six seats, with their own minimum and maximum stake.
- Deadlines on the rule pick, the join window, the insurance window and each seat's turn, all
  resolving themselves.
- Its own cooldown pair, and its own switch for whether a seat counts towards hands per hour.
- Six statistics of its own.
- The overlay curves the seats around the dealer and shows the open ones as dashed chips while the
  join window runs.
- 39 chat lines of its own.
- Played for real currency only: with Free Game on, the table refuses to open.

### 🎩 Dealer peeks for blackjack (new)

Optional, and off by default so nothing changes unless you want it to. With it on, the dealer takes
a face-down card at the deal and checks it as soon as the upcard is an Ace or a ten. A dealer
blackjack then ends the hand on the spot, so the player loses the original stake and nothing more —
rather than finding out after they have put more on the table by doubling or splitting. Two
blackjacks push.

With it off, the dealer's second card is not drawn until the player has finished, which is how the
game has always played.

### 📺 Overlay and chat

- **Cards in chat** picks what a hand looks like in chat: cards and numbers, cards only, or numbers
  only. It applies to solo hands, duels and House tables alike.
- **Result hold** on the Overlay page sets how long a finished hand stays up (8 seconds by
  default, at least 1), replacing the fixed fade-out.
- Duel and House hands that stood under 21 get a `STAND` badge.
- Long names wrap and shrink to fit on the duel seats and the hand labels.
- The solo dealer sits in the middle of the screen.
- **Set it up in OBS** writes the overlay file straight away rather than on the next command.

### 🗑️ Removed

- **Moderators only can open a table**, and its message. Who may use `!house` is set on the
  command in Streamer.bot.
- Moderators no longer skip the hands-per-hour limit.

---

## [15.0.0] — 2026-08-04

The first release in a long time. Alongside the classic single-player game it
brought three large features: **player-vs-player duels**, a **full OBS browser-source overlay**, and
a **persistent leaderboard** kept in Streamer.bot user variables rather than a stats file.

---

### ⚔️ Duel Mode (new)

Chat can play **each other** instead of the dealer.

One player challenges another for a stake, the other accepts or declines, and the winner takes the
pot. No dealer and no house: the points move straight from one viewer to the other, so the economy
stays where it was.

**Commands**

| Command | What it does |
| --- | --- |
| `!bjduel @user <bet>` | Challenges a chatter. Same bet shortcuts as `!blackjack` (`all`, `half`, `max`, `1.5k`, `50%`, …), and the order does not matter — `!bjduel 500 @user` works too |
| `accept` | Answers whichever prompt is waiting on you |
| `decline` | Refuses it |
| `hit` / `stand` | Plays your hand, as at the table |
| `double` / `split` | Asks for a raise, see below |
| `quit` | Forfeits, and the opponent takes the whole pot |

**How a duel plays**

- Both players get two cards face up from one shared shoe.
- The challenger acts first, then the opponent, then the showdown.
- A bust loses at once. A natural blackjack beats a made 21 and **stands by itself**, so a player
  holding one is never asked to act.
- Equal totals push, and both stakes go back.
- Stakes are held from the moment of the challenge, and returned on a decline, a timeout or a push.

**Raises need the opponent's agreement**

Splitting and doubling lift the stake for *both* players, so neither can do it alone. The opponent
gets an accept or decline prompt, and declining refuses the raise only — it never ends the duel.

- Each split hand plays for its own equally funded side pot.
- A player with fewer hands defends every pot with the single one they have.
- A split that does not add a pot needs no agreement and happens at once.

**Sudden death** *(optional, off by default)*

Switched on, a tied duel goes to double or nothing instead of simply returning the stakes. Both
players have to accept, both stake again, and each draws one more card. A decline, a timeout or a
shortfall returns everything. It repeats up to the round limit you set.

**Also included**

- Deadlines on accepting a challenge, taking a turn and answering a raise.
- Full Free Game support, so a duel can be pure bragging rights.
- Blocked groups, the Addicted list, cooldowns and hands per hour all apply.
- Insurance and surrender are dealer rules, so a duel has neither.
- 51 chat lines of its own.

---

### 📺 OBS Overlay (new)

A self-contained browser source that draws the game live, installed into the overlay folder on its
own — no copying files about.

**Setting it up:** add a Browser Source in OBS pointing at `blackjack_overlay.html`. The background
is transparent, so it sits over any scene.

**What it draws**

- Real card faces, suits and colours for the dealer and every hand.
- Deal-in animations, a highlight on the hand in play, totals, stakes, an insurance line and outcome
  badges — win, lose, push, bust, blackjack, 5-card Charlie, three sevens.
- **Duels:** the two players either side of a VS badge with the live pot between them. Split hands
  stack per side pot, and each side gets its own result.
- **Prompt cards** with a countdown ring for a pending challenge, a raise and a sudden-death offer.
  The card counts itself down and disappears, so the overlay never sits on a dead prompt.
- Fades out a few seconds after a hand ends.

**On the Overlay page**

Scale, font scale, the two card colours, backdrop colour and opacity, the dealer's name, and
switches for the totals, stakes, outcome badges, insurance line and animations — or for hiding the
overlay entirely during Free Game.

**It updates itself** — the file is version-stamped, and a newer one is written out automatically
with your previous file kept alongside as a `.bak` rather than silently overwritten.

---

### 🏆 Leaderboard and statistics (new and reworked)

**Statistics moved out of the file.** Every tracked value is a persisted Streamer.bot **user
variable** named `bjg_*` — 50 of them, covering hands, wins, losses, blackjacks, busts, streaks,
doubles, splits, surrenders, insurance, and a full separate set of duel and House records.

Which means any other Streamer.bot action can read a player's record straight out of them —
`bjg_duelsWon`, `bjg_netProfit` — without going through this game at all.

**The stats commands moved into their own action** (`Blackjack Game - Stats`), so the leaderboard can
grow without touching the game.

**`!allstats`**

- Ranked on **combined lifetime net** — table profit plus duel winnings — so someone who only ever
  duels is on the board too.
- 🥇 🥈 🥉 for the top three, switchable, numbers below that.
- One to ten places.
- The **broadcaster** and the **bot account** can be hidden, matched by user id so a rename cannot
  put them back on.
- Its own bot and /me switches, separate from the game's messages.

**Resets**

- A button in the settings window, with a dialog that spells out exactly what is about to go.
- **Weekly or monthly** resets for running seasons.
- Two scopes: *the leaderboard totals only*, which keeps hands played, blackjacks, streaks and the
  duel records, or *every stat there is*.
- Turning automatic resets on never wipes anything there and then — the current period is noted
  first, and the reset waits for the next boundary.

**`!stats`** also reports your duel record, duel blackjacks, duel busts, forfeits and a combined net
matching the leaderboard ranking.

**Migration** — an existing `CBJG-PlayerStats.json` is imported into user variables on the first
run. The old file is left on disk as a backup and never read again. Nothing is lost.

---

### 🆓 Free Game

Free Game has its own lines rather than reusing the ones written for real currency. Wins, blackjack
wins, losses, quitting, and the duel challenge, expiry and sudden-death lines all have Free Game
versions, so nothing mentions points that were never on the table.

---

### 🔧 Changed

- **Statistics storage** moved from `BJG-PlayerStats.json` to persisted user variables.
- **`stats` and `allstats` moved to their own action.** Point both commands at
  `Blackjack Game - Stats`; the command IDs themselves did not change.
- Insurance and surrender are routed by command ID only. The older name-matching fallback is gone.

### 🗑️ Removed

- **Maximum payout** and **show payout cap message**, along with the capped-payout line. They
  predated the current payout model and no longer did anything useful.

---

### Setting it up

1. Import the three actions: **Blackjack Game**, **Blackjack Game - Settings** and
   **Blackjack Game - Stats**.
2. Point `stats` and `allstats` at the Stats action, and everything else at the game action.
3. Create the three duel commands and point them at the game action:

   | Command | Command ID |
   | --- | --- |
   | `!bjduel` | `8ba7b2b7-f9f9-4b70-8cca-ee2aaebe4892` |
   | `accept` | `4a669793-bd4b-422e-bed5-e9b0054cc7ea` |
   | `decline` | `25d6d4e1-4790-4de4-97c2-33444b948348` |

4. Open the settings window to set it up, then add the overlay to OBS as a Browser Source if you
   want it on stream.

**Platforms:** Twitch, YouTube and Kick.
