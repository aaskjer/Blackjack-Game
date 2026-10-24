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

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/1c7f7cdb-5fdd-4fb6-b802-f3eb6aedea48"> </p>

Work through the pages in the rail, save, and you are playing. The game reads its settings fresh on every command, so nothing needs restarting.

Three pages are worth a look before the first hand: **Table** for the stakes and the dealer, **Access** for the platforms you stream on and the points variable, and **Messages** if you want the game to sound like your channel.

# OBS Overlay

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/80f527c8-6156-430b-98cc-3e9d9bfed429"> </p>

Connect streamer.bot to OBS, set the connection number, scene and source name on the **Overlay** page, and press `Set up in OBS`. The game creates the scene and the browser source for you, pointing at the overlay file — move and resize it from there as you like.

## Examples

### Duel challenge
<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/daaccc2e-588a-4665-9672-b3a28cd92dd9"> </p>
The challenged player has a deadline to accept or decline. Let it run out and the duel is off, with the stake returned.

### A duel in play
<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/6027414c-8282-4f23-8edb-74821dbac2b7"> </p>
Every solo command works here except insurance and surrender — those are dealer rules, and a duel has no dealer.
