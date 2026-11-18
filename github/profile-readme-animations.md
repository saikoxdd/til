# Animated SVGs in a GitHub profile README

- GitHub serves README images through its camo proxy: **CSS animations inside SVG work**, JavaScript does not
- `<use href="#shape">` keeps a 100-petal animation small (20 KB instead of 45 KB)
- third-party stat cards are often down or rate-limited - self-hosted SVGs in the repo never are
- camo caches images: after updating `banner.svg` it can show the old one for a few minutes
- a crescent moon cut with a fill of `#0d1117` looks like an eye on the light theme, use a `<mask>`
