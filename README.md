# Planning Poker

A real-time, browser-based planning poker tool for Agile estimation sessions — built so a team can join a session with just a room code, vote, and see results live, without any account creation or app install.

**[Live Demo →](#)** *(add your GitHub Pages link here once published)*

## Why I built this

Running estimation sessions across distributed teams usually means either paying for a third-party planning poker tool, or doing it awkwardly over a shared screen with everyone calling out numbers. I wanted something lightweight, free, and good enough to actually use in a real sprint planning or backlog refinement session — with proper live sync, not just a static form.

This was also my first project building something with real-time multi-user state, rather than a single-user local tool — a natural next step from the GitLab MR Analyser and PI Capacity Planner.

## What it does

**Host a session** — pick a name, a session title, and an estimation scale (Fibonacci or T-shirt sizes). You get a room code and shareable link.

**Join a session** — anyone on the team enters their name and the room code (or just clicks the shared link) to join instantly. No sign-up, no account.

**Vote in real time** — everyone picks a card from the scale. Votes stay hidden from each other until the host reveals them — true blind estimation, the whole point of planning poker.

**Reveal & discuss** — once revealed, the tool shows every vote, the average, min/max spread, and flags a wide divergence so the team knows when a story needs more discussion before locking an estimate.

**Round history** — every completed round is logged automatically with the story name, result, and whether the team reached consensus or had a spread.

**Export** — full session history downloadable as JSON at the end.

The host role isn't fixed to the Scrum Master — anyone running the session (a Dev Lead covering in someone's absence, for example) can host and still cast their own vote like any other participant.

## How it works

- Pure HTML/CSS/JavaScript front end — no build step
- Real-time sync powered by **Firebase Realtime Database** (free tier) — every vote, reveal, and round update propagates live to all connected participants
- Room state lives entirely in Firebase; nothing is stored beyond the session unless exported
- Built with the help of Claude (Anthropic) as a coding assistant

## Tech

`HTML` `CSS` `Vanilla JavaScript` `Firebase Realtime Database`

---

## ⚠️ Setup required before this works

This tool needs a free Firebase project connected before it will function — the HTML file alone won't sync anything until you add your own Firebase config. Takes about 10 minutes, no cost.

### 1. Create a Firebase project
Go to [console.firebase.google.com](https://console.firebase.google.com) → **Add project** → give it a name → skip Google Analytics → **Create project**.

### 2. Enable Realtime Database
**Build** → **Realtime Database** → **Create Database** → pick a region → **Start in test mode** → **Enable**.

### 3. Register a web app
**Project settings** (gear icon) → scroll to **Your apps** → click the **</>** Web icon → give it a nickname → **Register app** (skip Firebase Hosting, this project uses GitHub Pages instead).

### 4. Copy your config
Firebase will show a `firebaseConfig` object with your `apiKey`, `databaseURL`, etc. Copy the whole block.

### 5. Paste into the HTML file
Open `index.html`, find the `firebaseConfig` placeholder near the bottom of the `<script>` section, and replace it with your copied config.

### 6. Check your database rules
**Realtime Database** → **Rules** tab. Test-mode projects default to a time-limited open rule. For long-term use, either extend the expiry timestamp periodically or simplify to:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
This keeps the tool fully open (no login required to host or join) — appropriate for a low-stakes internal estimation tool with no sensitive data, but worth knowing if you fork this for something else.

### 7. Test it
Open the file in two browser tabs (or send to a colleague), create a session in one, join from the other using the room code, and confirm votes sync live.

---

## Usage

1. Open the live link (or `index.html` locally once Firebase is configured)
2. Host: enter your name, a session name, pick a scale, create the session
3. Share the room code or invite link with your team
4. Enter a story title, everyone votes, reveal when ready
5. Review the spread, discuss if divergent, move to the next story

---

Built by [Dhruv S.](https://linkedin.com/in/dhruvsridharan) — Scrum Master / Agile Delivery Lead
