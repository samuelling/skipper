# Skipper

Last Skip: swing your phone like a flat stone and watch it skip across a dusk lake. Single file, no build step.

- Motion: reads `devicemotion` (peak rotation = swing speed, gravity vector = tilt, rotation about the screen normal = spin). Release is detected when angular speed falls off its peak.
- Swipe mode on desktop or when sensors are missing (also Space bar).
- Needs HTTPS for motion on phones (GitHub Pages works). iOS asks for motion permission after the first tap.
- Tuning constants live in `TUNE` and `simulate()` in `index.html`.
