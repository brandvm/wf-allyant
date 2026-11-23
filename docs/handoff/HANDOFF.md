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
3. ✅ Style guide in the prototype, split across pages, frame in Allyant's
   look (Decisions, 2026-11-19).
4. ✅ Variable proposals in `../prototype/src/css/proposed.css`, approved
   2026-11-19: A colour (P1–P5), B type (P6–P9), C spacing, radius and
   layout (P11–P13). P10 decided: buttons and card titles stay in Geist (no Host Grotesk).
   The four small Figma accents are annotation colours only (the
   "Detail · …" panels), not site colours: not proposed. Ink tints decided: Figma's 70/64/50/45/40 % round
   to Ink 64 and Ink 48. (Radius 43/44 is Pill: every one is on a 24–65 px tall tag, pill or avatar.)
5. ✅ Batch D, buttons and form (P14–P33), approved 2026-11-19; P34
   (hugging) added at the person's request the same day: button sheen, Secondary in ink (hover inverts), new Dark and
   Large variants, label = icon size, Button Is Disabled, button label
   white on hover (Button/Text Hover), mono uppercase labels, 54 px
   fields, custom select/checkbox/radio (Webflow custom inputs), help,
   error and Optional text, form messages, focus ring for the keyboard
   only (module focus-modality, repo), textarea not resizable, custom
   select list (module custom-select, repo; P31), status colours with
   tints and coloured, line-centred icons in form messages (P32). The
   style guide shows every state (Default, Hover, Keyboard focus,
   Pressed, Disabled; light and Dark) and its specimens are usable (not
   inert). Three prototype modules move to the repo with the build:
   focus-modality, custom-select (webflow-forms is a prototype stand-in).
   Hugging (P34): Form and Form Field hold children at their own size
   (align-items flex-start), only fields fill the width, Form Check is
   inline-flex — so a button's or checkbox's hover and click area is what
   you see. Audited on all eight style guide pages: no clickable element
   is wider or taller than what it shows.
   Motion (P33): the select list grows out of its field (180 ms in,
   120 ms out), its chevron turns over, buttons shrink to 97 % when
   pressed and the arrow nudges 3 px on hover; reduced motion drops the
   movement. No motion on option navigation or focus rings.
Icons (the person, 2026-11-19): Phosphor **Regular** everywhere, **Bold**
only in buttons with a text label (C | Button), so the arrow holds its own
next to the Medium label; icon-only buttons, field chevrons and message
icons stay Regular. In the build: `pnpm phosphor regular bold` in the
repo; C | Button's Icon prop default becomes `ph-bold ph-arrow-right`;
Icon, Icon Size and C | Icon Button keep `ph ph-…`.
6. Style guide completed 2026-11-19 (the person: "add all five"):
   - a Logo page (orange, ink, on Dark, the mark at 12/24/48, use rules;
     files in Assets/brand, the mark exported from Figma as
     allyant-mark-*.svg);
   - the pages show the approved system, not Webflow's current values:
     `prototype/scripts/approved-snapshot.mjs` applies proposed.css to a
     copy of the snapshot (.sg-approved/) and the generator draws from it;
     one list, no markers (the person: nothing is in Webflow yet; what
     the build must make is in Webflow build list below). The class
     check, check:snapshot and check:drift still use the real snapshot;
   - P35 Chip in the design's style (orange mono caps, the Allyant mark,
     faint orange border, hugs its text in any container; NEW Accent
     Tint/Accent 12, NEW class Chip Mark), approved;
   - P36 NEW class Text Link (orange, underline on hover, keyboard ring),
     All Links tag unchanged, approved;
   - Figma icons mapped to Phosphor on the Components page: every one has
     a close match (Regular by default, Bold in text buttons, Fill for the
     solid ones: stars, play, share, home). LinkedIn is the SVG logo,
     not Phosphor (Decisions). The repo needs
     `pnpm phosphor regular bold fill`.
   Pages now: introduction, logo, colour, typography, layout, buttons,
   forms, components, class-index.
Next: the person reviews the whole style guide at 1440, 820, 667 and 390
and approves it (gate: Prototype style guide approved), then the first
content page (Home).
7. 2026-11-19 (the person: nav and footer first, then the Home hero with
   its scroll animation; Lenis in this project) — in the prototype,
   **awaiting review**:
   - Nav (`partials/nav.html`, module `nav`): logo and light glass pill at
     the top; once scrolled the pill moves to the centre (FLIP, 450 ms)
     and "Talk to Allyant" fades in; over the hero photo the pill turns
     to dark glass (Is On Dark). Tablet and phone: Menu button and a
     panel. G | Nav W fixed (P38).
   - Footer (`partials/footer.html`): light panel with links, offices,
     credentials; orange CTA card. G | Footer W padding (P39).
   - Fluted texture: as Figma draws it (the person, 2026-11-20: the
     animated shader tried first was removed). Each panel's background is
     exported from Figma with its fill and texture layer already blended
     (light: #E7E5DF + texture OVERLAY 70 %; primary: radial
     #FFB9A4→#F2592B + texture SCREEN 20 %; the texture image greyscale,
     exposure −6 %, highlights +16 %), in
     `prototype/public/images/textures/footer-panel-*.webp`, cover, over
     the fill colour. Other sections with the texture get their own
     export the same way.
   - Home hero (`index.html`, module `home-hero`, GSAP ScrollTrigger and
     Lenis): pinned for 1.2 screens, the photo opens from its rounded
     frame to full screen and widens, the shade lifts, heading and card
     blur away upward, the round arrow appears. Glass variants on Button
     Color and Icon Button Box (P37), Chip Is Glass.
   - Lenis + GSAP from the skill's recipe (`pnpm add gsap lenis` in the
     prototype; modules `gsap.ts`, `smooth-scroll.ts`, rules in repo.css).
   Checked at 1440, 820, 667 and 390: no sideways overflow; style guide
   check passes. P37–P39 need the person's approval.
   2026-11-20 (the person): nav links 15 px (Figma 13); the nav hides on
   scrolling down and returns on any scroll up; hero load animation
   (photo settles, heading parts rise and sharpen in turn, card fades
   in); the heading parts fade out on scroll in a longer stagger, the
   card fades in place; the end arrow is a play button that opens the
   film full screen with sound in a modal (skill's modal recipe) with
   Allyant's own player controls (glass bar, orange seek bar, mono time,
   sound, full screen, keyboard; hides when idle); a Koto-style round
   pause/play toggle with a progress ring for inline autoplay videos only
   (skill's video recipe). Glass and blurred elements keep their backdrop
   blur while animating: fades and blurs go on the element itself, never
   on a parent (GOTCHAS). The film is a silent stand-in made from the
   hero photo until Allyant supplies the video.

8. 2026-11-20 — Prototype environment at `/design/canvas` (the person: an
   infinite canvas with the site at three breakpoints and an Inspect
   panel; to go into wf-template once it holds up). PROTOTYPE-ONLY, never
   in Webflow:
   - Desktop 1440, Tablet 820, Mobile 390 side by side, each the real page
     at that width in a frame; pan (scroll, space-drag), zoom (⌘/pinch,
     Zoom control, fit). Live (screen-tall, scrolls, motion, sync scroll)
     or Full page (the whole page, motion off via `?static`, viewport
     units frozen to the device height).
   - Inspect (⌥T): pick in any frame; the panel edits at a breakpoint, and
     edits cascade like Webflow (a smaller breakpoint with its own value
     keeps it). "Copy for Claude" gives the changes grouped by breakpoint.
   - UI from kit (`@weftly/components`, `@weftly/editor`, read as source
     from `../../../Projects/weftlyhq/kit` or `WEFTLY_KIT`; React 19 in the
     prototype). Code: `prototype/src/canvas/`, `design/canvas/index.html`,
     the `?static` script in `partials/head.html`.
   - Presence (the person, 2026-11-20): each Claude session and agent
     working here shows in the canvas, Figma-style: avatars in the
     toolbar, a list with each one's status and plan step, and a cursor
     with a label on the element a recent change touched, in every frame.
     A Claude Code hook (`prototype/scripts/presence-hook.mjs`, registered
     in the workspace's `.claude/settings.json`) posts each tool call to
     the dev server's `/__presence`, which pushes it to the canvas live.
     Agents don't move a mouse: the cursor marks where the work is.
   - All pages (Full page view, the person 2026-11-20): every site page
     side by side, each at its three breakpoints. Drawn like Figma for
     speed: pictures for the overview (`pnpm canvas:shots`, or Update
     pictures in the toolbar; half scale, JPEG, public/canvas-shots/, not
     committed); a frame runs live only on screen at 30 % zoom or closer
     (at most four, once the canvas stops moving); nothing off screen is
     drawn. A page's name opens it. View only (Inspect is off there).
   - Before wf-template: kit is private and proprietary, wf-template and
     client repos are public; how a public template gets kit (a private
     submodule, a published package, or the environment living in Weftly)
     is the person's call.

9. 2026-11-20 — Home › "Why Project Delivery Needs to Change" (Figma
   632:241), in the prototype, within 2–5 px of Figma at 1440: heading row,
   the stages panel (fluted texture exported from Figma, OVERLAY 80 %),
   the bars, the closing line and button. The four stages are tabs (module
   `stage-tabs`): autoplay starts only on screen, the bars track it; a
   click or key stops it until the section leaves the screen; hover does
   not pause (the person). The stages share one grid cell so the tabs
   never move. Only Design has copy and an image in Figma: Tender, Bid and
   Build show `[TBC]` and Design's image (Open decisions). Tabs numbered
   01–04 (Figma 01, 02, 02, 02).
   Tablet and phone (2026-11-20, the person: "don't take so long"):
   section 1976 → 1399 at 820 and 1909 → 1358 at 390, desktop unchanged.
   Tablet keeps image and text side by side (image 40 %); 767 and below
   stack with the image above (5:2, 16:9 at 479); at 479 the results are a
   plain list of dots and text, intro 15, closing line 20. When the tab row
   scrolls sideways (phone), the current tab slides into view as the stages
   advance, scrolling only the row.

10. 2026-11-20 — Four more Home sections in the prototype, built by
   subagents in parallel (the person watched their cursors on the canvas),
   checked at 1440, 820 and 390 (no sideways overflow; style guide check
   passes):
   - The Traditional Model photo card (Figma 632:317; section
     `#traditional-model`, module `model-scroll`): full width like the hero
     (Section Padding H in, no max width). Pinned: the photo opens from the
     card's rounded box to the whole screen and settles from a touch
     closer, then the nine problems move up one at a time (100/60/40/20 %),
     the photo crossfading lab → glassware → site at problems 4 and 7, each
     zooming out 1.12 → 1; parallax as it leaves. Dark glass cards with the
     pill number for all nine (the person). Reduced motion: not pinned, all
     nine shown.
   - What Makes Allyant Different (632:373; `diff-` classes): staircase
     of six photo cards (parallax, stagger-scroll); two columns at 991; a
     snapping sideways row at 767 and below.
   - Services Overview (632:441; `services-` classes, module `services`):
     heading photo opening with the scroll, ten cards in a drag/arrow strip
     (arrows and progress line added: not in Figma); 07–10 [TBC].
   - Featured Projects (632:477; `featured-` classes, module
     `featured-slider`): centred slider, drag/click/keys, mouse cursor
     bubble; all five cards EmpowerPharm as in Figma, sector [TBC].
   Open from these: "Greater clarity." as the first of nine problems;
   Featured chip reads "What we do" (Figma copy slip?); Services arrows
   not in Figma.
   Later the same day (the person): the photo card opens full width like
   the hero and fills the screen on any window; Services title left
   aligned, its heading photo cropped to the engineer's head, the strip's
   edges masked (right until the end, left only once it has moved; mask is
   repo-css) instead of a stone gradient, and it ends as far from the right
   as it starts from the left; Featured cursor bubble exactly as Figma (120 px,
   11 px label, 14 px carets; it had rendered at 60 % because GSAP folded
   its CSS scale into its transform), no native link drag (it froze or
   vanished on a press); a click on a side card moves one card (focus and
   click had each moved it); stage tabs: the
   bars under the panel are gone, each tab fills from the left as its own
   tracker (::before, repo-css; Why Progress, Bar, Bar Fill dropped from
   the build list); Shift+G hides and shows the page lines (module
   page-grid-toggle; whether it ships to the site is open).
   Featured Projects now loops and its photos move in parallax as the
   cards slide, on smooothy (npm `smooothy` 0.0.35, MIT, by Federico
   Valla; the person approved adding it): Featured Img is 120 % of the
   card for the travel, the 16 px gap is the item's padding (smooothy
   spaces items by their width). Build list: `pnpm add smooothy` in the
   repo with the featured-slider module.

11. 2026-11-20 — The rest of Home, one subagent per section, merged into
   index.html and new-classes.css (style guide check passes, 341 classes):
   Proof of Partnership (`partner-`, module `logo-marquee`; all copy and
   client logos are Figma placeholders), Partners on Trust video (`trust-`,
   stand-in video, band 40 px in where Figma has 24), Industries
   (`industries-`), The Outcome (`outcome-`; Figma has no stats here), Why
   Allyant? (`whyal-`). Figma's last frame (632:1349) is the footer, already
   built. Motion: the nine Outcome icons loop (CSS inside each SVG: dashes
   flow, dots travel or pulse, rings turn; reduced motion stops them); the
   Why Allyant cards' line art has a light running along its lines (module
   `line-shine`, lines-only masks `whyal-card-*-lines.webp`; in Webflow the
   mask is an asset URL in the data-line-shine attribute). A separate
   session made motion consistent site-wide (`prototype/MOTION.md`,
   `src/modules/motion.ts`); the new sections still need its pass.
   Then (the person): heading photos are one shared pill, Title Pill /
   Title Pill Img (1.9375 × 1.0156em of the heading, Figma's 124 × 65 at
   64 px; replaces Services Title Media/Img and Whyal Title Pill/Img),
   opening sideways from 0 as the heading comes on screen, as on
   brandvm.com (module `title-pill`; its width follows the scroll, open
   from the heading at 90 % of the screen to 55 %, closing again on the way
   back; the person). Five Title Pill Edge layers (+ Is 2–Is 5) over
   the photo give it brandvm.com's glassy bevel: inset box shadows with
   darken / overlay / lighten blending, all Designer styles. The client logo row: logos at 40 % on the
   images (the row's own opacity is the reveal's), and the loop follows the
   scroll: faster with scroll speed, left on the way down, right on the way
   up, easing through each change (module `logo-marquee`, GSAP ticker, no
   CSS animation). Featured Projects got the Services strip's arrows and progress
   line (the person): the foot's classes are now shared, Slider Foot,
   Slider Progress (+ Fill) and Slider Nav (were Services Foot, Progress,
   Progress Fill, Nav), plus Featured Foot for its spacing. On the looping
   slider the arrows never disable and the progress is a window one
   project wide that travels round the line, wrapping end to start.
   Tablet and phone menu fills the screen when open (the person): Nav
   Menu fixed over the page colour, links 40 px (32 at 767) stacked with
   rules, Talk to Allyant at the foot, logo and toggle above it; the toggle
   reads Close with an ×; the page doesn't scroll while open (Lenis
   stopped), Tab stays inside, Escape or a link closes it. The links rise
   in one after another (repo-css stagger; Designer transitions on Nav Menu
   use plain values, not the repo's motion tokens).

12. 2026-11-20 — What We Do and the Service template in the prototype,
   one subagent per page, merged (style guide check passes, 441 classes;
   no sideways overflow at 1440, 820, 667, 390; no console errors):
   - What We Do (`what-we-do/index.html`, Figma 632:4133, `wwd-`): hero,
     the service journey (Home's Services cards in a 3-column grid, all
     Services by Order, "View service" in each card), "Three Routes to the
     Same Outcome" (Figma shows two rows numbered 01 01 and cut off; built
     three, 01–03, the third [TBC] with a stand-in photo), Related
     Projects (Home's Featured slider).
   - Service template (`what-we-do/construction-management/index.html`,
     Figma 632:4599, `svc-`, modules `svc-accordion`, `svc-deliverables`):
     breadcrumb, hero with parallax photo; What This Covers (Services strip
     with arrows); When Owners Choose It (three rows opening one at a
     time, route panel to the current service and Compare with); How
     Allyant Runs It (static; Figma has four points, the handoff said
     five: built four); What You Get (cards widen on hover/focus; 2×2 at
     991, a sideways row on phone); Related Projects (Featured slider,
     Delivery method = current, hidden when empty).
   - New module `hide-if-empty` (the CMS plan's data-hide-if-empty).
   CMS plan v1.1 (the person, 2026-11-20): Services gets Cover 1–6
   title/text/image (one illustration per cover; replaces Cover 1–5 and the
   single Covers image) and Deliverables intro + Deliverable 1–4
   title/text/image (replaces the Deliverables rich text and image), so the
   template's six covers and four deliverable cards come from fields; each
   cover and deliverable card hides when its title is empty. "How Allyant
   runs it" is four points. Figma updated to match: CMS Planning › 02 ·
   Services rows, the page map notes, rule 04, the label (v1.1), and the
   Dev Mode notes on the Service frame's What This Covers and What You Get. The
   nav has no current state for What We Do yet.

13. 2026-11-20 — Projects and the Project template, one subagent per page,
   merged (style guide check passes, 534 classes; no sideways overflow at
   1440, 820, 667, 390; no console errors):
   - Projects (`projects/index.html`, Figma 632:5224, `prj-`): hero (reuses
     Wwd Hero classes), four filter pills with checkbox menus, count and
     Reset, 3-column cards, View more, empty state; Repeat Clients panel.
     Markup is Finsweet Attributes List v2 (`fs-list-*`);
     `prj-filters.ts` is a prototype stand-in, not ported (in the build:
     Webflow Dropdowns + `pnpm finsweet add list`). Filter data on all but
     EmpowerPharm is invented so the filters can be tried; Size and Region
     options are ours (Under 10k / 10–50k / 50–100k / Over 100k sf;
     Ontario / Rest of Canada / United States); three [TBC] projects make
     Figma's count of 9.
   - Project template (`projects/empowerpharm-…/index.html`, Figma
     632:5665, `prd-`): breadcrumb, hero, At a Glance, About (sticky chip,
     wave text, four photo + text pairs), The Story (title pill, accordion),
     The Outcome, Systems & Scope, Gallery mosaic, Client Quote (hidden
     without a testimonial), Related Projects (hidden when empty).
   - Project links agree everywhere (`/projects/bluerock-gmp-manufacturing-
     facility/`, `/projects/gay-lea-research-and-development-laboratory/`…).
   - Breadcrumb separator: Figma's own Remix `arrow-drop-right-line`,
     inline SVG in the text colour (Ink 48) — the person's call, an
     exception to the Phosphor Icon rule (Decisions); the home icon stays
     Phosphor.
   - What We Do hero: its button was cut off while rising on load (Section
     clips both axes and the hero has no bottom padding); Section · Wwd Hero
     now overflows downward (overflow-y visible, z-index 1).
   CMS plan gaps from the Project template (open, for the person): a
   Statement field (About intro), About 1–4 text + image (the plan has one
   About and two images), Scope as label/value pairs (separate fields or a
   "Label: value" line format), and a testimonial photo. Shared class names
   suggested: the Svc Breadcrumb/Hero/When and Wwd Hero classes now serve
   several pages (Breadcrumb, Page Hero, Accordion would read better), and
   one Title Line class for the three heading-with-photo lines.

14. 2026-11-20 — Page transitions site-wide (the person: Osmo Supply's
   "Stacked Cards"), on the build skill's Barba recipe, in the prototype:
   - `@barba/core` 2.10.3 (approved); recipe files `transition.ts`,
     `pages.ts`, `barba.d.ts`; GSAP CustomEase registered with Osmo's
     `osmo` ease (0.625, 0.05, 0, 1).
   - Markup on every page: `data-barba="wrapper"` on G | Page W,
     `data-barba="container"` + `data-barba-namespace` on G | Main W (the
     footer inside it); the nav stays outside. New classes Transition and
     Transition Middle (the orange card, Brand Accent) outside the
     container.
   - The animation: the leaving page, the orange card and the arriving
     page shrink and drop one after another with rounded corners, the new
     page grows back to full size (sync). Reduced motion: instant swap.
     Made more noticeable (the person: the pages share one sand colour):
     a darker sand stage behind the cards while it plays (#CDCBC6, Stone
     under Ink 12 %, the person's pick over ink; data-transitioning,
     repo-css), the Allyant mark and the arriving page's name on the orange
     card (Transition Label, Transition Mark, D1 › Transition Title), and a
     deeper shrink (0.9 / 0.82 / 0.74; Osmo's 0.95 / 0.875 / 0.8).
     Hero Img rests at scale 1.15 on its class, and the transition sets the
     arriving Home hero's frame up before showing it (no snap; GOTCHAS).
     Repo CSS no longer reads Webflow variables (24 references in 11 blocks
     replaced by their values; GOTCHAS).
   - `main.ts` split: global modules once (smooth-scroll, focus-modality,
     page-grid-toggle, nav — closes its menu before a page leaves and
     re-reads its state after) and page modules again on every page,
     torn down when it leaves. `pages.ts` (Allyant addition) also records
     observers, ticker callbacks, ScrollTrigger events, media-query
     listeners and intervals a module starts, so our modules need no
     clean-up code of their own.
   - After a transition, entrances skip what is already on screen (the
     cards just showed it; `pageArrival` in motion.ts): no flash.
   - Checked: seven swaps between all five pages without a reload; each
     page always comes back with the same ScrollTriggers, ticker callbacks
     and height; Home's two pins rebuilt; no console errors. The
     Webflow-only parts of the recipe (forms, Turnstile, IX2, head tags)
     can't run in the prototype (no webflow.js): check them on staging
     with `pnpm wf:transitions` and a real form submit (recipe › step 7).
   Also (the person): Intro Grid is a third and two thirds with no gap, so
   the statement and photo start on the page grid's second line (Home
   intro and the Project template's About).

15. 2026-11-20 — The remaining pages, one subagent per page, merged (style
   guide check passes, 882 classes; no sideways overflow at 1440, 820, 667,
   390; no console errors): Our Approach, Company, Contact, Insights, the
   Insights article (Insights built on the person's go-ahead, though still
   undecided above) and a 404 (designed here, not in Figma; a utility page:
   MANUAL-TODO when built).
   - Our Approach › We Think Like Owners (module `oa-owners`): pinned from
     768 px, half a screen per point; the current point shows, its rule
     fills, a photo per point crossfades and zooms out 1.12 → 1 (01 and 05
     from Figma, 02–04 Home's Different photos). Phones and reduced motion:
     every point in the flow.
   - Our Approach › A Route Through Every Project: stage-tabs in scroll
     mode (`data-stage-scroll`, `data-stage-pin`, `data-stage-zoom`): pinned
     from 768 px, the scroll steps through the ten services, the tab line
     fills through each stretch, the photo zooms out; a click scrolls to
     that service. Phones: autoplay tabs as before. Then (the person): the
     section is a screen tall (stages at its foot); a photo per service
     crossfades and zooms out (01 Figma's desk, the rest the Services
     photos: CMS Hero image), and the tab row glides with the scroll.
   - Our Approach hero, the Allyant Method band: opens from its rounded box
     to the whole screen as it is scrolled, like Home's photo card (module
     `band-open`; markup: the band's contents in Oa Band Stage); the label
     and the stage row ride with the edges; photo 1.15 → 1. Once open, the
     six stages become current one after another with the scroll and the
     track fills orange up to the current one (new class Oa Method Fill);
     a dot is current only once the line reaches it; each stage has its
     own photo, crossfading in as the line nears its dot and zooming out
     1.12 → 1 through its stage (new combo Oa Band Img Is Later; stand-ins
     from the site's photos, 03 Figma's site photo: Allyant to supply six);
     the band keeps room below it so the next section never covers the
     open photo.
   - How Allyant Runs It icons (Cost, Schedule, Risk, Quality) loop like
     Home's Outcome icons (CSS inside each SVG; reduced motion stops them).
   - Dotted landscape under How Allyant Runs It (Service template, Our
     Approach): module `dot-field`, a WebGL shader (no library) in place of
     Figma's video: square dots on whole device pixels, a drifting ridge,
     dithered greys, a few blinking orange squares; the pointer raises the
     land and lights the dot under it orange. The image stays as the
     fallback (no WebGL) and is hidden once it runs (repo-css js-state).
     Reduced motion: one still frame. A click sends an orange ripple
     across the field. Lower-end devices keep the image (the person):
     Save-Data, 4 GB or less, 4 cores or fewer, or a software renderer.
     2026-11-23 (the person: "the exact diagram with dots"): the floor plan
     under Built Around the Client (Our Approach) is drawn by the same
     module in image mode (`data-dot-field-image`, pitch 4.2 px at 1440 =
     the plan picture's own dot pitch): each cell takes the picture's
     darkness under it, a slow band of light crosses the lines, rare
     orange dots blink on them; pointer and click ripple as on the
     landscape. The random hash no longer uses sin() (it lit stray orange
     cells at small dot sizes on some GPUs).
   - 2026-11-23 (the person: the snap after landing, checked on every
     page): arriving by transition, the hero photos of Service, Project,
     Company and the article, Contact's supplier photo and Our Approach's
     Method band jumped once the cards settled. Parallax and band-open now
     draw their resting state on the arriving page before it shows (as
     Home's hero); all eleven pages measured clean (GOTCHAS).
   - Company › Why collaborate with Allyant (2026-11-23, the person: "the
     same as We Think Like Owners"): it had been built static from Figma's
     one point; Figma has stages 02 and 03 beside the page (632:8230,
     632:8249), each with its own photo. Now module `co-why`: from 992 px
     pinned, the three points one at a time (rising in as on Our
     Approach), the orange mark sliding down one grey rail (new classes Co
     Why Rail, Co Why Mark), a photo per point crossfading and zooming out
     (Figma's three; `why-plans.webp`, `why-team.webp`; new combo Co Why Img
     Is Later). Below 992 and reduced motion: the three points in a list.
     The photo no longer has parallax (the pin's zoom replaces it).
   - Company › Careers card (2026-11-23, the person: "with dots as well"
     and the portraits floating very subtly): Figma's artwork is a video
     of dotted roads at 60 % under a stone fade, with seven portrait
     circles on top; it had been exported flat (careers-team.webp, now
     unused). Now in its layers: the video's still drawn live in dots by
     dot-field (image mode; its orange markers kept orange; new settings
     data-dot-field-ground and data-dot-field-host, the dots keep the
     picture's opacity), the fade, and the seven portraits (exported from
     Figma with their 4 px sand ring) floating 4 px each at its own pace
     (repo-css @keyframes; reduced motion: still). New classes Co Careers
     Pattern, Fade, People, Avatar (+ Is 1–Is 7). Then (the person: "yes,
     animate them"): the six orange markers travel along the roads, as in
     Figma's video (data-dot-field-movers): the module finds each marker
     in the picture and traces the dark road it sits on (keeping to the
     middle of the band, coasting over small gaps), then moves it along
     that road at a steady pace, back and forth or round a loop; at
     junctions a marker may carry on onto the next road. The picture's own
     markers are drawn as road. The grey dots keep the picture's 60 %;
     the markers are at full strength. Reduced motion: markers where the
     picture has them.
   Open from these pages: Offices needs a Photo field (Contact, Company);
   Company's credential figures vs the Credentials collection; Insights
   categories are placeholders; the LinkedIn URL in Figma reads
   `allyantdesign0andconstruction` (typo?); Contact's Timing options are
   ours, and whether to show a map. Shared class names to consider: Page
   Hero, Breadcrumb, Accordion, one card class.

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
  surface `#E7E5DF`, grey `#8B9096`. (`#0F766E`, `#0D9755`, `#0D5797`,
  `#5E9A95` appear only in the annotation panels.)
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

Plan v1.1 (Services fields updated 2026-11-20, see Status 12) — **awaiting approval.** Drawn in Figma › page
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
| **Services** (10) · template `/what-we-do/<slug>` | Name · Slug · Order (number, journey 1–10) · Summary (plain, cards) · Thumbnail (image) · Hero image (image) · Hero intro (plain) · Covers intro (plain) · Cover 1–6 title (plain, opt) · Cover 1–6 text (plain, opt) · Cover 1–6 image (image, opt; each cover's own illustration) · Situation (plain, opt) · When (plain, opt) · Compare with (multi-ref Services, opt) · Deliverables intro (plain, opt) · Deliverable 1–4 title (plain, opt) · Deliverable 1–4 text (plain, opt) · Deliverable 1–4 image (image, opt) · SEO title (opt) · Meta description (opt) | Home › Services overview (all, by Order) · What We Do › service journey (all, by Order) · Service template · Project template (delivery method) · Contact form "Project type" options |
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
(the same four points on every service, as Figma draws it), stats (100+, 99%, 5/5), Why
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

## Webflow build list

Everything approved in the prototype that Webflow doesn't have yet, in
build order. Nothing here is in Webflow; each batch is written after a
go-ahead, then read back (and `pnpm check:drift` after publishing). The
prototype's style guide already draws all of it. Sources:
`prototype/src/css/proposed.css` (P1–P36), `new-classes.css`, the
modules in `prototype/src/modules/`. Tick a line when it's in Webflow and
read back.

**1. Site settings and uploads** [Person unless noted]
- [ ] Fonts: upload Geist Mono (`Assets/fonts/`, variable woff2, display
      swap). Geist is already there.
- [ ] Head code: paste piece 1 of `loader.html` (Site settings › Custom
      code); publish.
- [ ] Assets [Claude, upload]: `why-panel-light.webp`, `why-stage-design.webp`, `intro-meeting-room.webp`, `allyant-mark-orange.svg`,
      `allyant-mark-white.svg`, Home hero `hero-lab.webp`,
      `footer-panel-light.webp`, `footer-panel-primary.webp`,
      `avatar-lab.webp`, `avatar-atrium.webp` (prototype/public/images),
      `allyant-logo-*.svg` (Assets/brand), `linkedin.svg`
      (Assets/brand/social), `select-chevron.svg`, `check.svg`,
      `check-ink.svg` (prototype/public/images/ui).

**2. Variables** [Claude, MCP]
- [ ] Colors: change Brand/Ink, Brand/Accent, Ink Tint 4–64, Status/Success,
      Status/Error (P1, P2, P32); new Neutral/Sand, Neutral/Stone,
      Neutral/Grey (P3), Status/Success Tint, Error Tint, Warning Tint
      (P32), Accent Tint/Accent 12 (P35).
- [ ] Colors Semantic: Background/Page → Sand, Background/Surface → Stone
      (P4), Button/Background → Accent, Background Hover → Ink (P5); new
      Button/Text Hover, Base and Dark = Paper (P29).
- [ ] Typography Scale: new Size/11, 13, 15 (P6), Family/Mono (P7),
      Letter Spacing/Close, Line Height/Solid; change Letter Spacing/Tight,
      Wide, Line Height/Normal, Snug (P8).
- [ ] Typography Role: every role's size per breakpoint, weight, line
      height, tracking; Label → Family/Mono (P9; 26 variables).
- [ ] Spacing: new 20, 40, 56, 80, 120 (P11). Radius: new 12, 16, 20, 40
      (P12). Layout: Section Padding V/H, Nav Padding H, Content Gap per
      breakpoint (P13); new Input/Height 3.375em (P19).
- [ ] Order: Webflow adds new variables at the end; drag them into place in
      the Designer if wanted [Person].

**3. Classes** [Claude, MCP; Person where marked]
- [ ] Changed: Button Color (sheen gradient and inner shadow [Person:
      gradient and shadow with variables], pressed scale, transitions,
      hover label colour; P14, P29, P33), Button Color · Secondary variant
      (ink border, hover inverts; P15), Button Size (heights 3.2em /
      2.6667em, Small padding and type; P17), Button Icon 1em (P21), Label
      (mono caps; P18), Form Input (54 px, white, 12 px corners, Body type,
      placeholder, focus border, no ring; textarea not resizable; P19,
      P30), Form (flex column, 20 gap, align start; P20, P34), Form Field
      (align; P34), Chip (P35).
- [ ] New: Button Is Disabled (P22); Form Input Is Select [Person:
      custom property appearance none], Is Error, Is Placeholder, Is Open;
      Label Optional, Form Help, Form Error; Form Check, Form Check Input
      (+ Is Radio; Checked and Focused states on Webflow's custom checkbox
      and radio), Form Check Label; Form Message (+ Is Success, Is Error),
      Form Message Icon; Select Value, Select Chevron (+ Is Open), Select
      Menu, Select Option (+ Is Active, Is Selected); Chip Mark; Text Link;
      Social Logo (P23–P28, P31, P35, P36).
- [ ] Body tag style: Typography › Wrap › Pretty (P40, proposed 2026-11-20).
- [ ] Nav, footer, hero (batch E, new-classes.css): Nav Logo (+ Img),
      Nav Menu (+ Is Open), Nav Links (+ Is Scrolled, Is On Dark), Nav
      Link, Nav Link Plus, Nav Cta (+ Is Scrolled), Nav Toggle (+ Is On
      Dark, Label); Footer Grid, Main, Top, Logo (+ Img), Col, Links,
      Link, Text, Credentials, Badges, Badge, Bottom, Meta, Cta, Cta Text;
      Chip Is Glass; Hero Stage, Frame, Img, Shade, Content, Card,
      Avatars, Avatar, Scroll, Play, Heading, Actions; Section Is Intro,
      Intro Grid, Aside, Chip, Main, Statement, Media, Image, Img, Points,
      Point, Point Rule, Point Text; D1 › Footer Cta Title; Page Grid,
      Inner, Col (+ Is Last) — four page lines; Why Head, Head Main, Intro,
      Stages, Panel, Media, Media Img (+ Is Active), Content, Tabs, Tab
      (+ Is Active), Tab Num, Stage Stack, Stage, Stage Text, Results,
      Result, Result Dot, Foot, Foot Text, Foot
      Title, Foot Note, Foot Action. Page Grid placed inside G | Page W:
      Figma's layout grid (3 columns, stretch, margin 40, gutter 0) held to
      Container Max Width like S Wrapper. Content sections sit in S Wrapper
      and align to it at every width; the nav, hero and footer are exempt
      and run full width, 40 px in (the person, 2026-11-20). Chip padding
      corrected (Spacing/8 16, gap 12: the em variables render at 11/16 on
      its 11 px label). Changed: G | Nav W fixed, G | Nav
      flex (P38); G | Footer W padding (P39). [Person: Footer Cta radial
      gradient.]
- [ ] Every new class on the style guide page so Clean up unused styles
      keeps it.

**4. Components** [Claude, MCP; Person where marked]
- [ ] Button Color: new variant Dark (P16). Button Size: new variant
      Large (P17). Button Color and Icon Button Box: new variant Glass (P37).
- [ ] Expose the nested variants as props: Color and Size on C | Button,
      Size on Icon [Person: Link to new prop; the API can't] (inventory,
      Components to make).
- [ ] C | Button: Icon prop default `ph-bold ph-arrow-right`. Icon, Icon
      Size, C | Icon Button keep `ph ph-…`.
- [ ] Section Head: the chip gets the Chip Mark image (P35).

**5. Repo** [Claude, commit after an OK]
- [ ] `pnpm phosphor regular bold fill`.
- [ ] Smooth scroll: `pnpm add gsap lenis`, the recipe's `gsap.ts`,
      `smooth-scroll.ts` and `lenis.css` (recipes/smooth-scroll), wired in
      `src/index.ts` as the recipe says. If Webflow's own GSAP is on (Site
      settings), turn it off.
- [ ] Modules `stage-tabs.ts`, `wave-text.ts`, `parallax.ts`, `stagger-scroll.ts` (GSAP
      SplitText registered in `gsap.ts`).
- [ ] Modules `nav.ts`, `home-hero.ts`, `video-modal.ts`,
      `video-player.ts` from the prototype, and the recipes' `modal.ts`,
      `autoplay-video.ts`, `video-toggle.ts` (with the prototype's
      progress-ring addition); their `repo-css` rules (nav hide and button
      fade, hero load lock, modal, player states, toggle
      ring, Safari video glyphs).
- [ ] Media page: upload the film (and later inline videos) to
      /design/media; replace the stand-in `allyant-standin.mp4`.
- [ ] Modules: `focus-modality.ts`, `custom-select.ts` into
      `wf-allyant/src/modules/`, registered in `src/index.ts`.
      (`webflow-forms.ts` is a prototype stand-in for webflow.js: not
      ported.)
- [ ] `src/styles.css`: the `repo-css` rules at the end of
      `new-classes.css` (keyboard ring on fields, native select padding,
      select list motion, button arrow nudge, reduced motion) and the
      motion tokens `--ease-ui`, `--ease-ui-move` beside `--ease-bbs`.
- [ ] Page transitions: `pnpm add @barba/core`, the prototype's `transition.ts`, `pages.ts`, `barba.d.ts`, CustomEase in `gsap.ts`; `src/index.ts` split into global and page modules as `prototype/src/main.ts`; Barba attributes on G | Page W and G | Main W; Transition classes. Then `pnpm wf:transitions` on staging and a form submitted after a transition.
- [ ] `pnpm build`, commit `dist/`, push; check staging.

**6. Style guide in Webflow** [Claude, MCP; Person for the folder]
- [ ] Folder `design › style-guide` [Person: the API can't make folders];
      pages introduction, logo, colour, typography, layout, buttons,
      forms, components, class-index; noindex, out of the sitemap.
- [ ] Sg classes in Allyant's look (`style-guide-allyant.css`), new Sg Row
      Classes and Sg Row Sep.
- [ ] Move the Forms row (Form, Label) before removing the old single
      `/design/style-guide` page.

**7. Before going live** [Claude checks]
- [ ] `pnpm check:drift` shows no difference between the prototype and
      staging; this list is all ticked.
- [ ] No `data-manual-todo` left on a published page (MANUAL-TODO.md).

## Modules

| Module | `data-*` hooks | What it does |
| --- | --- | --- |
| nav | `data-nav`, `data-nav-scroll`, `data-nav-theme-target`, `data-nav-toggle`, `data-nav-menu`; `data-nav-theme="dark"` on dark areas | Is Scrolled after 40 px (pill to the centre, button in), Is On Dark over dark areas, the tablet/phone menu (Escape and outside click close it) |
| home-hero | `data-hero`, `data-hero-stage`, `-frame`, `-img`, `-shade`, `-content`, `-card`, `-play` | The Home hero scroll story (ScrollTrigger, pinned, scrubbed) |
| smooth-scroll | — | Lenis on the GSAP ticker (recipe); off under reduced motion |
| modal | `data-modal`, `data-modal-panel`, `data-modal-close`, `#name` links (recipe) | Dialogs: focus kept inside, Escape and backdrop close, Lenis stopped |
| video-modal | `data-video-modal` | Plays the film from the start with sound when its modal opens, pauses on close |
| video-player | `data-player`, `data-player-big`, `-play`, `-time`, `-duration`, `-track`, `-buffer`, `-fill`, `-thumb`, `-mute`, `-fullscreen`, `-controls` | Allyant's player controls for a film with sound; hides them when idle |
| wave-text | `data-wave-text` (+ `-start`, `-end`, `-from`, `-color`, `-scrub`) | Statement fills letter by letter on scroll: ink 20 % → orange (holds briefly) → own colour; after Osmo's Gradient Wave Text (SplitText) |
| parallax | `data-parallax` (travel, default 0.06), `data-parallax-target` | Image moves slower than the page inside its frame |
| stagger-scroll | `data-stagger-scroll`, `data-stagger-item`, `data-stagger-rule` | Items come in one after another with the scroll (rule draws, then content rises) |
| stage-tabs | `data-stage-tabs` (`data-stage-duration`), `data-stage-tab`, `data-stage-panel`, `data-stage-media`, `data-stage-bar` | Tabs that autoplay on screen with a bar tracking each stage; an interaction stops it until the section leaves the screen; state on `data-stage-state` / `data-stage-why` |
| transition, pages | `data-barba` wrapper/container/namespace, `data-transition-wrap`, `data-transition-middle`, `data-barba-prevent` | Barba page transitions (stacked cards); global vs page modules, teardown on leave (recipe barba) |
| dot-field | `data-dot-field` (pitch px at 1440) on the image; `data-dot-field-image` draws the image itself in dots | WebGL dotted landscape, or an image redrawn in dots (the floor plan); pointer and click ripple; image as fallback |
| band-open | `data-band-open`, `data-band-stage`, `-img`, `-label`, `-foot` | A photo band opens to the whole screen on scroll (pinned) |
| co-why | `data-why`, `data-why-point`, `data-why-mark`, `data-why-img` | Company › Why collaborate: pinned from 992 px, a point and photo per step |
| oa-owners | `data-owners`, `data-owners-point`, `-rule`, `-img` | Our Approach owners: pinned, a photo per point |
| autoplay-video, video-toggle | `video[autoplay][muted]`, `data-video-toggle`, `data-video-ignore` (recipe) | Inline videos play only on screen; the round pause/play toggle with its progress ring |

## Decisions

- 2026-11-19 · "For Dev" is the source of truth; the frames as placed are the final picks.
- 2026-11-19 · Responsive layouts, interaction states, colours/type/spacing system and SEO are done by us in the prototype.
- 2026-11-19 · Spacing on a 4 px base (4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 120…); type sizes taken from what the Figma file uses, rounded to a scale.
- 2026-11-19 · CMS: conditional visibility on field content instead of switch fields; empty filtered lists hidden in the browser.
- 2026-11-19 · Copy and images from Figma for development; final assets supplied by the person when needed.
- 2026-11-20 · Fluid scale set to the Figma frame (src/styles.css §01 and prototype.css): 1em = 16 px at 1440 (`--size-container-ideal: 1440`) and above (`--size-container-max: 1440px`, the person 2026-11-20), holding at 1280 and below down to 992 (`--size-container-min: 1280px`). It had been left at the template's 1680, so every size rendered at 86 % of Figma at 1440. Nav, hero and footer then measured against Figma at 1440 and matched within 2–5 px; known leftovers: the hero heading's line height (Display 1.0, Figma 0.9) and the Large button (225×62, Figma 204×61, approved P17).
- 2026-11-20 · Breadcrumb separator is Figma's Remix arrow-drop-right-line as inline SVG (currentColor, Ink 48), not a Phosphor icon (the person: "exactly what Figma has").
- 2026-11-19 · Host Grotesk is not used: buttons, card titles and filters are set in Geist (P10).
- 2026-11-19 · Social logos (LinkedIn, the only one in the design) are the networks' own logos as SVG (`Assets/brand/social/`, exported from Figma, currentColor), not Phosphor icons: Phosphor's logo icons aren't good enough. Brand marks, so allowed by AGENTS.md › Reuse what the site already has. Class Social Logo.
- 2026-11-19 · Style guide: by the person's request its content is divided
  across several pages in the folder `design › style-guide` (the URL
  pattern of https://wf-tvs.webflow.io/design/style-guide/introduction),
  instead of one page. TVS's pages, layout and scripts are not ported.
  Split agreed 2026-11-19: introduction (Overview), colour (Colors,
  Themes), typography (Typography, Utilities), layout (Spacing, radius and
  layout), buttons, forms, components, class-index (Other classes), and
  sections (Page sections) while page previews exist. The prototype's
  `scripts/style-guide-pages.mjs` (`pnpm style-guide`, `pnpm
  check:style-guide`) runs the skill's generator and check on the one page
  `design/style-guide/index.html`, then cuts it into
  `design/style-guide/<page>/index.html` with the frame and Sg classes
  unchanged: the sidebar lists the pages (current page `w--current`, as
  Webflow's Current state), the first section's title is the page's h1.
- 2026-11-19 · Style guide frame in Allyant's look, by the person's
  request: `prototype/src/css/style-guide-allyant.css`, loaded on the split
  pages after the generated layout. Existing Sg classes only, fixed Figma
  values (page `#F4F3F0`, ink `#0E0F0F`, orange `#F2592B`, surface
  `#E7E5DF`, light Geist headings, Geist Mono labels and chips, 16–20 px
  corners), not the site's variables, so the frame doesn't move with a
  proposal; specimens still show the variables. In Webflow the same values
  go onto the Sg classes (style guide build), and Geist Mono must be
  uploaded first. Two new Sg classes for it: **Sg Row Classes** (a row of
  class chips that wraps) and **Sg Row Sep** (the › or · between chips),
  so a class chain is one chip per class and no chip's text wraps. In Webflow: the
  folder is made by the person (the API can't create folders); the
  starter's single page `/design/style-guide` is removed only after its
  Forms row (the reserved Label and Form classes) is on one of the new
  pages; each page gets `noindex` and is excluded from the sitemap.

## Open decisions

| Item | Now | Options |
| --- | --- | --- |
| Insights / blog | Undecided (note in Figma) | Build list + template · leave out |
| Primary CTA wording | "Talk to Allyant" as the button | Let's Build Differently · Start a Conversation · Discuss Your Project |
| JetBrains Mono (project numbers/meta) | Used 54× | Replace with Geist Mono (already loaded) · keep |
| Careers, Our Approach pages | Brand Vision proposals | Keep · drop |
| Credentials, safety figures | Placeholders | Client to supply |
| Nav "+" menus | Figma marks five links with "+", no menu designed; links go to their pages | Design the menus · drop the "+" |
| Hero round arrow | Figma: "Video is playing", a round arrow on the photo; links to the next section | Play a video · link to a page · keep |
| Why section: Tender, Bid, Build | Figma has copy, results and an image for Design only; the others show [TBC] and Design's image | Client/designer to supply |
| Hero card text | Figma Geist Light 24; built as H4 (Regular 24): no Light weight class | Add Weight Light · keep Regular |

## Stand-in assets and placeholders

- Logo: `../Assets/brand/allyant-logo-*.svg` is a vector trace from Figma ("image 13 [Vectorized]"), cleaned. Ask for the original vector logo before launch.
- Images and videos: Figma exports, stand-ins until final files are supplied.
- Every `[TBC]` in the copy.
