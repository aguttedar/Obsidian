# My Projects

> Last Updated: April 2026

---

## Project 1: Charcha

### What It Is
A visual podcast experience for users. When a video podcast is playing and the host/guest discuss a topic, the app automatically shows relevant articles, news, blog posts, and research papers in an interactive way. Users can explore the linked content without losing their place in the podcast.

**Status:** 🟡 In Progress (MVP stage)

---

### Tech Stack
- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Node.js / Express
- **AI/ML:** Ollama (Llama 3.2 local), Whisper (local transcription), spaCy NER
- **APIs:** YouTube Data API v3, Podcast Index API, Serper, AssemblyAI, Groq
- **Hosting:** Local (ngrok for sharing)
- **Version Control:** GitHub — `aguttedar/Charcha` (private)

---

### Core Features Built
- Homepage with cinematic Devanagari intro animation (च zooms in → zooms out → "Charcha" fades in → moves to top)
- Apple-style smooth scroll with 3 sections:
  - **Section 1** — Hero: *"Podcast, deeply Understood"*
  - **Section 2** — Why Charcha (Hindi meaning of the word)
  - **Section 3** — Start Listening (URL input + Library/Login)
- Podcast Library page (YouTube trending + search)
- Player page with 2 modes:
  - **Static Mode** — Article panel on the right side
  - **Interactive Mode** — Bubbles around the video *(planned)*
- Custom YouTube embed player with full controls
- Resizable article sidebar
- Progressive article loading (first 5 min fast, rest in background)
- Server-side caching for YouTube API quota management

---

### Bottlenecks

#### 1. Article Relevance (Major)
Articles had nothing to do with the video. Example: watching an Ali Abdaal wealth video, the app showed *"Using Easy Mode on my Galaxy phone"*.
- **Root cause:** Keywords were extracted without any video context
- **Fix:** Added video title/theme as context to every Serper search query + added exclusion terms

#### 2. Timestamp Accuracy
Articles showing wrong timestamps — not matching when the topic was actually discussed.
- **Root cause:** Ollama extracted topics but didn't link them to actual transcript timestamps
- **Fix:** Chunked processing — split transcript into 5-min chunks, each chunk's topics inherit that chunk's start timestamp

#### 3. Only Processing First 2 Minutes
All articles had early timestamps because the backend only transcribed the beginning of the video.
- **Fix:** Full progressive chunked processing across the entire video duration

#### 4. User Wait Time
Processing a full 200-min podcast takes 15–30 min — users can't wait.
- **Fix:** Process first 5 min immediately (articles appear in ~30 sec), then silently process the rest in background chunks

#### 5. Duplicate Articles
Same article or same topic appearing multiple times in the sidebar.
- **Fix:** Deduplication logic — filter same URLs, compare title similarity (>80% match = remove), max 2 articles per topic

#### 6. Low Quality Article Sources
Too many Wikipedia and Reddit results, not enough quality content.
- **Fix:** Source tier system:
  - **Tier 1 (Priority):** NYT, WSJ, TechCrunch, Forbes, Medium, Substack, HBR, arXiv, Bloomberg, Wired
  - **Tier 2 (Allowed):** Wikipedia (last resort)
  - **Tier 3 (Blocked):** Reddit, Quora, Pinterest, Facebook

#### 7. YouTube API Quota Exceeded
Hit the 10,000 units/day free limit during development.
- **Fix:** Created a new API key + added server-side caching (search: 1hr, video details: 24hr, trending: 2hr)

#### 8. YouTube Player Aesthetics
The default YouTube embedded player didn't match Charcha's dark aesthetic.
- **Fix:** Built a custom player wrapper using YouTube IFrame API with custom HTML/CSS controls (play/pause, progress bar, volume, speed, fullscreen)

#### 9. Intro Animation Alignment on Mobile
The Devanagari character च appeared off-center on mobile and vibrated on refresh.
- **Fix:** Switched from font-size scaling to `transform: scale()` which avoids layout shifts

#### 10. Header Overlapping Content on Scroll
The "च Charcha" logo scrolled down the page and overlapped with hero text.
- **Fix:** `position: fixed`, `z-index: 1000`, opacity controlled via Intersection Observer (visible on sections 1 & 3, hidden on section 2)

#### 11. Sticky Header Transition Was Jumpy
On the Library page, switching from full header to sticky header during scroll felt jarring.
- **Fix:** CSS transitions on all changing properties (opacity + transform), using `translateY` instead of position jumps

#### 12. Background Image Stretching
Background image distorted on wide/fullscreen browser windows.
- **Fix:** `background-size: cover` + `background-position: center`

#### 13. Claude Code Not Found After Mac Restart
Terminal couldn't find the `claude` command after restarting the Mac even though it was installed.
- **Root cause:** PATH not reloaded after restart
- **Fix:** `source ~/.bash_profile` (or reinstall via npm)

#### 14. Audio Download From YouTube Blocked
Backend tried to download audio directly from YouTube URL which YouTube blocks.
- **Fix:** Used `yt-dlp` to extract the audio stream URL first, then passed it to AssemblyAI

#### 15. Article Click Opening External Link Instead of Timestamp
Clicking an article card was opening the URL instead of jumping the video to the relevant timestamp.
- **Fix:** Main card click = `seekTo(timestamp - 2 seconds)`, small ↗ button in top-right corner = open external link. The −2 second offset ensures the user hears the lead-up to the topic

---

### Open Items / Planned
- [ ] Interactive Mode (article bubbles around video)
- [ ] Semantic search implementation
- [ ] User authentication (Login)
- [ ] Save favourites + watch history
- [ ] Sonar API (Perplexity) for smarter article search
- [ ] Twitter/X article links
- [ ] Captions button for the player
- [ ] Request YouTube API quota increase before public launch
- [ ] Mobile app (iOS) — future phase
- [ ] Deploy publicly (Vercel or Railway)

---

### Design Philosophy
- Dark background, white text — calm, soothing aesthetic
- Perplexity-inspired typography
- Sunset/warm background image with golden accent colours
- Devanagari script in logo: **च Charcha** — *"Charcha" means "discussion" in Hindi*

---

*More projects coming soon...*
