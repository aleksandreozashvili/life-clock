# Recreate This Project

Paste this entire prompt into a fresh Claude Code session to reproduce this project end-to-end. Replace the `{{curly-brace}}` values for a different person.

---

Build me a personal "life clock" — a live web page that shows my exact age in real time, ticking every second from my moment of birth. It must be publicly shareable via a custom subdomain and installable as a PWA on any phone.

MY DETAILS (change these for a different person):
- Name: {{Aleksandre Ozashvili}}
- Email (for git config): {{aleksandreozashvili@gmail.com}}
- Birth moment: {{1995-03-30, 17:00}} in {{Asia/Tbilisi (UTC+4)}}
- GitHub username: {{aleksandreozashvili}} (or let `gh` detect)
- Custom domain I own: {{aisolution.to}}
- Target subdomain: {{aleksandreozashvili.aisolution.to}}

REQUIREMENTS:
1. A single static HTML page, no build step, no framework. Everything inline in index.html (CSS + JS).
2. Two display modes, toggled by clicking anywhere on the page; selection persists via localStorage:
   - "Days" mode: "Xd YYh MMm SSs" (days with thousands separator, plus hours/mins/secs with tabular-nums so digits don't jitter)
   - "Seconds" mode: total seconds since birth with thousands separator
3. Huge centered number (clamp-based font-size that scales phone → 4K), minimalist airport-display aesthetic, modern system font stack, light/dark auto via prefers-color-scheme.
4. Browser tab title updates live to the current count.
5. Small label above with my name; small "tap to toggle units" hint below that fades after first interaction.
6. Small QR code in bottom-right corner pointing at the final URL, with caption "scan to share". Pre-generate the QR as inline SVG using `npx qrcode <url> -t svg` so no runtime library is needed. Invert QR in dark mode so it stays scannable.
7. Full PWA: manifest.webmanifest (name, short_name, icons 192/512 any+maskable, standalone display, theme colors), service worker (cache-first app shell, cleans old caches on activate, claims clients), apple-touch-icon + apple-mobile-web-app-capable meta for iOS. Must work offline after first load.
8. Favicon SVG: simple clock glyph (circle, hour/minute hands, tick marks). Convert favicon.svg to icon-192.png and icon-512.png via `npx sharp-cli`.
9. Birth timestamp is stored as a UTC Date constant in JS — convert the local birth time to UTC. Verify in devtools console that `new Date(BIRTH).toLocaleString('en-US', {timeZone: '{{Asia/Tbilisi}}'})` prints the original local time.
10. File list: index.html, manifest.webmanifest, sw.js, favicon.svg, icon-192.png, icon-512.png, CNAME (contains the subdomain), README.md. Nothing else.

DEPLOYMENT (do all of this from this session):
1. If `gh` CLI is not installed, install it via `winget install --id GitHub.cli -e` (Windows) or the OS-appropriate package manager. Add it to PATH for this shell session if needed.
2. Configure git: `git config --global user.name "{{name}}"` and `user.email "{{email}}"`.
3. Run `gh auth login --hostname github.com --git-protocol https --web` IN THE BACKGROUND, read the device code it prints, and give it to me to approve in a browser.
4. Once authenticated, create the project folder, write all files, `git init -b main`, commit, then `gh repo create life-clock --public --source=. --push`.
5. Enable GitHub Pages via `gh api -X POST "repos/<user>/life-clock/pages" -f "source[branch]=main" -f "source[path]=/"`. The CNAME file in the repo auto-configures the custom domain.
6. Give me exact DNS instructions for my registrar: one CNAME record — Name: `{{aleksandreozashvili}}`, Target: `{{aleksandreozashvili}}.github.io`. I'll add it myself since you can't touch my registrar.
7. After I confirm DNS is added, verify resolution with `nslookup <subdomain> 8.8.8.8`, wait for the Let's Encrypt cert to provision (~10–15 min), then enforce HTTPS via `gh api -X PUT "repos/<user>/life-clock/pages" -F "https_enforced=true"`.

EXPLICIT NON-GOALS:
- No backend, database, analytics, or framework.
- No build tooling, bundler, or package.json committed to the repo. (`npx qrcode` and `npx sharp-cli` are one-off local tools, their output is committed; nothing runs at deploy time.)
- No milestone notifications (e.g. "1 billion seconds").
- No other personal-page sections — the page is clock-only.

VERIFICATION (run through each before declaring done):
- Open index.html locally: counter ticks smoothly, click toggles units, reload preserves choice, tab title updates live, QR renders.
- Timezone sanity check (console command above).
- OS light/dark toggle flips page colors correctly.
- Mobile: no horizontal scroll, readable, QR still tappable.
- PWA: Chrome/Edge shows install prompt; iOS Add-to-Home-Screen works; offline reload after first load still shows the ticking counter.
- Live: after DNS + cert are ready, https://{{subdomain}} loads with a padlock from a second device on cellular data.

Start by confirming you understand the requirements, then proceed through implementation. Use a todo list to track progress. Ask for clarification on any detail that's ambiguous before writing files.
