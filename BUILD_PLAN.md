# Uncle Inc. — Build Plan

## 1. PRODUCT

Uncle Inc. is a pre-launch marketing site for an AI-assisted MVP platform that lets solo founders go from idea to validated prototype in days instead of months. The landing page exists to convert one specific action: capture qualified waitlist emails from would-be users. The product goal is a complete, deployable Next.js 15 marketing site (no dashboard, no auth) with a real, working waitlist endpoint — every section on the page drives toward that single conversion.

The pain the site addresses: non-technical founders with ideas sit stuck between "I have a concept" and "I have something a user has actually clicked on" because building an MVP traditionally requires a technical co-founder, weeks of setup, and capital. The page makes the alternative — AI prototyping, built-in user testing, and launch analytics — feel concrete and accessible.

## 2. WHO IT'S FOR

**ICP:** First-time and early-stage solo founders (often ex-corporate, 28–45, technical-adjacent — they can read code but not write a full app), who have a specific idea and a 30–90 day validation window. They are time-poor, skeptical of vague "AI" claims, and respond to clear, structured information over hype.

**How this shapes the product and tone:**
- **Crisp Operator archetype** → the visual system is enterprise-clean, dark, restrained, no playful gradients or illustrations.
- **Skeptical** → no fake testimonials, no invented user counts, no "as seen in" press logos. Trust is built through clarity of feature language, transparent pricing, and real working UI.
- **Time-poor** → a single, scrollable, one-page site. No nested navigation. Hero loads in under a second on a fast connection.
- **Comparison-driven** → pricing is visible above the fold-below (in the same scroll), not buried. Feature descriptions name a specific outcome, not a vague benefit.
- **Tone:** direct, slightly dry, no exclamations, no emojis, no second-person marketing fluff. Like reading a well-written spec.

## 3. LOOK & FEEL

### Visual system

- **Overall vibe:** enterprise SaaS dark mode — think Linear, Vercel, PostHog. Restrained, dense, information-forward. No illustrations of rockets, lightbulbs, or abstract gradients. Optional subtle radial gradient at the very top of the hero (cobalt fading to navy) to add depth without being decorative.
- **Color palette (Tailwind `extend.colors`):**
  - `navy: #0a0a0f` — page background, primary surface
  - `cobalt: #1e3a5f` — card surfaces, elevated panels, subtle borders
  - `slate: #64748b` — muted body text, captions, dividers
  - `green: #10b981` — primary CTAs, "Most Popular" badge, success state, focus rings
  - White (`#ffffff`) — primary heading and body text
  - Off-white (`#e2e8f0`) — secondary text on dark surfaces
  - Border default: `rgba(100, 116, 139, 0.2)` (slate at 20%)
- **Typography:**
  - Font: Inter, loaded via `next/font/google` with weights 400, 500, 600, 700; applied via `className` on `<html>`.
  - Hero h1: `text-5xl md:text-6xl font-semibold tracking-tight leading-[1.05]`
  - Section h2: `text-3xl md:text-4xl font-semibold tracking-tight`
  - Card title: `text-lg font-semibold`
  - Body: `text-base text-slate-300 leading-relaxed`
  - Eyebrow / overline: `text-sm font-medium uppercase tracking-wider text-green-400`
- **Spacing & layout:**
  - Max content width: `max-w-6xl` for most sections, `max-w-3xl` for prose (FAQ, CTA copy).
  - Vertical rhythm: sections separated by `py-20 md:py-28`.
  - Container: `px-6 md:px-8`.
- **Key components:**
  - **Button (primary):** `bg-green-500 hover:bg-green-400 text-navy font-semibold px-5 py-2.5 rounded-md transition-colors` with a 2px green glow on hover (`shadow-[0_0_24px_-6px_rgba(16,185,129,0.5)]`).
  - **Button (secondary/ghost):** `border border-slate-700 hover:border-slate-500 text-white px-5 py-2.5 rounded-md transition-colors`.
  - **Card:** `bg-cobalt/40 border border-slate-800 rounded-xl p-6` (cobalt at 40% opacity over navy = a slightly lifted dark surface). On hover: `hover:border-slate-700 hover:bg-cobalt/60 transition-colors`.
  - **Badge (Most Popular):** `text-xs font-semibold uppercase tracking-wider bg-green-500/10 text-green-400 border border-green-500/30 px-2.5 py-1 rounded-full`.
  - **Section header pattern:** small green eyebrow → h2 → muted subhead (`max-w-2xl text-slate-400`), centered.
  - **Iconography:** Lucide React icons, `h-6 w-6 text-green-400 stroke-[1.75]`. No filled icons, no emoji. Icons sit in a `h-10 w-10 rounded-md bg-cobalt/60 border border-slate-800 flex items-center justify-center` wrapper.
  - **Imagery:** none. Pure UI. No hero image, no product screenshot (none exists yet), no stock photos. The product is textually communicated.
- **Interaction & motion:**
  - Subtle entrance fade-in on sections using Tailwind `animate-fade-in` (custom keyframe in `tailwind.config.ts`: opacity 0 → 1, translateY 8px → 0, 400ms ease-out, no scroll-triggered animation library).
  - Nav becomes opaque + adds a bottom border after scrolling past 32px (controlled by a `useEffect` scroll listener adding a `scrolled` state).
  - FAQ items: smooth height transition via `grid-template-rows: 0fr → 1fr` (no JS height calc).
  - Waitlist form: input focus ring is `focus:ring-2 focus:ring-green-500/60 focus:border-green-500`. Submit button shows inline spinner during request, then success message ("You're on the list — we'll be in touch.") replaces the form. Errors render below input in `text-red-400 text-sm`.
  - No parallax, no scroll-jacking, no autoplay video.

### Screen-by-screen layout (single page, top to bottom)

**`<Nav>` (sticky, top of page)**
- Height: `h-16`, position `sticky top-0 z-50`.
- Left: wordmark "Uncle Inc." in `text-white font-semibold tracking-tight`.
- Center (desktop only, hidden on mobile): three text links — Features, Pricing, FAQ — `text-sm text-slate-300 hover:text-white`.
- Right: "Join Waitlist" button (primary, small variant `px-4 py-2 text-sm`).
- Mobile: hamburger icon (Lucide `Menu`) toggles a slide-down panel with the three links stacked + the Join Waitlist button at the bottom.
- Backdrop: `bg-navy/80 backdrop-blur-md`; on scroll adds `border-b border-slate-800`.

**`<Hero>` (first viewport)**
- `min-h-[80vh] flex items-center` with the optional subtle cobalt→navy radial gradient at top.
- Content stack, max-w-3xl, centered:
  1. Eyebrow: "AI-Assisted MVP Platform" in green.
  2. h1: "Validate Before You Build" (white, 5xl/6xl).
  3. Subtext: "AI-assisted MVP platform that helps founders validate, prototype, and launch startup ideas in days — no technical co-founder required." in `text-slate-300 text-lg md:text-xl max-w-2xl mx-auto`.
  4. Two CTAs side-by-side, `flex flex-col sm:flex-row gap-3 justify-center`:
     - Primary: "Join Waitlist"
     - Secondary (bordered): "See How It Works" — anchor link (`href="#features"`) that smooth-scrolls to Features section.
  5. One-line trust cue below buttons: "No credit card. Free tier available at launch." in `text-slate-500 text-sm`.
- No logo wall, no metrics row, no testimonial slider.

**`<Features>` (id="features")**
- Section header (centered): eyebrow "What You Get" → h2 "Everything you need to ship a validated MVP" → subhead "Six capabilities, one workflow. No glue code, no integration projects.".
- Grid: `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6`.
- Six feature cards (exact spec):
  1. **AI Rapid Prototyping** — icon: `Zap`. Description: "Describe your idea in plain language. Uncle generates a working clickable prototype — flows, screens, copy — in under an hour."
  2. **Built-in User Testing** — icon: `Users`. Description: "Share a link, watch real users complete tasks. Session recordings and friction points surface automatically."
  3. **Launch Analytics** — icon: `BarChart3`. Description: "Activation, retention, and funnel events tracked from day one. See which features people actually use."
  4. **Guided Validation** — icon: `Compass`. Description: "Structured experiments — fake-door tests, concierge MVPs, pricing probes — with templates, not blank canvases."
  5. **No Code Required** — icon: `MousePointerClick`. Description: "Visual editor for tweaks. Ship changes without opening a terminal or writing a line of code."
  6. **Iterate with Data** — icon: `RefreshCw`. Description: "Versioned prototypes with a changelog. Roll back, A/B test variants, and pick the winner with evidence."
- Each card: icon wrapper (top-left) + title + description. No card-level CTAs — the section CTA is the page-level Join Waitlist.

**`<Pricing>` (id="pricing")**
- Section header: eyebrow "Pricing" → h2 "Simple plans, no surprises" → subhead "Start free. Upgrade when you're ready to ship to real users.".
- Grid: `grid grid-cols-1 md:grid-cols-3 gap-6`.
- Three tier cards, equal width; Pro is visually elevated:
  - **Free** — `bg-cobalt/40 border border-slate-800`:
    - Title: "Free", `$0` in `text-4xl font-semibold`, "/mo" muted.
    - Bullets (Lucide `Check` icon, green): "1 MVP", "Basic prototyping", "Community support".
    - CTA button (secondary, full-width): "Join Waitlist".
  - **Pro** (highlighted) — `bg-cobalt/60 border-2 border-green-500 relative`, with "Most Popular" badge floating top-right:
    - Title: "Pro", `$29` `/mo`.
    - Bullets: "Everything in Free", "Unlimited MVPs", "Built-in user testing", "Launch analytics", "Email support".
    - CTA button (primary green, full-width): "Join Waitlist".
  - **Team** — `bg-cobalt/40 border border-slate-800`:
    - Title: "Team", `$79` `/mo`.
    - Bullets: "Everything in Pro", "Team workspaces", "Collaboration & comments", "Shared analytics", "Priority support".
    - CTA button (secondary, full-width): "Join Waitlist".
- Below grid: small muted line: "Prices in USD. Annual billing coming soon." in `text-slate-500 text-sm text-center`.

**`<FAQ>` (id="faq")**
- Section header: eyebrow "FAQ" → h2 "Questions, answered".
- Layout: `max-w-3xl mx-auto` stack of 6 items.
- Each item: full-width button row with question on left, Lucide `ChevronDown` on right (rotates 180° on open via `data-[state=open]`), bottom border `border-slate-800`. Body content: `text-slate-300 text-sm leading-relaxed pb-4`.
- Questions and answers (verbatim content, not invented metrics):
  1. **What is Uncle Inc.?** — "Uncle is an AI-assisted MVP platform for solo founders. You describe an idea; Uncle generates a working prototype, helps you test it with real users, and tracks the metrics that matter before you commit engineering time."
  2. **Do I need to code?** — "No. Prototypes are built and edited visually. If you can write a clear problem statement, you can ship a prototype on Uncle."
  3. **How does user testing work?** — "Share a private link. Uncle records sessions, tracks task completion, and highlights where users hesitate or drop off. You review the highlights instead of watching every minute."
  4. **What happens after the free tier?** — "You'll get an email when paid plans open. Existing free-tier accounts keep their projects; upgrading adds unlimited MVPs, full analytics history, and team features."
  5. **Can I export my code?** — "Yes. Every prototype exports to clean, readable React + TypeScript you can take to your own repo. No lock-in."
  6. **Is my data secure?** — "Projects are encrypted in transit and at rest. We don't train models on your prototype data. A detailed security overview ships with the public beta."

**`<CTA>` (final conversion section, id="waitlist")**
- Full-bleed dark band: `bg-cobalt/30 border-y border-slate-800`.
- Content, max-w-2xl, centered:
  - h2: "Stop Guessing. Start Validating." in white, 4xl, tight.
  - Subtext: "Join the waitlist for early access. Founders on the list get a 50% lifetime discount on Pro." in `text-slate-300`.
  - Form (client component): `flex flex-col sm:flex-row gap-3 max-w-md mx-auto`:
    - Email input: `flex-1 bg-navy border border-slate-700 text-white placeholder:text-slate-500 px-4 py-3 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500/60 focus:border-green-500`.
    - Submit button: primary green, "Join Waitlist" / spinner / "Joined ✓".
  - Below form: `text-slate-500 text-xs` "No spam. Unsubscribe in one click."

**`<Footer>`**
- `border-t border-slate-800 py-10`.
- Top row: wordmark "Uncle Inc." on left; on right (desktop) a flex row of text links: Privacy, Terms, Contact (each `text-slate-400 hover:text-white text-sm`).
- Bottom row: `text-slate-500 text-sm` "© {currentYear} Uncle Inc. All rights reserved."
- On mobile the link row wraps below the wordmark.

## 4. USER FLOWS

**Flow A — Primary: Visit → Join Waitlist (the only conversion goal)**
1. User lands on `/` from any source. Hero loads with the eyebrow, headline, subtext, and two CTAs above the fold.
2. User scrolls or clicks "See How It Works" → smooth-scrolls to `#features`.
3. User reads features, scrolls to `#pricing` (price is visible, no hidden gating), then to `#faq` to resolve objections.
4. User reaches the final CTA section. Types email into the input. Tabs to the Join Waitlist button (focus ring appears).
5. User clicks submit.
   - **State: idle** → button text "Join Waitlist".
   - **State: submitting** → button is disabled, shows a small inline Lucide `Loader2` spinner with `animate-spin`, input becomes disabled.
   - **State: success** → form is replaced with a `CheckCircle2` icon + "You're on the list — we'll be in touch." message in `text-green-400`.
   - **State: error (network/validation)** → red helper text appears below input: "Something went wrong. Try again." Button returns to idle.
6. Email is POSTed to `/api/waitlist`, validated server-side (Zod schema: `email` string, valid email format), persisted, and acknowledged.

**Flow B — Secondary: Nav quick-jump**
1. User clicks "Pricing" in the nav. Page smooth-scrolls to `#pricing`, nav stays pinned. Active link in nav gets `text-white` instead of `text-slate-300` (basic scroll-spy via IntersectionObserver).

**Flow C — Mobile menu**
1. User on mobile taps the hamburger icon. Panel slides down with a 200ms ease-out transition, the three links stack vertically, "Join Waitlist" button sits at the bottom of the panel, full-width. Tapping any link closes the panel and smooth-scrolls. Tapping outside or the close icon closes it.

**States summary**
- Nav: `default` / `scrolled` (adds border + opacity) / `mobile-open`.
- Waitlist form: `idle` / `submitting` / `success` / `error`.
- FAQ item: `closed` / `open` (independent, no accordion exclusivity).
- Pricing card: `default` / Pro is permanently `highlighted` (no hover state changing it).

## 5. PAGES / ROUTES

| Route | Type | Purpose | Layout summary |
|---|---|---|---|
| `/` | Server component (page) | Single-page marketing site. Renders Nav → Hero → Features → Pricing → FAQ → CTA → Footer in order. | Full-bleed dark, single column, max-w-6xl content. |
| `/api/waitlist` | Route handler (POST) | Receives `{ email }`, validates, persists, returns `{ ok: true }`. | JSON. 200 on success, 400 on validation error, 500 on storage error. |
| `/privacy` | Static page | Placeholder privacy page. | `max-w-3xl` prose, same nav/footer. |
| `/terms` | Static page | Placeholder terms page. | Same as privacy. |
| `/contact` | Static page | `mailto:` link + short blurb. | Same shell. |

The secondary static pages share a minimal shell (Nav + footer, single `<article>` body) to keep the build small. Their content is short and honest: "This page will be finalized before public launch. For now, email hello@uncle.example."

## 6. CORE FEATURES

The marketing site's "features" are the on-page elements that drive conversion. Each is implemented as a real, working piece of UI.

1. **Sticky Nav with scroll-aware styling and mobile menu**
   - Listens to `window.scroll` (passive), sets `scrolled` state when `window.scrollY > 32`. Adds `bg-navy/95 border-b border-slate-800` when true.
   - Mobile menu uses local `open` state; locks `document.body` overflow while open; closes on route hash change or link click.
   - Anchors use `scroll-behavior: smooth` set on `html` in `globals.css`.

2. **Smooth scroll-to-section from any in-page link**
   - `html { scroll-behavior: smooth; }` in `globals.css`, plus `scroll-margin-top: 5rem` on each section with an id, so the sticky nav doesn't cover the heading.

3. **Six-card Features grid**
   - Pure presentational. Data lives in a typed `features` array at the top of `Features.tsx` so the content is editable in one place. Each entry: `{ icon: LucideName, title, description }`.

4. **Three-tier Pricing grid with highlighted Pro**
   - Data in a typed `tiers` array. The middle item gets a `highlighted: true` flag driving the green border and badge. Rendering is fully data-driven so adding a fourth tier is a one-line change.

5. **Accordion FAQ with six questions**
   - Local `openIndex: number | null` state; clicking an open item closes it. Animation: `grid-rows-[0fr]` ↔ `grid-rows-[1fr]` on the wrapper div, body has `overflow: hidden`. No external accordion library.
   - Keyboard accessible: each trigger is a `<button>` with `aria-expanded` and `aria-controls`.

6. **Functional Waitlist form (client component)**
   - Client component (`'use client'`). Local state: `email`, `status` (`'idle' | 'submitting' | 'success' | 'error'`).
   - On submit: `fetch('/api/waitlist', { method: 'POST', body: JSON.stringify({ email }) })`.
   - Disables input + button during `submitting`.
   - On success: swaps the form for a confirmation message.
   - Validation: HTML5 `type="email" required` plus Zod-equivalent server-side check.

7. **Footer with dynamic year and secondary nav**
   - `new Date().getFullYear()` rendered at build/render time.
   - Three link routes: `/privacy`, `/terms`, `/contact` (placeholder pages).

8. **Server-side waitlist persistence**
   - The `/api/waitlist` route handler validates with a Zod schema (`z.object({ email: z.string().email().max(254) })`).
   - Stores to a flat file `./data/waitlist.json` (created on first write if missing) with `{ email, createdAt }` entries. Returns `{ ok: true, count: number }` on success.
   - Deduplicates: if email already exists, returns 200 with `{ ok: true, alreadyOnList: true }` so the UI shows the success state.
   - The file is gitignored.

9. **Entrance fade-in animation**
   - One custom keyframe in `tailwind.config.ts`: `fade-in: { '0%': { opacity: '0', transform: 'translateY(8px)' }, '100%': { opacity: '1', transform: 'translateY(0)' } }`. Duration 400ms, applied via `animate-fade-in` to each `<section>`.

10. **Active-section nav highlight (scroll-spy)**
    - `useEffect` with `IntersectionObserver` on each `[id]` section. When a section is intersecting, the matching nav link gets `text-white`; others revert to `text-slate-300`.

## 7. DATA MODEL

Only one entity exists for this marketing site. (No auth, no users, no projects — the dashboard/app is a future build.)

**WaitlistEntry**
| Field | Type | Notes |
|---|---|---|
| `email` | `string` | Lowercased, trimmed. Unique. Validated against RFC-ish email regex on the server. Max length 254. |
| `createdAt` | `string` (ISO 8601) | Server timestamp. |

Storage: a JSON file at `./data/waitlist.json` shaped as `{ entries: WaitlistEntry[] }`. The file is created with `{ entries: [] }` on first write. This is intentionally simple — the site is pre-launch and the actual product will use a proper database. The data access layer is isolated to `lib/waitlist-store.ts` so swapping to Supabase / Postgres later is a one-file change.

No relationships, no foreign keys. If the site needs to be deployed to a serverless platform where the filesystem is ephemeral, the same `lib/waitlist-store.ts` interface can be swapped to write to an env-var-configured KV store (Upstash, Vercel KV) without touching the route handler.

## 8. AUTH

**No authentication on the marketing site.** The site is pre-launch; there are no user accounts, no login, no dashboard, no protected routes. The only user-facing action is the public waitlist form, which is a POST to a public route handler with rate-limiting-by-IP at the route handler level (a simple in-memory token bucket: 5 requests / minute / IP) to prevent abuse. No third-party auth provider is configured or surfaced in the UI.

If/when the dashboard is built, the auth plan is: Supabase Auth with email + password (and optional magic link), no social providers until real OAuth credentials are provisioned. Clerk is explicitly not used.

## 9. FILES

```
package.json                              # Next 15, React 19, Tailwind 3, TS, lucide-react, zod
next.config.js                            # React strict mode, standard config
tsconfig.json                             # Standard Next 15 TS config
postcss.config.js                         # tailwindcss + autoprefixer
tailwind.config.ts                        # Content globs, custom colors, fade-in keyframe
.gitignore                                # node_modules, .next, data/waitlist.json
app/
  globals.css                             # Tailwind directives, Inter import, smooth scroll, body bg
  layout.tsx                              # Root layout, Inter via next/font, metadata
  page.tsx                                # Renders Nav, Hero, Features, Pricing, FAQ, CTA, Footer
  icon.tsx                                # Simple favicon (a green "U" SVG)
  api/
    waitlist/
      route.ts                            # POST handler, Zod validation, calls waitlist-store
  privacy/
    page.tsx                              # Placeholder privacy page
  terms/
    page.tsx                              # Placeholder terms page
  contact/
    page.tsx                              # Placeholder contact page
components/
  Nav.tsx                                 # Sticky nav, scroll-aware, mobile menu, scroll-spy
  Hero.tsx                                # Headline, subtext, two CTAs, trust cue
  Features.tsx                            # Section header + 3x2 card grid driven by data array
  Pricing.tsx                             # Section header + 3-tier grid, Pro highlighted
  FAQ.tsx                                 # Section header + 6-item accordion
  CTA.tsx                                 # Final conversion section with WaitlistForm
  Footer.tsx                              # Wordmark, links, dynamic year
  WaitlistForm.tsx                        # 'use client' email input + submit, state machine
  icons.ts                                # Re-exports of used Lucide icons (single import surface)
lib/
  waitlist-store.ts                       # read/append to data/waitlist.json, dedup logic
  rate-limit.ts                           # In-memory token bucket for /api/waitlist
data/
  .gitkeep                                # Directory placeholder; waitlist.json is gitignored
```

## 10. ACCEPTANCE

A working build meets the following:

- [ ] `npm install` succeeds; `npm run dev` starts on port 3000 with no errors.
- [ ] `npm run build` succeeds; `npm run start` serves the production build.
- [ ] Visiting `/` renders Nav, Hero, Features, Pricing, FAQ, CTA, Footer in that order, top to bottom, with no console errors and no hydration warnings.
- [ ] Background is `#0a0a0f` (navy) across the page; text is white/slate per the design system.
- [ ] Inter font is loaded via `next/font` and applied to `<html>`; no FOUT on refresh.
- [ ] Nav is sticky, becomes opaque with a bottom border after 32px of scroll, and shows three links + a Join Waitlist button on desktop.
- [ ] On mobile (≤768px), the nav shows a hamburger that opens a panel with stacked links and a full-width Join Waitlist button; the panel closes on link click.
- [ ] Hero shows the exact headline "Validate Before You Build" and the exact subtext from the spec, with a primary green CTA "Join Waitlist" and a secondary bordered CTA "See How It Works" anchored to `#features`.
- [ ] Smooth scroll works from any in-page link and from the "See How It Works" button; the section heading is not hidden under the nav.
- [ ] Features section renders exactly six cards in a responsive grid (1 / 2 / 3 columns by breakpoint) with the exact titles and descriptions from the spec, each with a Lucide icon in a cobalt wrapper.
- [ ] Pricing renders three tiers with the exact prices, features, and CTAs from the spec; the Pro tier is visually highlighted with a green border and a "Most Popular" badge; the Pro CTA is the green primary button, the others are ghost buttons.
- [ ] FAQ renders six items, each expandable/collapsible with a rotating chevron, keyboard-accessible (Tab to focus, Enter/Space to toggle, `aria-expanded` updates).
- [ ] CTA section displays "Stop Guessing. Start Validating." with the email form.
- [ ] Submitting a valid email to the waitlist form returns 200, persists the email to `data/waitlist.json` (or the configured store), replaces the form with the success message, and is idempotent on duplicate emails.
- [ ] Submitting an invalid email (missing `@`, empty) is blocked client-side by HTML5 validation, and server-side validation returns 400.
- [ ] The waitlist route is rate-limited at 5 requests/minute/IP; a 6th request inside the window returns 429.
- [ ] Footer shows the current year dynamically and links to `/privacy`, `/terms`, `/contact`; each secondary page renders with the shared nav/footer shell.
- [ ] No fake testimonials, no invented user counts, no fabricated press logos, no stock photos, no decorative illustrations anywhere on the site.
- [ ] Lighthouse a11y score ≥ 95 on `/`: all images (none) have alt, all interactive elements have accessible names, color contrast passes for white-on-navy and green-on-navy CTAs, focus rings are visible.
- [ ] No 404s in the network tab; no external network requests besides the Google Font (handled via `next/font`, self-hosted at build time).

FILES: ["package.json","next.config.js","tsconfig.json","tailwind.config.ts","postcss.config.js",".gitignore","app/globals.css","app/layout.tsx","app/page.tsx","app/icon.tsx","app/api/waitlist/route.ts","app/privacy/page.tsx","app/terms/page.tsx","app/contact/page.tsx","components/Nav.tsx","components/Hero.tsx","components/Features.tsx","components/Pricing.tsx","components/FAQ.tsx","components/CTA.tsx","components/Footer.tsx","components/WaitlistForm.tsx","components/icons.ts","lib/waitlist-store.ts","lib/rate-limit.ts","data/.gitkeep"]