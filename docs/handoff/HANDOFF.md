# Allyant — design handoff → Webflow build

The brief every build session starts from. Written when the design is
approved (by the designer, or at the end of the prototype phase) and kept
current during the build. Agents read it after `AGENTS.md` and `GOTCHAS.md`.

## Read first

1. `AGENTS.md`, `GOTCHAS.md`, and the skill's `conventions.md`.
2. This file, then `MANUAL-TODO.md` and `BUILD-NOTES.md` in this folder;
   after the build handoff, `REVIEW.md` (feedback rounds until launch) and
   `SEO-PLAN.md`.
3. The approved design: `../prototype/` (localhost), built from the Figma page
   "For Dev" (https://www.figma.com/design/kQg1wvFPe0W9UO9seCB4ke/Allyant?node-id=632-120), the source of truth.

## Status

2026-11-19 — Setup done (repo facts, workspace, prototype folder, Geist +
Geist Mono v1.9.2, logo in `../Assets/brand/`). CMS plan v1 drafted (below,
in Figma › CMS Planning, and as Dev Mode annotations on every CMS section of
"For Dev"), awaiting approval. Rule added: reuse the site's components
(Icon, C | Button…) — AGENTS.md › Reuse what the site already has.
Done in the session opened in `Allyant/` with the `webflow-allyant` MCP:
1. ✅ Read-only inventory of the Webflow site against the starter list
   (Components to make › Site inventory).
2. ✅ Webflow snapshot, 2026-11-19: read through the MCP (all variables
   with every mode, all classes at every breakpoint and state, the 15
   component variant styles) and compared with
   `../prototype/webflow-snapshot/`. They match exactly, so the snapshot
   shipped with the prototype starter is the site's snapshot and was not
   rewritten. Known MCP limits, kept by hand as the README says: the
   `calc()` padding-top on Is Hero and Is Top (the read returns only Nav
   Height) and the breakpoint modes. Not in the snapshot by design: the
   Sg classes (86) and the empty combo Body L › Max Width Text. `pnpm css`,
   `pnpm check:snapshot` (8 collections, 151 variables, 89 styles) and
   `pnpm check:drift` (no drift against wf-allyant.webflow.io) all pass.
Setup committed on branch `setup/allyant` (not pushed, not merged).
Next: the style guide, divided across pages in `design › style-guide`
(Decisions, 2026-11-19), starting with the variable proposals (old → new)
for the design's colours, type, spacing and radius.

## Ground rules for this project

- Webflow writes only after a go-ahead from the person running the session.
- "For Dev" is the source of truth; what is in its frames is the final
  pick (where a section shows alternatives marked "OR", the frame placed
  in the page column is the one built).
- Figma has desktop frames only (1440). Tablet and mobile layouts are
  designed in the prototype, at 991 / 767 / 479, and approved there.
- States (hover, focus, pressed, disabled, error) are designed in the
  prototype.
- Copy: as in Figma for now; `[TBC]` text stays visible as a placeholder.

## Pages

| URL | Design source | Webflow page | Notes |
| --- | --- | --- | --- |
| `/` | Figma `632:121` HOME | Home | |
| `/our-approach` | Figma `632:2898` | Our Approach | Brand Vision proposal (not asked for by client) |
| `/what-we-do` | Figma `632:4133` | What We Do | Services list = CMS |
| `/what-we-do/<service>` | Figma `632:4599` | Services template | CMS template |
| `/projects` | Figma `632:5224` | Projects | Projects list + filters = CMS |
| `/projects/<project>` | Figma `632:5665` (v2) | Projects template | CMS template; sections optional per project |
| `/company` | Figma `632:6719` | Company | Team, Offices = CMS |
| `/insights` | Figma `632:7442` | Insights | **Undecided — do not build until confirmed** |
| `/insights/<article>` | Figma `632:6178` | Insights template | **Undecided** |
| `/contact` | Figma `632:7838` | Contact | Offices = CMS; form |
| `/design/style-guide` | | Style guide (folder `design`) | first page built; excluded from search |
| `/design/media` | | Media (folder `design`) | only when a page plays a video; excluded from search |

Breakpoints: 991 (tablet), 767 (mobile landscape), 479 (mobile). Checked at
1440, 820, 667 and 390.

## Variables and classes

Not proposed yet (next: the style guide). What the Figma file gives us,
audited 2026-11-19 on "For Dev" (no Figma variables or text styles exist;
only `Primary/bvm-true-black`):

- **Fonts in the design:** Geist (1,255 runs: headings, body), Geist Mono
  (554: eyebrows, labels, numbers), Host Grotesk (286: buttons, card
  titles, filters), JetBrains Mono (54: project numbers/meta). Inter only
  in the annotation panels. Geist and Geist Mono v1.9.2 are in
  `../Assets/fonts/` and `../prototype/public/fonts/`.
- **Colours:** ink `#0E0F0F` (with 70/64/50/45/40 % tints), white,
  orange `#F2592B` (a near-duplicate `#F1592A` to merge), page `#F4F3F0`,
  surface `#E7E5DF`, grey `#8B9096`; small accents `#0F766E`, `#0D9755`,
  `#0D5797`, `#5E9A95`.
- **Type sizes used most:** 11 (mono labels, +2–4 % tracking), 13, 14, 15,
  16, 18, 20, 24, 32, 40, 48, 64, 80 (display, −6 % to −2.5 % tracking,
  line height 102–110 %); body 14–18 at 145–164 %. Odd values (14.5, 17.8,
  19.1, 59.9, 9.5, 10.5) round onto the scale.
- **Spacing:** gaps and paddings cluster on 4, 8, 12, 16, 20, 24, 32, 40,
  80, 120; off-grid values (9, 10, 14, 18, 22, 41) snap to the 4 px base.
  Section padding 120 top/bottom, 40 side gutter at 1440.
- **Radii:** 4, 12, 16, 20, 40, pill (60/100).

Principle: a 4 px base for every space, gap and radius; type on one scale
taken from the sizes above; colours as primitives → semantic roles.

## CMS

Plan v1, 2026-11-19 — **awaiting approval.** Drawn in Figma › page
"CMS Planning". Field names are the Webflow names; *(opt)* = optional.

**Hiding empty content.** No "Show section" switches. A section or element
that depends on optional fields is hidden with Webflow conditional
visibility ("<field> is set") on a plain wrapper div (a Designer step,
`MANUAL-TODO.md`). A section holding a filtered Collection List that can
come back empty (related projects, a project's testimonial) is hidden in
the browser by a module (`data-hide-if-empty`) because Webflow's
conditional visibility can't read a list. "Featured" uses a number field
(`Featured order`: set = featured, sorts the featured list) instead of a
switch.

| Collection | Fields | Used on |
| --- | --- | --- |
| **Services** (10) · template `/what-we-do/<slug>` | Name · Slug · Order (number, journey 1–10) · Summary (plain, cards) · Thumbnail (image) · Hero image (image) · Hero intro (plain) · Covers intro (plain) · Cover 1–5 title (plain, opt) · Cover 1–5 text (plain, opt) · Covers image (image, opt) · Situation (plain, opt) · When (plain, opt) · Compare with (multi-ref Services, opt) · Deliverables (rich text, opt) · Deliverables image (image, opt) · SEO title (opt) · Meta description (opt) | Home › Services overview (all, by Order) · What We Do › service journey (all, by Order) · Service template · Project template (delivery method) · Contact form "Project type" options |
| **Projects** · template `/projects/<slug>` | Name · Slug · Client (ref Clients) · Sector (ref Sectors) · Delivery method (ref Services) · Delivery label (plain, opt — e.g. "Construction Management – Fast Track"; falls back to the service name) · Location (plain, "Burlington, ON") · Region (option, filter) · Year (number, opt) · Size sf (number, opt) · Size range (option, filter) · Budget (plain, opt) · Status (option, opt) · Thumbnail (image) · Hero image (image) · Excerpt (plain) · About (rich text) · About image 1–2 (image, opt) · Challenge (rich text, opt) · Approach (rich text, opt) · Result (rich text, opt) · Story image (image, opt) · Outcome 1–3 label (plain, opt) · Outcome 1–3 text (plain, opt) · Systems (rich text, opt) · Scope (rich text, opt) · Gallery (multi-image, opt) · Featured order (number, opt) · Sort order (number) · SEO title (opt) · Meta description (opt) | Home › Featured projects (Featured order is set, ascending) · Projects › grid (Sort order; filters Sector / Delivery method / Size range / Region; "View more") · Service template › Related projects (Delivery method = current service) · Project template › Related projects (exclude current) · What We Do › Featured projects |
| **Sectors** (6) · no page | Name · Slug · Number (plain, "01") · Summary (plain) · Image (image) · Order (number) | Home › Industries · Our Approach › Industries · Projects filter · Project › At a glance |
| **Clients** · no page | Name · Slug · Logo (image, opt) · Website (link, opt) | Project › Client · Home › Proof of Partnership logo row (Logo is set) · Projects › Repeat clients |
| **Testimonials** · no page | Name (quote author) · Quote (plain) · Role (plain, opt) · Client (ref Clients, opt) · Project (ref Projects, opt) · Order (number) | Home › Proof of Partnership (by Order) · Project template › Client quote (Project = current; hidden when empty) |
| **Team** · no page | Name · Slug · Credentials (plain, opt — "P.Eng., PMP") · Role (plain) · Photo (image) · Bio (plain, opt) · LinkedIn (link, opt) · Email (email, opt) · Order (number) | Company › Leadership & Team (portrait swaps with active row) · Article template › Author |
| **Offices** · no page | Name (plain, "Vaughan, Ontario") · Country (option: Canada / United States) · Address line 1 · Address line 2 · Phone (phone, opt) · Map link (link) · Order (number) | Company › Offices · Contact › Offices · Footer › Offices · Contact › General enquiries (phone per country) |
| **Credentials** · no page, **content TBC** | Name · Badge (image) · Link (link, opt) · Order (number) | Footer › Credentials · Company › Safety, Quality & Credentials |
| **Insights** · template `/insights/<slug>` · **undecided** | Name · Slug · Category (ref Insight Categories) · Publish date (date) · Read time (number, minutes) · Excerpt (plain) · Hero image (image) · Body (rich text; "On this page" built from its H2s in the browser) · Key takeaway (plain, opt) · Author (ref Team, opt) · Featured order (number, opt) · LinkedIn post (link, opt) · SEO title (opt) · Meta description (opt) | Insights › Featured + grid (category filter, "View more") · Article template › Related insights (same category, exclude current) |
| **Insight Categories** · no page · **undecided** | Name · Slug · Order (number) | Insights filter · article cards |

Webflow makes a template page for every collection; the ones marked "no
page" get a 301 to their listing page (or stay in draft) and are excluded
from the sitemap.

Static (not CMS): nav, footer links, Home hero, "How Allyant runs it"
(the same five points on every service), stats (100+, 99%, 5/5), Why
Allyant cards, Our Approach and Company sections other than Team/Offices.

## Components to make

### Site inventory (read-only, 2026-11-19)

Read through `webflow-allyant` (site `6aff472323df3b1e36cc8be7`, the only
site the server reaches); nothing was written. Compared with the starter's
list in `AGENTS.md` › Reuse what the site already has and the
webflow-build skill's `starter-site.md` (A–D2). Pages on the site: Home,
Style Guide (`/design/style-guide`), Media (`/design/media`), 404.

**Components (13, all from the starter, none from a library):**

| Component | Group | Props | Variants | Instances |
| --- | --- | --- | --- | --- |
| G \| Components | Global | — | Base | 4 |
| G \| Page W | Global | Slot (slot) | Base | 4 |
| G \| Nav W | Global | — | Base | 4 |
| G \| Footer W | Global | — | Base | 4 |
| Icon | Controls | Variant, Icon (`ph ph-arrow-right`) | Inherit (base), Primary, Secondary, Accent, Success, Warning, Error | 7 |
| Icon Size | Controls | Variant, Icon | 12px, 16px (base), 20px, 24px, 32px, 48px, 64px | 29 |
| C \| Video | Controls | MP4, Poster, WebM, Has WebM; Autoplay, Muted, Loop, Plays Inline | Base | 1 |
| Button Content | Button | Label, Icon, Icon Before (off), Icon After (on) | Base | 7 |
| Button Size | Button | Variant, Label, Icon, Icon Before, Icon After | Default (base), Small | 7 |
| Button Color | Button | Variant, Label, Icon, Icon Before, Icon After | Primary (base), Secondary | 7 |
| C \| Button | Button | Label, Link (Main); Icon, Icon Before, Icon After (Icon) | Base | 4 |
| C \| Icon Button | Button | Variant, Icon, Accessible Name, Hook | Primary (base), Secondary | 1 |
| Section Head | Sections | Chip, Show Chip, Heading, Text | Base | 1 |

**Variable collections (9):** Base collection (empty, Webflow's default) ·
Colors (18: Brand Ink/Paper/Accent, Ink Tint 4–64, Paper Tint 12–64,
Status ×3 + On Dark ×3) · Colors Semantic (15; modes Base, Dark) ·
Typography Scale (28: Family/Base = Geist, Size/12–96, Weight Thin–Black,
Line Height Tight/Snug/Normal, Letter Spacing Tight/Normal/Wide) ·
Typography Role (60: Family/Size/Weight/Line Height/Letter Spacing × 12
roles; modes Base, Tablet, Mobile Landscape, Mobile) · Typography Styles
(5; modes Base + Display, H1–H6, Body Large, Body Small, Label, Button) ·
Layout (7; modes Base, Tablet, Mobile Landscape, Mobile) · Spacing (13:
None, Hairline, 2–128) · Radius (5: None, 2, 4, 8, Pill). Names, modes and
counts match `starter-site.md` › A exactly.

**Classes (~165):** every starter base class from `starter-site.md` › C is
there (Section + Is Hero/Top/Media/Dark, S Bg/S Bg Media/S Bg Overlay,
S Wrapper + Is Wide, G | Page/Nav/Main/Footer W and G | Nav/Main/Footer/
Components, D0–D6, Body L, Body S, Label Text, Chip, Skip Link, Weight ×3,
Leading ×3, Tracking ×2, Text Secondary/Accent/Uppercase/Center/Balance,
Max Width Text, Button, Form Field, Form Input + Is Textarea, To Slot), the
reserved Label and Form, the component classes (Icon, Icon Color, Icon
Glyph, Button Icon, Button Content/Size/Color, Icon Button Box, Video,
Section Head) and the style guide's Sg classes with their swatch, bar and
radius combos. Tag styles set: body, div, p, a, h1–h6, input. Webflow's
own Utility Page Wrap/Content. No project-specific class exists yet.

**Font:** Geist variable (100–900, swap) uploaded. No Geist Mono, Host
Grotesk or JetBrains Mono.

**Missing or different from the starter**

- **Variant props not exposed** (the starter's [Person] step in
  `starter-site.md` › D2 was never done): C | Button has no Color or Size
  prop, Button Color has no Size prop, and Icon has no Size prop (its
  description says Size is linked, but the props list only Variant and
  Icon). Until this is done, every button is Primary/Default and every
  Icon is 16px unless the inner instance is edited by hand. Needs a
  `MANUAL-TODO.md` row (the API can't link a nested variant).
- **Values are still the starter's neutral placeholders**, not the design:
  Brand/Ink `#111111` (design `#0E0F0F`), Brand/Accent `#2f5bff` (design
  orange `#F2592B`), Brand/Paper `#ffffff` (design page `#F4F3F0`); no
  surface `#E7E5DF`, grey `#8B9096` or the four small accents.
- **Scales lack sizes the design uses** (to propose as old → new with the
  style guide): Typography Scale has no 11, 13 or 15; Spacing has no 20,
  40, 80 or 120; Radius has no 12, 16, 20 or 40.
- **Families:** only Family/Base (Geist). Geist Mono (eyebrows, labels,
  numbers) needs a font upload and a family variable; Host Grotesk and
  JetBrains Mono wait on the open decisions below.
- One oddity: a combo **Body L › Max Width Text** exists next to the
  global Max Width Text class. Harmless; check it is intended before the
  style guide cleanup.
- Nothing from the starter is missing.

### Components still to make

From the design, to be proposed after the style guide (none exist yet).
The page shell components already exist; Nav and Footer are built inside
them, not as new components.

| Component | Used on | Props | Behaviour inside |
| --- | --- | --- | --- |
| Nav (content of G \| Nav W) | every page | — | <modules> |
| Footer (content of G \| Footer W) | every page | — (Offices, Credentials from CMS) | — |
| <Name> | | | |

## Modules

| Module | `data-*` hooks | What it does |
| --- | --- | --- |
| <name> | `data-…` | |

## Decisions

- 2026-11-19 · "For Dev" is the source of truth; the frames as placed are the final picks.
- 2026-11-19 · Responsive layouts, interaction states, colours/type/spacing system and SEO are done by us in the prototype.
- 2026-11-19 · Spacing on a 4 px base (4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 120…); type sizes taken from what the Figma file uses, rounded to a scale.
- 2026-11-19 · CMS: conditional visibility on field content instead of switch fields; empty filtered lists hidden in the browser.
- 2026-11-19 · Copy and images from Figma for development; final assets supplied by the person when needed.
- 2026-11-19 · Design width range as in the template (1440–1680).
- 2026-11-19 · Style guide: by the person's request its content is divided
  across several pages in the folder `design › style-guide` (the URL
  pattern of https://wf-tvs.webflow.io/design/style-guide/introduction),
  instead of one page. TVS's pages, layout and scripts are not ported; the
  split per page is agreed when the style guide starts. In Webflow: the
  folder is made by the person (the API can't create folders); the
  starter's single page `/design/style-guide` is removed only after its
  Forms row (the reserved Label and Form classes) is on one of the new
  pages; each page gets `noindex` and is excluded from the sitemap.

## Open decisions

| Item | Now | Options |
| --- | --- | --- |
| Insights / blog | Undecided (note in Figma) | Build list + template · leave out |
| Primary CTA wording | "Talk to Allyant" as the button | Let's Build Differently · Start a Conversation · Discuss Your Project |
| Host Grotesk (buttons, card titles) | Used 286× in Figma | Keep as a second family · replace with Geist |
| JetBrains Mono (project numbers/meta) | Used 54× | Replace with Geist Mono (already loaded) · keep |
| Careers, Our Approach pages | Brand Vision proposals | Keep · drop |
| Credentials, safety figures | Placeholders | Client to supply |

## Stand-in assets and placeholders

- Logo: `../Assets/brand/allyant-logo-*.svg` is a vector trace from Figma ("image 13 [Vectorized]"), cleaned. Ask for the original vector logo before launch.
- Images and videos: Figma exports, stand-ins until final files are supplied.
- Every `[TBC]` in the copy.
