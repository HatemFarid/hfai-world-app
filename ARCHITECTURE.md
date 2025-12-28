# HFAi.world — Architecture (Source of Truth)

## 1) Purpose
HFAi.world is the public cinematic “World Shell” of the HFAi Universe.
It is designed to feel alive, navigable, and meaningful — while remaining safe, fast, and deployable.
The authenticated deeper app will live later under: app.hfai.world.

## 2) Repos & Ownership
### Source of Truth (Canon / Specs)
Repo: HFAi-Core
Path: /projects/hfai-world
Contains: vision, sitemap, architecture, UX copy, design tokens, system rules.

### Implementation (Production Code)
Repo: hfai-world-app
Contains: frontend code, assets, data bundles, serverless endpoints, deployment config.

Rule:
- Specs evolve in HFAi-Core.
- Code ships from hfai-world-app.
- Any feature change requires updating both: (spec first, then code).

## 3) Runtime Architecture (High Level)
Client (Browser SPA)
- Cinematic UI + Universe Map
- Magic Lens (global navigation + discovery)
- Persistent Audio Controller (no restart across navigation)
- Local Vault (localStorage for MVP)
- Language toggle (EN default, AR optional)

Serverless Layer (Proxy)
- Gemini API calls via /api/gemini
- Optional logging + rate limits
- Prevent API key exposure
- Future: auth + database integration

Data Layer (MVP)
- Bundled JSON for Personas/Teams (read-only)
- LocalStorage for Vault projects + demo registration

Future Data Layer (Phase 2+)
- Auth (Firebase/Auth0/Custom)
- Database (Firestore/Supabase/Postgres)
- Vault storage per user
- Role-based access & permissions

## 4) Core UX Systems
### 4.1 Universe Map
- Central “HFAi Core” planet (only element using the HFAi signature accent)
- Orbiting planets (Personas, Prompt Engine, Labs, Vault, Knowledge, Applications)
- All planets clickable with consistent hit-testing
- Smooth transitions without page reload

### 4.2 Magic Lens (Global Command UI)
- Always available
- Category + item selectors (Personas / Knowledge Teams / Applications / Labs)
- Shows short preview + action buttons:
  - Navigate
  - Copy link
  - Email support (world@hfai.global) with pre-filled subject

### 4.3 Persistent Audio
- Single audio instance for the entire session
- Floating control bar:
  - Play/Pause
  - Mute/Unmute
  - Restart
- State persistence:
  - isPlaying, isMuted, currentTime

## 5) AI Integration (Gemini)
Rule: No keys in frontend.
All AI requests go through serverless proxy.

Endpoint: /api/gemini
Inputs:
- userText
- mode (Lab01, PersonaChat, OmniPrompt)
- language (en/ar)
- optional: personaId / teamId

Outputs:
- structured JSON (title, sections, bullets, prompts)
- safe, filtered responses
- optional: TTS-ready text

## 6) Voice (Phase 1.5)
MVP Voice Approach:
- SpeechRecognition (browser) for input
- SpeechSynthesis (browser) for output
- AI response via /api/gemini then read aloud
Fallback:
- always allow text input/output

## 7) Security & Safety
- No private keys in client code
- Rate limiting in serverless
- Basic abuse guardrails (length limits, forbidden content filters)
- Remove unknown/undescriptive personas or GPT entries from public UI

## 8) Deployment
Target: hfai.world
Hosting options:
- Vercel (recommended for Next.js)
- Netlify
- Cloudflare Pages

Assets:
- /public/assets/audio/background.mp3 (drop-in replacement)

## 9) Roadmap
Phase 1 (Now): Cinematic shell + map + lens + directory + demo labs + local vault.
Phase 2: Auth + database vault + user profiles.
Phase 3: Full Persona Realm (voice-first) + paid tiers + permissions.
