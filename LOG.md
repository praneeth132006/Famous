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

---

## Session 2 — 2026-09-12 (continued)

### 2.1 Built R1 by hand, then got corrected

**What:** Produced the R1 assets (handwritten timetable, dark calendar UI, .ics code scene, profile mark) by hand-writing HTML/CSS, rendering at 1080x1920 through the browser, and generating voiceover with macOS `say` (Rishi, en_IN). Also generated a genuinely valid 15-event recurring .ics file so the demonstrated output was real, not mocked.

**Why it was wrong:** the user pays for Gemini, ChatGPT, and Google Flow, and pointed out I was hand-building assets instead of using them. Correct call. Hand-coded HTML gives accurate UI but it is slow and it is not what a paid generative stack is for.

**Kept from that work:** the .ics generator (real output, still the honest core of R1), the profile mark, and the voiceover pipeline. Discarded the approach of hand-building every visual.

### 2.2 Browser session lost on resume

The session resume killed the headed browser daemon (PID changed, all tabs and cookies gone). Relaunched with `connect --force-restart`. The underlying Chrome profile retained Google and Instagram logins, so nothing needed re-authenticating.

**Learning for future sessions:** do not assume the browser daemon survives a session resume. Re-check `browse status` and expect mode to drop from `headed` to `launched`.

### 2.3 Flow app entry point

`flow.google.com/tools/flow` 404s. The working app URL is **`labs.google/fx/tools/flow`** (which then redirects to `flow.google.com/`). The `flow.google.com/about` page is marketing only.

### 2.4 Two blocking findings in Google Flow

**A. The signed-in Google account is not the user's.**
Flow reports the active account as **Swathi — bojanapuswathi12@gmail.com**, with **1,050 Google Flow credits**. The user's own address is praneeth132006b@gmail.com. Paused rather than spending someone else's credits. Needs the user to confirm which account to use.

**B. Visible watermarking cannot be disabled.**
Flow's own settings state: *"Visible watermarking is required in your region."* Every Veo output from this account will carry a visible watermark. Not a toggle, not a tier upgrade within this plan. Relevant because it changes what Veo is usable for.

**C. Plan mismatch.** myaccount.google.com reports **Google AI Plus**, while Flow displays a **PRO** badge. Did not resolve which governs the actual model and quota limits. Credits (1,050) are the number that matters in practice.

### 2.5 Standing constraint on Veo for this niche

Veo generates cinematic footage. It does not generate accurate software interfaces. For a niche built on "here is a real AI workflow," the core asset is screen content showing a real tool producing a real result. Veo cannot produce that, and asking it to would mean inventing fake UI for an account whose entire promise is showing real output.

**Where Veo is genuinely strong for this account:** cold-open hook shots, visual metaphor, transitions, and b-roll between demo steps. That is a real upgrade over hand-coded scenes and worth using.

**Where it is not:** the demo itself.

This is a constraint on format, not a reason to avoid the tool.

### 2.6 Switched to Google Flow for generation — it works, and it is much better

**Decisions taken (user answered both):** use Swathi's account and its 1,050 credits; I screen-record real workflows on the Mac for the demo segments.

**Flow setup that works:**
- App URL: `labs.google/fx/tools/flow` (redirects to `flow.google.com`). `flow.google.com/tools/flow` 404s.
- Created a separate project `0f639106-4332-4acd-8423-b98aba8c75dc` rather than working inside Swathi's existing June projects.
- Settings: **Video · Omni 1.1 Flash · 720p · 8s · 9:16 · x1 = 12 credits per clip.** At 1,050 credits that is ~87 clips, which is far more than the sprint needs.
- The prompt input is a **contenteditable div**, not a textarea. Focus it via JS and place the caret before typing, or the text never lands.
- A changelog modal blocks the whole UI on first load. Dismiss "Get started" first.
- Most controls are generic divs with no usable text selectors. Driving them by `document.elementFromPoint(x,y).click()` off a screenshot is reliable; text selectors are not.

**Result:** generated the R1 hook shot first try. Overhead macro of a real handwritten timetable on a dark desk, blue ink lifting off and reorganizing into glowing lime calendar blocks. 8s, 720x1280, genuinely cinematic. Far better than the hand-built HTML scenes it replaces.

### 2.7 Blocked: cannot extract the MP4 out of Flow

Generation is solved. Getting the file out is not. Everything tried:

| Attempt | Result |
|---|---|
| `download` button → 1080p Upscaled | Menu opens and the click registers, but no file appears anywhere on disk |
| Project menu → "Download project" | Same, nothing written |
| Read `<video>.src` from the DOM | Element exists only transiently; `querySelectorAll('video')` returns 0 moments later |
| Deep walk through all shadow roots | `count: 0` |
| Click play first, then read the element | Still 0 |
| Network log for `flow-content` / `.mp4` | Empty — media requests are not captured |
| `browse download` on the `/asb/` asset URL | **Crashed the browse daemon twice in a row** |
| Re-capture the `/asb/` URL headless | Media never loads without a visible render |

**Read:** Chromium under CDP automation is refusing downloads to disk, and Flow's player does not expose a durable media element. This is a harness limitation, not a Flow limitation.

**The unblock:** one manual click. The headed window is open on the project. User clicks the download icon → 1080p Upscaled, the file lands in ~/Downloads, and everything after that (assembly, captions, voiceover, posting) is fully automated again.

**Do not spend more agent time on automated extraction.** It cost a large share of this session and produced nothing. One human click costs seconds.

### 2.8 Still true from earlier

ffmpeg 9.0.1 is installed and has zoompan, xfade, overlay, concat, amix, adelay. It does **not** have `drawtext` (no libfreetype), so all text must be composited as rendered PNG layers rather than burned by ffmpeg. Premium macOS voices are still not installed; only the 43 basic voices are available, and Rishi (en_IN) is the best fit for this audience.
