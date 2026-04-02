# CLAUDE.md — Luke Rall Site Context

This file gives Claude (and any AI tool) full context about this project so it can work autonomously without needing re-explanation each session.

---

## What This Site Is

A personal portfolio and founder brand site for **Luke Oliver Rall** — a 21-year-old South African entrepreneur building companies at the intersection of media, tech and business. Tagline: **"Born to Build *.*"**

**Live URL:** https://lukeoliverrall.com
**Vercel URL:** https://luke-rall-site.vercel.app
**GitHub Repo:** https://github.com/lukerall/luke-rall-site
**Owner email:** owner@lukerall.com

Deployment is automatic: pushing to `master` triggers a Vercel redeploy within ~60 seconds.

---

## Tech Stack

| Layer | Tool |
|-------|------|
| Framework | Astro 6.1.1 |
| CMS | TinaCMS 3.7.0 |
| Styling | Custom CSS (no Tailwind, no CSS framework) |
| Image processing | Sharp |
| Content format | MDX + Markdown |
| Node requirement | >=22.12.0 |

**Dev server:** `npm run dev` (runs `tinacms dev -c "astro dev"`)
**Build:** `npm run build` (runs `tinacms build && astro build`)
**Output:** `/dist/`

---

## File Structure

```
luke-rall-site/
├── public/
│   ├── headshot.jpg          ← Luke's profile photo (used in Hero + About)
│   ├── rocket.png            ← Rocket image (original HTML hero, not currently used)
│   └── favicon.svg / .ico
├── src/
│   ├── pages/
│   │   ├── index.astro       ← Main landing page (imports all section components)
│   │   ├── about.astro       ← Standalone about page (minimal)
│   │   └── blog/
│   │       ├── index.astro   ← Blog listing
│   │       └── [...slug].astro ← Dynamic blog post routes
│   ├── components/
│   │   ├── Nav.astro         ← Fixed nav with hamburger, back-to-top
│   │   ├── Hero.astro        ← Two-column hero with photo, stars, particles
│   │   ├── About.astro       ← Two-column: headshot + text + stat cards
│   │   ├── Skills.astro      ← 4-column skill card grid
│   │   ├── Ventures.astro    ← 2-column venture cards (RMG + Pinnacle)
│   │   ├── Media.astro       ← Rumble embed + social content cards
│   │   ├── Newsletter.astro  ← Email signup (light background section)
│   │   ├── Connect.astro     ← Social links + contact form (light bg)
│   │   └── Footer.astro      ← 3-column footer (embedded inside Connect)
│   ├── styles/
│   │   ├── landing.css       ← ALL landing page styles (~650 lines)
│   │   └── global.css        ← Blog page styles
│   └── content/
│       ├── blog/             ← Placeholder/example blog posts
│       └── posts/            ← TinaCMS-managed blog posts
├── tina/
│   └── config.ts             ← TinaCMS schema (blog post fields)
├── astro.config.mjs
├── tsconfig.json
└── CLAUDE.md                 ← This file
```

---

## Design System

**All styles live in `src/styles/landing.css`.** There is no Tailwind. Do not add Tailwind.

### CSS Variables
```css
--red: #e63946          /* primary accent — used everywhere */
--red-glow: rgba(230,57,70,0.35)
--red-faint: rgba(230,57,70,0.08)
--dark: #06060f         /* page background */
--dark2: #0d0d1e
--dark3: #121228
--dark4: #18183a
--mid: #1f1f45
--light-bg: #f2f2f8     /* used in Newsletter + Connect sections */
--light-bg2: #e8e8f5
--text-muted: #8888aa
--text-mid: #c0c0d8
```

### Typography
- **Body:** `Inter` (Google Fonts, weights 300–900)
- **Headings/Logo:** `Space Grotesk` (Google Fonts, weights 400–700)

### Animation Classes
- `.reveal` — starts invisible (opacity 0, translateY 36px), animates in via IntersectionObserver when element enters viewport
- `.d1`, `.d2`, `.d3`, `.d4` — stagger delays (0.1s, 0.2s, 0.3s, 0.45s)
- `.reveal.in` — triggered state (opacity 1, transform none)
- IntersectionObserver is set up in `src/pages/index.astro` with threshold 0.10

### Responsive Breakpoints
- `1100px` — hero collapses to single column
- `960px` — about-grid, ventures, media collapse
- `700px` — mobile nav (hamburger), single column everything

---

## Section-by-Section Reference

### Hero (`src/components/Hero.astro`)
- Two-column layout: text left, photo right
- 120 randomly generated twinkling stars (JS in `is:inline` script)
- 22 red floating particles
- Parallax on `.hero-name` on scroll
- Glow cursor (desktop only, set up in index.astro)
- Stats: 2 Companies | 21 Years Old | 🇿🇦→🇺🇸 2026
- CTAs: "Explore My Ventures" → `#ventures` | "Watch Latest Interview" → `#media`

### About (`src/components/About.astro`)
- **Two-column grid** (300px image | 1fr text) — do NOT simplify this to single column
- Left: `/public/headshot.jpg` with gradient border (`::before`) and offset shadow border (`::after`)
- Right: Eyebrow "Who I Am", h2 "Born to Build.", two paragraphs, tag pills, stat cards
- Stat cards: 2 Companies | 21 Years Old | 🚀 Always Moving
- History note: A previous Cursor refactor (commit `cc60a6c`) stripped this to a plain text block. It was restored (commit `4781172`). Do not revert to the stripped version.

### Skills (`src/components/Skills.astro`)
- 4-column grid of skill cards
- Each card: icon, title, description
- Skills: Digital Marketing, Content Strategy & Production, Creative Direction, AI Tools & Automation, No-Code / Low-Code Building, Brand Building, Entrepreneurship & Scaling, Public Speaking & Media

### Ventures (`src/components/Ventures.astro`)
- 2-column card grid
- **Rall Media Group** — Founder & CEO — rallgroup.net
- **Pinnacle Creatives** — Co-Founder — world-leading marketing for construction companies
- Each card has SVG logo, role pill, description, metrics, link

### Media (`src/components/Media.astro`)
- Featured Rumble embed (Professor Arno Wingen × Luke Rall interview)
- 3 social content cards (X, Instagram, LinkedIn)
- 3 social stream cards with platform icons and CTAs

### Newsletter (`src/components/Newsletter.astro`)
- Light background section (contrast break in page)
- Email input + subscribe button
- Client-side only — no backend connected yet

### Connect (`src/components/Connect.astro`)
- Light background section
- Left: 5 social link cards (X, Instagram, LinkedIn, Rumble, Email)
- Right: Contact form (name, email, message)
- Both form and newsletter are client-side only — no backend yet

### Footer (`src/components/Footer.astro`)
- Embedded at bottom of Connect section
- 3-column: brand bio | nav links | social icons
- Copyright: © 2026 Luke Oliver Rall

---

## Content & Copy

**Key personal details:**
- Full name: Luke Oliver Rall
- Age: 21
- From: Durban, South Africa
- Moving to: USA in 2026
- Email: luke@rallgroup.net
- Twitter/X: @LukeOliverRall
- Instagram: @lukerall
- LinkedIn: luke-oliver-rall-138568171
- Rumble: @LukeOliverRall

**Companies:**
- Rall Media Group (Founder & CEO) — rallgroup.net
- Pinnacle Creatives (Co-Founder) — marketing agency for construction industry

---

## Git Workflow

**Branch:** `master`
**Remote:** `https://github.com/lukerall/luke-rall-site.git`
**Git user:** Luke Rall / owner@lukerall.com

The remote URL contains a GitHub Personal Access Token embedded for automated pushes. Do not log or expose this URL.

**Standard commit flow:**
```bash
git add <files>
git commit -m "Description of change"
git push origin master
```

If a `.git/index.lock` file exists (stale lock from a crashed process), it is safe to delete it before committing.

---

## Known Issues / Things Not Yet Built

- Newsletter form has no backend — submissions do nothing server-side
- Contact form has no backend — submissions do nothing server-side
- Privacy policy link in footer goes to `#` (not built)
- `src/consts.ts` has placeholder `SITE_TITLE` and `SITE_DESCRIPTION` (not wired to meta tags)
- The standalone `/about` page is minimal and not heavily used

---

## History & Context

This site was originally built as a single HTML file (`luke-oliver-rall.html`) by Claude. It was then converted into an Astro component-based site. Cursor (AI coding tool) has been used for various edits, some of which degraded the design — notably a refactor that stripped the About section of its image and stat cards. Claude (via Cowork) restored this.

**Rule of thumb:** When in doubt about what a section should look like, the original `luke-oliver-rall.html` file (in the uploads folder) is the design reference. The Astro site should match or improve on that reference, not simplify it.

---

## Goals & Objectives

1. **Primary goal:** Professional founder brand site that positions Luke as a serious entrepreneur
2. **Tone:** Premium, dark, bold — not a typical dev portfolio
3. **Key impressions to make:** Young but credible, South African roots, US ambitions, media-savvy
4. **Future work:**
   - Connect newsletter/contact forms to a backend (e.g. Resend, Formspree)
   - Add more blog content via TinaCMS
   - Potentially add a dedicated Ventures page
   - Keep design polished — avoid letting AI tools simplify or strip visual elements
