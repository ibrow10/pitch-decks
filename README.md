# Pitch Decks — Static HTML Project

This repository contains a set of highly‑polished, single‑file HTML pitch decks built with Tailwind CSS. Each deck shares consistent navigation, responsive behavior, and typography so you can rapidly compose investor‑ready presentations without a full web app framework.

## What’s Here
- `fineo.html`: Investor deck with refined navigation, redesigned slides (Unique Position, Traction, Why Now).
- `kella.html`: Deck aligned to the shared nav and mobile behavior.
- `latinum.html`: Baseline reference for slide mechanics and styling.
- `devday.html`: DevDay’s primary deck (terminal‑green style) with new “Flight Simulator” slide, improved GTM, a restyled “Why Now”, and world‑class reveal animations.
- `devday_alt.html`: Experimental, graphics‑forward variant (mesh gradients, glass cards, inline radar SVG) to explore a radically different visual language.
- `people/…`: Team images used by multiple decks.
- `public/…`: Shared images and media assets.

All decks are self‑contained and can be opened directly in a browser. No build step required.

## Shared Navigation & Behavior
- Arrow buttons at left/right; keyboard navigation with ArrowLeft/ArrowRight.
- Slide counter in the footer.
- Desktop: horizontal deck transitions (`translateX` across `100vw` slides).
- Mobile: vertical, natural scrolling with CSS scroll‑snap; compact header.
- Subtle, reduced‑motion‑aware reveal animations (DevDay) with IntersectionObserver.

## Styling
- Tailwind via CDN for speed and portability.
- Deck‑specific accent colors and minimal custom CSS per file.
- SVG‑based, license‑free graphics (e.g., radar/HUD panels) for crisp visuals.

## File Structure
- Root HTML files are independent; edits to one will not affect others.
- Reusable assets live under `public/` (generic media) and `people/` (team images).

## Local Preview
Open any deck directly in your browser:
- macOS Finder: double‑click `devday.html` (or any deck file)
- Terminal: `open devday.html`

## Editing Notes
- Keep slides modular and concise: 1 core idea per slide.
- Prefer SVG or CSS graphics; fallback to images in `public/`.
- For new team members, add square/cropped images to `people/` for consistency.
- Maintain reduced‑motion safeguards for animations.

## Adding a New Deck
1. Duplicate a deck that’s closest to your target style (e.g., copy `devday.html`).
2. Update the accent color, header brand, and slide copy.
3. Replace images in `people/` and `public/` as needed.
4. Validate desktop and mobile behaviors (arrows, keys, scroll‑snap).

## Roadmap (Project‑Level)
- Unify the navigation/animation helpers into a light `deck.js` you can embed across files.
- Add a compact “print/PDF” stylesheet for cleaner export without a headless browser.
- Create a minimal theme system (accent, header font, background pattern) shareable across decks.
- Provide a script to generate a new deck skeleton with your preferred style.

---

# Productization Plan (Transcript → Deck)

Below is the plan to evolve these static decks into a product where a user drops a call transcript and the system generates a polished, themed investor deck.

## User Flow
- Upload: paste transcript, drag a file, or link a meeting recording.
- Extract: distill Problem, Solution, Why Now, ICP/GTM, Traction, Team, Ask.
- Compose: build a slide outline + per‑slide copy and key numbers.
- Style: choose a theme (current decks + experimental), set brand color/logo.
- Render: generate HTML/Tailwind deck, export PDF/PNG, optional PowerPoint.
- Edit: inline tweaks in a simple WYSIWYG or YAML/JSON panel, then re‑render.

## Core Architecture
- Frontend: Next.js (App Router) + TypeScript + Tailwind; basic preview + editor.
- Backend: Node APIs (Next routes / tRPC) on Vercel or Cloudflare Workers.
- LLM: OpenAI JSON‑schema outputs for deterministic steps (extraction → outline → copy → assets).
- Data: Postgres (Supabase) for projects, transcripts, versions; pgvector if adding RAG.
- Storage: S3/Supabase for images/exports; cache intermediate LLM results.
- Rendering: React/Handlebars templates → HTML/Tailwind; Playwright/Puppeteer for PDF/PNG; optional PPTX via PptxGenJS.

## LLM Pipeline (Deterministic)
1. Clean Transcript: segment speakers; normalize text.
2. Extract Entities: company, product, stage, ask, ICP, geo, numbers, quotes.
3. Build Outline: slide list with objectives and counts.
4. Slide Specs: per‑slide JSON (title, bullets, callouts, visuals).
5. Copywriter Pass: concise VC‑grade text (headlines/subheads/bullets).
6. Style Map: theme + accent color + icon/graphic set.
7. Render: slide JSON + theme → deck output.

## Templates & Themes
- Encode slides as reusable components: e.g., `WhyNow3Up`, `GTM3Card`, `SimulatorHUD`.
- Programmatic SVGs for visuals; pass color/theme as props.

## Editing UX
- Dual view: left JSON/YAML, right live preview.
- Inline contenteditable for quick headline/bullet changes that sync to JSON.
- Version snapshots per render; simple diffs.

## Exports
- HTML deck, PDF (Playwright), per‑slide PNG images, optional PPTX for enterprise.

## Security & Privacy
- Encrypt at rest; short retention options; disable training data logging.
- Optionally mask emails/phone numbers in outputs.

## Tech Choices
- Frontend: Next.js + Tailwind + Framer Motion.
- Backend: Next API routes or Workers; Zod validation.
- LLM: gpt‑4o‑mini for fast iterations; higher‑quality model for final copy.
- Embeddings: `text-embedding-3-large`; pgvector.
- Rendering: Playwright/Chromium in a serverless or lightweight container worker.

## MVP Scope (2–3 Weeks)
- One theme (current DevDay style) and ~10–12 slide types.
- Transcript upload → outline → deck copy → HTML render → PDF export.
- Minimal editor + project storage.

## Next Steps
- Define slide JSON schema and prompt set.
- Scaffold Next.js app with one theme + components.
- Implement LLM pipeline with strict JSON output.
- Add Playwright export and basic project/asset storage.

> Note: This README is not yet committed upstream. Use it for local planning; we will publish once the initial scaffolding is in place.

