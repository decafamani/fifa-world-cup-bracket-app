# ⚽ World Cup 2026 Prediction Pool

A zero-backend, single-file web app for running a World Cup prediction pool with friends, family, or coworkers. No accounts, no logins, no spreadsheets — players click one link, enter their name, and make their picks. One shared leaderboard updates for everyone as the organizer enters results.

Built for the 2026 FIFA World Cup (48 teams, 12 groups, Round of 32 format), but the structure is adaptable to any bracket-style tournament.

## Features

**For players**
- Join with just a name (optional honor-system entry fee tracking)
- Group stage picks: choose the top 2 finishers in each of the 12 groups (24 picks, 1 pt each)
- Knockout picks before every round — Round of 32 (1 pt), Round of 16 (2), Quarterfinals (4), Semifinals (8), Final (16) — so nobody is mathematically dead after a bad week
- Pick lists automatically narrow to teams still alive as results come in; eliminated picks get flagged with a prompt to swap
- Live leaderboard with per-round scoring breakdown, medals, pot tracking, and optional per-round side stakes
- Picks lock automatically at each round's kickoff — no commissioner judgment calls
- Works on any device; identity is remembered per browser, and entries can be reclaimed on a new device by name

**For the organizer**
- PIN-protected organizer mode (created on first use)
- Tap-to-enter results: top-2 per group, the 8 best third-place qualifiers, and knockout winners
- Manual lock overrides per round (force open for latecomers, force closed early, or auto-follow the schedule)
- Add/remove players, CSV export of standings and stakes
- Optional AI-powered results sync when run as a Claude artifact (hidden in self-hosted mode)

**Under the hood**
- Single HTML file. No build step, no framework, no server.
- Shared state lives in a [JSONBin.io](https://jsonbin.io) bin (free tier is plenty for a full tournament)
- Polite syncing: refreshes on load and tab focus, slow background poll, merge-on-save to avoid clobbering concurrent picks
- A bouncing-soccer-ball easter egg, because every office pool deserves one

## Setup (~10 minutes, organizer only)

1. **Create the storage.** Sign up free at [jsonbin.io](https://jsonbin.io) → *Create Bin* → enter `{"init":true}` as the content → save. Note the **Bin ID** (in the bin's URL) and your **Master Key** (under *API Keys*).
2. **Host the file.** Drag `index.html` (zipped or in a folder) onto [Netlify Drop](https://app.netlify.com/drop) — or use GitHub Pages, Vercel, or any static host.
3. **Connect them.** Open your hosted URL. You'll see a one-time *Organizer setup* screen — paste the Bin ID and Master Key, and it generates the shareable pool link.
4. **Optional, recommended:** for a clean URL with no config fragment, edit the `BUILTIN` constant near the top of the `<script>` in `index.html` with your bin ID and key before deploying:

   ```js
   const BUILTIN={bin:'YOUR_BIN_ID',key:'YOUR_MASTER_KEY'};
   ```

   Then the bare site URL is the whole link.

5. Open the site, set your organizer PIN (Results → 🔐), make your own picks, and send the link out.

## Security model (read this honestly)

This is **trust-based, office-pool-grade security**, by design:

- The JSONBin key is visible to anyone who views the page source or the share link. Anyone with it can read or modify the pool data directly via the JSONBin API. Use a throwaway JSONBin account dedicated to the pool.
- The organizer PIN gates the *UI*, not the data. It keeps honest people honest.
- Pick deadlines are enforced against the device clock.
- Don't use this design for anything where money or data actually matters. For a pool among people you know, it's exactly enough.

## Customizing

- **Teams & groups:** edit the `TEAMS_DATA` array (flag emoji, name, FIFA rank).
- **Rounds, points, deadlines:** edit the `ROUNDS` array (`pts`, `pickLimit`, `lockAt`).
- **Styling:** plain CSS custom properties at the top of the file.

## License

MIT — see [LICENSE](LICENSE).

---

*Built iteratively with Claude (Anthropic). The entire app — picks flow, shared storage, lock system, elimination filtering — lives in one file on purpose: it's meant to be read, forked, and repurposed for your own pools.*
