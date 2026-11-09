# Kickback

Hubless laser tag for Recoil (Skyrocket) blasters. One web page; Android plays in Chrome, iPhone plays in the free **Bluefy** browser. No official hub, no official app.

Each phone referees its own player: it reads hits from its gun, tracks health, locks the gun on death and runs the respawn timer. The network only shares deaths, hit markers and the lobby, so a dropped connection never stops anyone's game.

## Put it online (5 minutes, free)

1. Create a GitHub repo (e.g. `kickback`) and upload `index.html`.
2. Repo **Settings → Pages → Source: Deploy from a branch → main / root → Save**.
3. After a minute your game lives at `https://<your-username>.github.io/kickback/`. HTTPS is required for Web Bluetooth; GitHub Pages provides it.

## Each player

- **Android:** open the link in Chrome.
- **iPhone:** install Bluefy (App Store), open the link inside Bluefy.
- Enter a name, turn the gun on, tap **Connect gun**. The gun kicks once when it's yours. If it doesn't appear in the list, use **Show all devices**.
- Mount the phone, keep the screen on, earbuds optional.

## Playing

- **Host a game** gives a 4-letter room code; everyone else taps **Join**. Host sets the rules and taps **Start**.
- **Play with no network** works anywhere: every phone picks a different gun number (and the same team setup), then starts together. Scores stay per phone.
- In game: reload button = 1.5 s reload. Thumb button cycles fire mode. **IR** button cycles Outdoor / Hybrid / Indoor for your own gun when you cross a doorway.

## First test (do this before a real match)

1. **Gun Lab tab**, one gun: confirm live telemetry updates, battery shows, and *Recoil kick* / *Muzzle flash* work.
2. Two guns: shoot gun B with gun A. Gun B's *Last hits* should show `shooter` = gun A's ID.
3. Tune Hybrid: in *Weapon config*, try close-cone values (`08`, `20`, `40`) shooting through a doorway and point-blank. Tell me the value you like and I'll make it the default.
4. Two phones, **Host** + **Join**, short match. Then put one phone in airplane mode for a minute and turn it back on: scores should match afterward.

## Status (v0.2)

- Protocol bytes match the Lazer Swarm wiki. Tested with simulated guns: hosting, joining, tagging, respawn, grenades, and score resync after a phone drops off the network. **Nothing has been tested on real guns or real phones yet.** Step 1 above will surface any surprises fast.
- The lobby uses a free public MQTT relay over the internet (HiveMQ, with Mosquitto and EMQX as fallbacks). It works through cell data and home Wi-Fi. It's public, so anyone who guessed your room code could see game messages; harmless for laser tag.
- Not built yet: base-QR respawn, end-of-game QR score merge, host hand-off if the host's phone leaves.

## If something doesn't work on a phone

- The version tag next to the logo should say **v0.2**. If it's stuck on "loading…", the script didn't run; a red bar at the bottom shows why.
- **Gun Lab → Diagnostics** lists what the browser supports and the gun's connection state. Screenshot it when reporting a problem.

## Files

- `index.html`: the file to upload; this is the game.
- `index.src.html` + `build.py`: readable source. `python3 build.py` compiles it into `index.html` for older iPhones (needs esbuild).

## Protocol notes

`parseTelemetry`, `commandBytes` and `weaponConfigBytes` in `index.html` implement the Lazer Swarm wiki's Recoil Bluetooth protocol. The app never sends the bootloader command.
