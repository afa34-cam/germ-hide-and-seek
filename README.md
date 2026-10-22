# Germ Hide & Seek

A two-phase memory card game about MHC molecules and the immune system.  
Built with vanilla HTML5 Canvas — no frameworks, no build step, no server needed.

---

## Testing locally

Just open `index.html` in any browser. No web server required.

```
double-click index.html   ← works on Windows, Mac, Linux
```

If your image/sound files don't load (browser blocks local file:// requests),
run a tiny local server instead:

```bash
# Python 3
python -m http.server 8000
# then open http://localhost:8000
```

---

## File structure

Place all files flat in the same folder:

```
index.html          ← the whole game (edit this file)
header.png          ← welcome screen banner  (860 × 172 px)
mhc_empty.png       ← card back / Phase 1 back  (80 × 80 px)
healthy.png         ← healthy cell face  (80 × 80 px)
front_0.png         ← pathogen 0 face  (80 × 80 px)
front_1.png
front_2.png
front_3.png
front_4.png
back_0.png          ← pathogen 0 Phase-2 back  (80 × 80 px)
back_1.png
back_2.png
back_3.png
back_4.png
path_0.png …        ← bouncing germs on the welcome & end screens
path_4.png
flip.ogg / .wav     ← sound: card flip  ("[trimmed] flip.ogg" is used first)
plop.ogg / .wav     ← sound: healthy pair cleared
match.ogg / .wav    ← sound: germ pair matched
mismatch.ogg / .wav ← sound: cards don't match
fonts/              ← the game's fonts (.woff2) + their licences
manifest.webmanifest ← lets phones/tablets add the game to the Home Screen
app-icon-180.png    ← Home Screen icon (iPhone / iPad)
app-icon-192.png    ← browser tab + Android icon
app-icon-512.png    ← Android icon / install screen
```

The `.wav` copies are used by browsers that can't play `.ogg` (older iPhones/iPads).
`header.png` and `MHC.png` are not currently used by the game.

Any missing image falls back to built-in coloured art automatically.  
Any missing sound is silently skipped.

---

## Editing design & colours

Everything you need to change is at the top of `index.html`,
between the two `STYLE CONFIGURATION` banners.

| Section | What to edit |
|---|---|
| Colours | CSS colour strings — e.g. `"#f88ca0"` |
| Fonts | Change `FONT_TITLE.family` etc. to any Google Font name |
| Screen layouts, card size, columns, text size | `LAYOUTS` (landscape / portrait / phone) |
| Card spacing, corners, animations | `CARD_MARGIN`, `CARD_RADIUS`, `CARD_FLIP_MS`, … |
| Version shown in the menu | `GAME_VERSION` |
| Button sizes / smallest tap target on touch screens | `navW`, `navH`, `big` in `LAYOUTS`; `MIN_TAP_PX` |
| Home screen title, peeking germs, "How to play" steps | `TITLE_A`, `TITLE_B`, `TITLE_PEEK_AT`, `PEEK_*`, `FOUND_LINES`, `howToSteps()` |
| Words + MHC picture on the in-between screen | `INTERMEZZO_TEXT`, `INTERMEZZO_READY`, `MHC_LEGEND` |
| Background blobs | `BG_BLOBS` array |
| Image filenames | `ASSET_*` constants in the IMAGE ASSETS section |
| Sound filenames | `SOUND_*` constants |

### Screen sizes

The game always fills the whole window and picks a layout automatically:

- **landscape** (10 × 4 cards): laptops and iPads held sideways
- **portrait** (5 × 8 cards): iPads held upright, tall browser windows
- **phoneLandscape** / **phonePortrait** (bigger cards and text): phones

`PHONE_ORIENTATION` (next to `LAYOUTS`) sets how phones may be held: `"both"` (default),
`"landscape"` or `"portrait"`. Held the other way, the game shows a "please turn your
phone" message and pauses the timer. Tablets and computers always work either way.

On phones and tablets that aren't running it as a Home Screen app, the game offers
an "Add to Home Screen" guide once per visit (with a "Don't show again" option).
It's also available from the button on the welcome screen and in the menu's Settings tab.

### Menu, sound and analytics

The ☰ menu (top right, or "Menu" in the bar during play) has **Settings**
(sound on/off, opt out of anonymous GoatCounter stats, Add to Home Screen) and
**About** (description, credits, version). Settings are saved in the browser.
Opening the menu pauses the timer.

GoatCounter only loads if the player hasn't opted out, and never counts visits
from `localhost`. The game loads a fixed version of GoatCounter's script
(`count.v4.js`) with an integrity hash, so the browser refuses to run it if it
was ever changed. If GoatCounter releases a new version you want to use, update
both the file name and the hash in `loadGoatcounter()`. It records the page visit plus `game_started` / `game_finished` events.

### Fonts

The fonts (Anta, Poppins, Sour Gummy) are stored in the `fonts/` folder instead of
being loaded from Google Fonts, so no visitor data goes to Google. They're all
under the SIL Open Font License; the licence files (`fonts/OFL-*.txt`) must stay
with them.

To use a different font:
1. Download it as `.woff2` (e.g. via https://gwfh.mranftl.com, which packages
   Google Fonts for self-hosting) and put it in `fonts/`.
2. Add an `@font-face` rule for it at the top of the `<style>` block in `index.html`.
3. Reference it in the font config:
   ```js
   const FONT_TITLE = { family:"'Nunito',sans-serif", size:40, bold:true };
   ```

---

## Hosting on GitHub Pages (free)

1. Create a new repository on github.com (can be private or public).
2. Push all your files to the `main` branch.
3. Go to **Settings → Pages**.
4. Under **Source**, select **Deploy from a branch → main → / (root)**.
5. Click **Save**.
6. After ~60 seconds your game is live at:
   ```
   https://<your-username>.github.io/<repo-name>/
   ```

That's it. Every time you push changes to `main`, the live site updates automatically.

---

## Custom domain (optional)

In **Settings → Pages → Custom domain**, enter your domain.  
Add a `CNAME` file to your repo containing just your domain name, e.g.:
```
game.yourschool.ac.uk
```
Then point a CNAME DNS record at `<your-username>.github.io`.
