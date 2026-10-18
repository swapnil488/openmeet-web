# OpenMeet

No-login video meetings with 4K screen sharing. The whole site is `index.html`.

## Put it online (free, about 2 minutes)

Camera, mic and screen sharing only work from an **https://** link. They don't work
if you open the file directly on a tablet or from an `http://` address.

**Netlify Drop (easiest)**
1. Go to https://app.netlify.com/drop
2. Drag the folder containing `index.html` onto the page.
3. You get a link like `https://something.netlify.app`. Open that on every device.

To update it later, drag the new folder onto your site's **Deploys** page.

## Using it
- **New meeting** creates a code like `abc-defg-hij`. Use the link button to copy the invite.
- **Share screen** opens a quality menu:
  - **4K · Auto** (default): full 4K when your computer keeps up. If encoding 4K is too slow (which is
    what makes viewers see things late), it steps down to 1440p or 1080p by itself, and back up when
    there's room. It never goes below 1080p, so text stays readable.
  - **1080p · Lowest delay**: for live demos and remote help. Can go down to 720p to stay quick.
  - **4K · Sharpest text**: always full 4K for code and documents. Can lag on slower computers.
  - **1440p · 60 fps**: for videos, games and fast scrolling.
  You can switch modes while presenting.
- The badge on a shared screen shows the real resolution and fps. On the presenter's side
  it also says if the computer or network is holding it back.
- **Full screen**: tap the corner button on any tile, or double-tap the tile.
- **Who's talking**: the speaker's tile glows green with a small level badge, like Google Meet.
- **Audio & video settings** (sliders button, in the lobby and during a call): pick the microphone,
  camera and speaker, including Bluetooth and USB headsets. Includes a mic level meter and a
  speaker test. Your choice is remembered. Speaker choice works in Chrome, Edge and Firefox on
  computers; phones and Safari use the device's current output.

## Delay
- **Screen sharing delay** comes mostly from the presenter's computer encoding each 4K frame and
  the viewer's browser buffering big frames. In testing on a slow machine, Auto mode cut the
  screen-to-screen delay from about 0.9 s to about 0.3 s (scrolling) and 0.6 s to about 0.27 s
  (typing). Lowest delay mode got about 0.24 s. Computers with hardware video encoding do better.
- Voice is sent separately from video, so a heavy 4K screen share no longer holds everyone's voice
  back (in testing, voice delay during a 4K share dropped from ~137 ms to ~38 ms).
- When presenting to several people, one upload budget is shared between them, so the uplink
  doesn't clog and delay everything.
- Hover a person's tile to see their delay. It stays visible in amber (150 ms+) or red (300 ms+),
  and its tooltip says whether the network or buffering is the main cause.
- The biggest remaining factor is the network: the distance between people and how loaded the
  upload is. Wired or 5 GHz Wi-Fi helps; uploading other things during a call hurts.

## Tablets and phones
- They can join, talk, chat and watch screen shares in the browser.
- **Android app:** the OpenMeet app joins the same meetings and can share the tablet's screen.
  Upload the APK next to `index.html` as `openmeet.apk`. Android visitors then see
  **Open in the app** and **Download the app** on the join page, and
  **Switch to the OpenMeet app** in the Share menu.
- **iPad/iPhone:** can't share their screen. Apple doesn't let websites capture the screen.
- Each tablet gets a share sized to its own display instead of full 4K. It also steps down
  automatically if it starts dropping frames. Laptops and desktops in the same call still get 4K.
- If you see video but hear nothing on an iPad, tap **Tap to turn on sound**.

## If someone can't connect
If a tile says "Couldn't connect", one of the networks (college/office Wi-Fi, some mobile data)
is blocking direct calls. Try a different Wi-Fi or a phone hotspot. The permanent fix is a TURN
relay server: get free credentials from metered.ca (Open Relay) and add them to `iceServers`
near the top of `index.html`:

```js
{urls: 'turn:YOUR_TURN_HOST:3478', username: 'USER', credential: 'PASS'},
```

## Limits
- 4K uses a lot of upload bandwidth: about 10–16 Mbps for each person watching.
  The presenter sends a separate stream to each viewer.
- Best for 2–6 people.
- 4K needs a 4K display on the presenter's side. A 1080p laptop can only send 1080p.
- Use Chrome or Edge to present. Any modern browser can watch.
