# Import

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/2c17f5a0-15f3-4396-a277-a66638017fb3"> </p> 

Click `Import` in [streamer.bot](https://streamer.bot/) and drag the [BlackjackGame.sb](https://github.com/aaskjer/Blackjack-Game/releases) file into the window, or paste the [import string](https://github.com/aaskjer/Blackjack-Game/blob/main/Import-String.md) instead.

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/a1c731fd-6ef0-4bf2-a312-05e3c95e0feb"> </p> 

`Import`, then `Yes` on the popups that follow.

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/19cc11ee-647e-401a-aafd-cde610918e6b"> </p>

Enable the commands in streamer.bot's `Commands` tab. They arrive switched off, so nothing answers chat until you do.

# Setup

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/8b7c43b3-d9de-4518-94a5-e9d38c8e219d"> </p>

Right-click `Test` in the trigger section and choose `Test Trigger` to open the settings window.

<p align="center"><img alt="The Table page of the settings window" src="Assets/screenshots/settings-table.png" width="460"></p>

Work through the pages in the rail, save, and you are playing. The game reads its settings fresh on every command, so nothing needs restarting.

Three pages are worth a look before the first hand: **Table** for the stakes and the dealer, **Access** for the points variable and who may play, and **Messages** if you want the game to sound like your channel. Every chat line there comes with a live preview, filled in the way chat will see it.

<p align="center"><img alt="The Messages page with its chat preview" src="Assets/screenshots/settings-messages.png" width="460"></p>

# OBS Overlay

<p align="center"><img alt="The Overlay page of the settings window" src="Assets/screenshots/settings-overlay.png" width="460"></p>

Connect streamer.bot to OBS, set the connection number, scene and source name on the **Overlay** page, and press `Set up in OBS`. The game creates the scene and a 1300 × 1080 browser source pointing at the overlay file — move and resize it from there as you like.

Adding the source by hand works too: a Browser Source with **Local file** ticked, pointing at `blackjack_overlay.html` in the overlay folder. A source that points at a `file:///` address works as well, OBS 33 included.

## Examples

### Duel challenge
<p align="center"><img alt="A duel challenge waiting for its answer" src="Assets/screenshots/overlay-duel-challenge.png" width="320"></p>
The challenged player has a deadline to accept or decline. Let it run out and the duel is off, with the stake returned.

### A duel in play
<p align="center"><img alt="A duel in play" src="Assets/screenshots/overlay-duel.png" width="480"></p>
Every solo command works here except insurance and surrender — those are dealer rules, and a duel has no dealer.

### A House table
<p align="center">
  <img alt="A House lobby with two of five seats taken" src="Assets/screenshots/overlay-house-lobby.png" width="380">
  <img alt="A House table with four seats" src="Assets/screenshots/overlay-house.png" width="400">
</p>
The seats curve around the dealer. While the join window runs, the open ones show as dashed chips; once the cards are out, every hand plays against the one dealer hand.
