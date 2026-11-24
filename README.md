# Cam Challenge (Price Bailey)

Two-screen exhibition game. Players steer a Price Bailey punt down the River Cam, from The Backs in Cambridge out through the fens to Ely. The big screen shows the game; a phone is the controller. Player details are captured before every game and sent to HubSpot.

## Files
- `index.html`: big-screen game (open this on the stand display).
- `controller/index.html`: phone controller (opened by scanning the QR code).
- `lib/link.js`: connection between the two screens (MQTT over WebSockets, with BroadcastChannel for desk testing).
- `lib/hubspot.js`: HubSpot settings and submission, shared by both screens. Game name and event name live here.
- `lib/mqtt.min.js`, `lib/qrcode.js`: bundled libraries (no CDN calls).
- `assets/logo.png`: TODO, add the approved Price Bailey logo here. A text fallback shows until then.

## Details form on both routes
Nobody starts a game without entering their details first.
- Scanning the QR code shows the form on the phone.
- Pressing Start cruise on the big screen, or the Space or Enter key, shows the same form on the big screen itself, so the stand tablet also records details and the high score.

Details are held in memory only, sent to HubSpot at the start and again with the score at the end, and cleared when the page reloads for the next player. The big screen never shows personal details once the form is submitted. An abandoned big-screen form clears after 90 seconds, and a phone player taking over closes it.

## Route and landmarks
The cruise runs downstream in three stages: The Backs, Jesus Green and The Fens, finishing at Ely. Landmarks drawn along the banks are King's College Chapel, Senate House, the Mathematical Bridge, the Bridge of Sighs, the Mill Pond punt station, moored punts, a rowing eight and Ely Cathedral. These are original simplified illustrations, not traced images. TODO: ask a local to sense-check the route order, which is simplified for gameplay.

## Publish on GitHub Pages
1. Create a public repo under `price-baileymarketing-star` (suggested name `cam-challenge`).
2. Upload the contents of this folder, not the folder itself, so `index.html` sits at the top of the repository.
3. Settings > Pages > Deploy from a branch > `main` / root.
4. Wait for the green tick on the Actions tab, then open `https://price-baileymarketing-star.github.io/cam-challenge/` on the big screen.
5. Scan the QR code with a phone. Add `&debug` to the controller address to see HubSpot's replies while testing.

## Test at your desk without a phone
Open the game in one tab, then open the URL shown under the QR code in a second tab of the same browser. The tabs talk through BroadcastChannel, no internet needed. You can also press Start cruise on the big screen to play on the tablet route.

## How the link works
Phone and screen connect through public MQTT brokers over secure WebSockets (`wss://broker.emqx.io:8084/mqtt` and `wss://broker.hivemq.com:8884/mqtt`, used together for redundancy). Both devices need internet. On the day, put the stand phone on 4G. The public brokers have no uptime guarantee: for an important event, move to a private broker (`?broker=` in the address) or Firebase.

## Change things
- Brand colours: `:root` block at the top of both HTML files.
- Balance and copy: `CONFIG`, `GOOD`, `RISKS` in `index.html`.
- Route and landmarks: `LANDMARKS`, `LABELS` and the `drawLandmark` function in `index.html`.
- HubSpot settings, game name and event name: `lib/hubspot.js`.

## Before any stand (blocking)
- Add the approved logo and confirm the brand hex values.
- Set the event name in `lib/hubspot.js` (`eventName`).
- Confirm the consent checkbox sends `game_form_consent` on a test contact, and have compliance approve the consent wording and privacy notice link.
- Check a test contact appears in HubSpot with every field filled in correctly.
- `price-baileymarketing-star.github.io` must stay a trusted domain in HubSpot. If hosted elsewhere, add that domain too.
