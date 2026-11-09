# Kickback

**Play it: [jxde-greene.github.io/kickback](https://jxde-greene.github.io/kickback/)**

Laser tag for Skyrocket **Recoil** blasters, with no hub and no official app. Open one web page on each player's phone, connect your blaster over Bluetooth and play. Android and iPhone can play in the same game.

Kickback isn't made by or affiliated with Skyrocket. Recoil is their trademark.

## What you need

- Recoil blasters (rifles and pistols) and, if you have them, Recoil grenades
- One phone per player:
  - **Android:** Chrome
  - **iPhone:** the free **Bluefy** browser from the App Store (Safari can't use Bluetooth)
- An internet connection to host or join a game. There's also a no-network mode.

## Getting started

1. Open the link above (in Bluefy on iPhone).
2. Enter your name.
3. Turn your blaster on and tap **Connect gun**. The blaster kicks once when it's paired to your phone. If it isn't in the list, tap **Show all devices**.
4. Mount the phone or keep it in a pocket. The screen stays on during a match. Earbuds help in a loud game.

To try the app without a blaster, tap **Simulated gun**.

## Starting a match

- **Host a game** gives you a 4-letter room code. Everyone else types it in and taps **Join**. The host sets the rules and taps **Start match**.
- **Play with no network** works anywhere. Every phone picks a different gun number and the same team setup, then everyone starts together. Each phone keeps its own score.

### Rules the host can set

- Free-for-all or team deathmatch (Blue vs Red)
- Arena: Outdoor, Indoor, or Hybrid (house plus yard)
- Time limit and kill limit
- Health, respawn time and reload time
- Spawn shield, friendly fire and hit-marker sounds
- Grenade damage and blast length

## During a match

- **Health** is the ring around the screen. It's green, and turns red when you're nearly out.
- **Ammo** is the big number in the middle, with one orange bar per round underneath.
- **Reload:** press the blaster's reload button. The blaster can't fire until the reload finishes.
- **Fire mode:** tap Single, Burst or Auto on screen, or press the blaster's thumb button to cycle. Auto is for rifles only.
- **Infrared:** tap Out, Hybrid or In to change your blaster's range when you move between indoors and outdoors.
- **Grenades** hit everyone in range once with heavy damage.
- When you're tagged out, the screen counts down to your respawn.

You'll also get sound, screen flashes and voice callouts. Sounds cover shots, hits, reloads and kills. When you're hit, the screen flashes red on the side the shot came from. Voice callouts announce kill streaks, lead changes and the last minute. The **Sound and feel** card on the start screen sets the volume and turns voice callouts and the hit kick on or off.

## How it works

Each phone referees its own player. It reads hits from its blaster, tracks health, locks the blaster when you're tagged out and runs the respawn timer. Phones only share deaths, hit markers and the lobby, through a free public relay (HiveMQ, with Mosquitto and EMQX as backups). If a phone drops off the network, its game keeps going and the scores catch up when it reconnects.

The relay is public, so anyone who guessed your room code could see your game's messages. That's harmless for laser tag, but don't use it for anything private.

## Troubleshooting

- **Buttons do nothing, or the page is stuck loading:**
  - Check the version tag next to the logo.
  - If it says "failed", a red bar at the bottom of the screen explains why.
  - On iPhone, make sure you're in Bluefy and not Safari.
- **The blaster won't show up:**
  - Turn it off and on again, then tap **Show all devices**.
  - Make sure Bluetooth is on and the browser is allowed to use it.
- **Can't join a game:** check the room code with the host. The lobby needs internet, either cell data or Wi-Fi.
- **No sound on iPhone:** tap any button once. Sound then plays even with the silent switch on.
- **Lab tab:** shows live data from your blaster and a Diagnostics box. Screenshot the Diagnostics box when reporting a problem.

## Known limitations

- There's no host hand-off yet. If the host's phone leaves, the others keep playing, but only the host can end the match early.
- In no-network mode, each phone only knows its own deaths and the kills it was told about.

## For developers

- `index.html` is the whole app. It's built from `index.src.html` with `python3 build.py`, which needs [esbuild](https://esbuild.github.io/).
- The build compiles the script for older Safari versions, then embeds the network library and fonts so the page needs no outside downloads.
- To host your own copy, upload `index.html` to a GitHub repo and turn on GitHub Pages (Settings, then Pages, then deploy from the main branch). Web Bluetooth needs HTTPS, which GitHub Pages provides.
- The Recoil Bluetooth protocol comes from the Lazer Swarm community's wiki. `parseTelemetry`, `commandBytes` and `weaponConfigBytes` in the source implement it. The app never sends the bootloader command.

## Credits

- Recoil protocol research: the Lazer Swarm community
- [MQTT.js](https://github.com/mqttjs/MQTT.js) (MIT licence) for networking
- Barlow and Barlow Condensed fonts by Jeremy Tribby (SIL Open Font License, in `vendor/fonts`)
