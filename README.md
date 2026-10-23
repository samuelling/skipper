# Skipper

**Last Skip**: swing your phone like a flat stone and watch it skip across a dusk lake. Single file, no build step.

## Standard mode
- Reads `devicemotion`: peak rotation = swing speed, gravity vector = tilt, rotation about the screen normal = spin. Release is detected when angular speed falls off its peak.
- A small physics model turns speed, angle and spin into a skip count. Nothing leaves your hand.
- Swipe mode on desktop or when sensors are missing (Space bar also throws).

## Farewell mode (real throws)
Counts the phone's *actual* bounces and sends each one live to a watcher.

- **Detector:** free fall (accelerometer magnitude under ~0.7 g for 90 ms) marks flight. A spike when flight ends marks a contact. Flight that resumes after a contact confirms a rebound. The last contact is the one that did not rebound, so skips = contacts - 1.
- **Surfaces:** soft (bed, couch, snow, grass) and water use different impact thresholds. Constants are in the `FW` object.
- **Relay:** each event (`Armed`, `Thrown`, `Skip n`, `Farewell: n skips. note`) is POSTed to a random private topic on [ntfy.sh](https://ntfy.sh). The watcher opens `watch.html#<topic>` (the setup screen gives the link) and sees the count tick up live. Change `FW.relay` in `index.html` and `RELAY` in `watch.html` to use your own ntfy server.
- **Live broadcast:** the thrower's screen shows a red LIVE badge, how many watchers have opened the link, and each packet as it leaves ("sending" then "delivered"). The watch page shows a LIVE pill and a heartbeat line that spikes on every skip. If the stream goes quiet for ~4.5 s after the throw, the watcher sees SIGNAL LOST, the line flatlines and a tone plays. If the final tally lands late, it revives to the farewell.
- **Honest limits:** browsers sample motion at ~60 Hz, so late hops shorter than ~50 ms are missed and the count can run low. The page cannot know the phone's fate: each bounce is sent as it happens, and the final tally goes out about 0.7 s after the last contact, if the phone still has a connection. Water kills phones, waterproof or not. Try the soft surface first.

## Hosting
Phones only give pages motion data over HTTPS. Enable GitHub Pages on `main` and open `https://<user>.github.io/skipper/`. iOS asks for motion permission on the first tap.
