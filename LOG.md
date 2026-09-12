# LOG

Chronological record of every action taken, how it was done, and why. Newest session at the bottom. Any LLM continuing this project should read PROJECT.md first, then this file, then STRATEGY.md.

Timestamps are IST. Environment: macOS 26.5.1, Claude Opus 5 via Claude Code.

---

## Session 1 — 2026-09-12

### 1.1 Research: how Instagram reach actually works in 2026

**What:** Two web searches on the 2026 Reels algorithm and on fast-growth faceless niches.

**How:** WebSearch against SocialPilot, Buffer, Hootsuite, and public Mosseri statements.

**Why:** Every downstream decision (video length, hook design, which metric to optimize) depends on knowing the current ranking signals. Guessing from 2023-era advice would have produced a plan optimized for likes, which no longer drives reach.

**Findings that changed the plan:**
- Ranked signals are watch time → **sends per reach** → likes per reach. Sends, not likes, is the growth lever. This became the "send test" in STRATEGY.md §2.
- Follower count does not gate Reels reach. Reels go mostly to non-followers. This is the only reason a 48-hour sprint from zero is worth attempting at all.
- Skip rate in the first 3 seconds terminates distribution. This is why every script now opens on the result, never the setup.
- Near-duplicate posts carry a 24h–30d reach penalty, and 10+ reposts in 30 days removes an account from recommendations entirely. This is why the plan uses five rotating formats rather than one repeated template.

### 1.2 Honest assessment of the 48-hour / 10k target

**What:** Wrote a section in PROJECT.md stating that 10k followers in 48 hours is not achievable legitimately, with a realistic range of 300–3,000.

**Why:** The user is making a decision about how to spend two days and whether to keep paying for a subscription. A plan built on a number that cannot happen would waste both. Documented rather than mentioned in passing, so it survives into future sessions and does not get quietly forgotten.

**Also recorded:** the specific tactics that are off the table (bought followers, follow/unfollow bots, engagement pods, mass-DM automation, comment bots, reposting). Reason is practical as much as ethical: Instagram detects these and the penalty is permanent reach suppression, which would end the sprint rather than accelerate it.

### 1.3 Niche shortlist and selection

**What:** Eight niches scored on four axes (Speed, Ship, Send, Ceiling) in NICHES.md. User selected **A — AI Tools & Workflows**.

**Why those axes:** Speed and Ceiling are the obvious ones. "Ship" was added because a 48-hour sprint is production-constrained, not idea-constrained. "Send" was added because of the §1.1 finding that sends drive reach.

**Why A was recommended:** the operator has three paid AI subscriptions and builds software. Most competing accounts in this niche are affiliate marketers who do not use the tools they promote. Showing real output is a defensible advantage that cannot be copied cheaply.

### 1.4 Git repository setup

**What:** Initialized a standalone git repo in the project folder, pushed to github.com/praneeth132006/Famous.

**How:** `git init` inside the project directory, then remote add and push.

**Why not just commit:** `git rev-parse --show-toplevel` returned `/Users/praneeth`. The user's entire home directory is a git repository, so a plain `git add` from this folder would have staged files into the home-directory repo instead. Checked before committing specifically because of this class of mistake. Logged as a durable learning in gstack.

### 1.5 Narrowing the niche

**What:** Reduced "AI tools" to "one AI workflow per video that replaces something you currently do by hand."

**Why:** The §1.1 research found that narrow topics get matched to audiences more aggressively than broad ones. "AI tools" also collides with thousands of listicle accounts, and listicles generate no sends because nobody DMs a list to a specific friend. A workflow with a visible result does get sent, because the sender can point at the outcome.

### 1.6 Strategy and content queue

**What:** Wrote STRATEGY.md (positioning, send test, five pillars, structural rules, cadence, legitimate tactics, pipeline, measurement) and CONTENT-QUEUE.md (10 fully-scripted Reels, handle options, bio, 20-hook rotation).

**Key decisions and their reasons:**
- **Five rotating pillars** rather than one repeated format — defends against the near-duplicate penalty from §1.1 while still building a recognizable identity.
- **Length bands 12–25s and 35–50s, never 26–34s** — the middle band loses loop completion without gaining enough depth to justify the length.
- **Iteration metric is sends per 1000 views, not follower count** — follower count in the first 24 hours is noise and would cause the wrong format to be kept.
- **Collabs flagged as the highest-leverage free tactic** — a Collab post appears on two grids and pools reach from both audiences. It is the only genuine accelerant available without spending money, and most accounts skip it.

### 1.7 Browser setup

**What:** Launched gstack's headed Chromium and opened six tool tabs for the user to log into.

**How:** `browse connect`, then `newtab` per URL, then `focus`.

**Why a headed browser:** the user needs to sign in themselves. Credentials never pass through the agent, so a visible window the user can type into is required. Cookies then persist in that browser profile for the rest of the session.

**Why not Aside:** Aside is gstack's preferred driver (it uses the user's real browser and real sessions) but is not installed on this machine. Told the user once, then fell back.

### 1.8 Tool failures found and resolved

**CapCut — dead, permanently.**
`https://www.capcut.com/` returns **HTTP 451 Unavailable For Legal Reasons**. This is the Indian government's ByteDance ban, not a bad URL or a temporary outage. Confirmed with curl against both `/` and `/login`. CapCut cannot be used from India and no amount of retrying will change that. Tabs closed.

**ElevenLabs — renders blank.**
The sign-in page loads (title and DOM are present, including a Cookiebot consent banner) but the viewport screenshot is entirely white. The SPA does not render in this Chromium build. Not worth debugging, because §1.9 removes the need for it.

### 1.9 Toolchain revision

Replacing the two broken tools turned out to simplify the pipeline rather than complicate it.

| Job | Was | Now | Why |
|---|---|---|---|
| Video assembly | CapCut | **ffmpeg** (local) | No geo-block, no login, no watermark, and fully scriptable by the agent. |
| Captions | CapCut auto-caption | **ffmpeg burn-in from our own script** | We write the scripts, so exact text and timings already exist. Auto-transcription was solving a problem we do not have, and it mangles tool names. |
| Voiceover | ElevenLabs | **macOS `say`** | Free, unlimited, offline, no login, scriptable. No 10k-character monthly cap. |
| Covers, profile picture | Canva | **Canva** (unchanged, already logged in) | Works fine. |
| Screen recording | — | **macOS Cmd+Shift+5** | Built in, free. |
| Stock footage | Pexels | **Pexels** (unchanged) | No login needed. |

**Net effect:** the pipeline now depends on one third-party login (Canva) instead of three, and the agent can drive assembly end to end without a browser.

**Two prerequisites this creates, both free:**
1. `brew install ffmpeg` — not currently installed.
2. Download premium system voices: System Settings → Accessibility → Spoken Content → System Voice → Manage Voices. Currently only the 43 basic voices are installed, and the basic ones sound obviously robotic. The Premium tier (Ava, Zoe, Evan) is close to commercial TTS quality and is a free download.

### 1.10 Username availability could not be checked programmatically

**What:** Attempted to check five candidate handles by requesting `instagram.com/<handle>/`.

**Result:** Inconclusive, and reported as such. All candidates returned HTTP 200 with the title "Instagram" — but so did a deliberate nonsense control (`@zzzqx9v8w7nonexistentzz`) and a known-existing control (`@instagram`). Instagram serves an identical login wall for every profile URL to logged-out clients, so the response carries no signal.

**Resolution:** availability shows live in the signup form as the user types. No workaround needed, and no point building one.

### Open items at end of session 1

- User to finish logins (Instagram signup in progress, Canva done, Meta Business Suite pending).
- User to pick a handle.
- Decide on voiceover: synthetic vs the user's own voice.
- `brew install ffmpeg`.
- Nothing has been posted. Nothing will be posted without explicit confirmation.
