# Gotchas

A running log of things that cost time on this project. Agents read it at
the start of every session and add to it when they hit something new (see
the Session protocol in `AGENTS.md`). Never delete an entry — update its
`Status` instead.

Lessons from earlier builds are in the skill, not here:
`.claude/skills/webflow-build/lessons/` (one file per area). Read the file
for the area you work in. Entries tagged `Scope: template-candidate` are
collected from client repos into those files.

## Entry format

```md
### YYYY-MM-DD · Short title
- Area: designer | css | loader | release | mcp | ci | js | perf
- Scope: project | template-candidate
- Symptom: what was observed
- Cause: why it happened
- Fix: what was done, or the workaround
- Status: open | fixed <sha> | upstreamed wf-template <sha>
- Found by: claude | codex | human
```

## This project

### 2026-11-19 · Style guide split across pages, against the skill's one-page layout
- Area: designer
- Scope: template-candidate
- Symptom: the person wants the style guide divided across pages (design › style-guide/<page>), as on TVS; the webflow-style-guide skill fixes it as one page with a section sidebar, and its check expects that page.
- Cause: the skill has no multi-page option; TVS solved it with its own docs layout, which this project did not want.
- Fix: the prototype keeps the skill's generated one page as the source and for the check; `prototype/scripts/style-guide-pages.mjs` cuts it into one page per topic with the same frame and Sg classes, the sidebar listing pages. A multi-page option in the skill would remove the per-project script.
- Status: open
- Found by: human

<!-- Add new entries here, newest first. -->
### 2026-11-19 · Icons rebuilt as SVG / custom elements instead of the Icon component
- Area: designer
- Scope: template-candidate
- Symptom: on earlier builds Claude exported Figma icons as SVG or made custom elements, although the starter site has a Phosphor Icon component.
- Cause: `webflow-prototype/figma.md` (Assets: "icons as SVG") and `prototype/README.md` (`images/`: "icons as SVG") point the prototype at SVG exports, and nothing said to check the site's components first.
- Fix: AGENTS.md › Reuse what the site already has: query the site before making anything; every UI icon is an Icon instance (`ph ph-<name>`); SVG only for logo and illustrations. The two skill lines should be changed in the template.
- Status: open
- Found by: human

### 2026-11-20 · Backdrop blur disappears while a parent animates
- Area: css
- Scope: template-candidate
- Symptom: the glass chip and play button on the hero lost their backdrop blur at rest and all through the scroll animation.
- Cause: any `filter` (even `blur(0)`) or `opacity` below 1 on an ancestor makes it the backdrop root, so a child's `backdrop-filter` only sees that ancestor's own layer, not the photo. GSAP leaves `filter: blur(0rem)` inline after a tween, and autoAlpha on a wrapper sets opacity.
- Fix: animate filter and opacity on the glass element itself (or each child), never on its wrapper; move a wrapper only with transform. `prototype/src/modules/home-hero.ts`.
- Status: open
- Found by: human

### 2026-11-20 · Fluid scale left at the template's 1680 design width
- Area: css
- Scope: template-candidate
- Symptom: every spacing, size and radius on the prototype was 86 % of Figma at 1440 (body 13.7 px).
- Cause: `--size-container-ideal` stayed at the template's 1680 while the Figma frames are 1440; the "set ideal/min/max before the prototype" step was skipped.
- Fix: ideal 1440, min 1280px, in src/styles.css and prototype/src/css/prototype.css. A check comparing body font-size at the Figma frame width with 16 px would catch it.
- Status: open
- Found by: human

### 2026-11-20 · instanceof CSSStyleRule is always false for a frame's rules
- Area: js
- Scope: template-candidate
- Symptom: the prototype canvas's Full page view grew its hero with the frame (10,000 px); the variable list and the breakpoint check came back empty.
- Cause: a same-origin iframe's CSS rules are instances of the iframe window's CSSStyleRule, not the parent's, so `rule instanceof CSSStyleRule` in the parent is false.
- Fix: check the shape (`'selectorText' in rule`, `'conditionText' in rule`). `prototype/src/canvas/frames.ts`.
- Status: open
- Found by: claude

### 2026-11-20 · Environment switcher tests expect port 3000, the project's dev port is 3213
- Area: ci
- Scope: template-candidate
- Symptom: `pnpm test` fails 7 tests in tests/environment-switcher.spec.mjs (Expected "http://localhost:3000/", Received "http://localhost:3213/"); the same on the setup commits, before any build work.
- Cause: the spec hard-codes `http://localhost:3000/`; `pnpm new-project` writes the project's own `devPort` (3213) into webflow-build.config.json, which the loader uses.
- Fix: open. The spec should read `devPort` from webflow-build.config.json (template fix). CI and the staging deploy fail on push until then.
- Status: open
- Found by: claude

### 2026-11-20 · Canvas Full page frame grew without limit
- Area: js
- Scope: template-candidate
- Symptom: in the prototype canvas's Full page view the hero sometimes filled a 10,000+ px frame, and a tall empty area stayed below the footer.
- Cause: the vh freeze ran once at load; styles that arrived later (Vite's module CSS, every hot update) brought vh back, so the hero grew with the frame. The frame was sized from `documentElement.scrollHeight`, which is never less than the frame itself, so any brief growth stuck.
- Fix: re-freeze on every change in the frame's head (MutationObserver), and size the frame from the body's own height. `prototype/src/canvas/frames.ts` (keepViewportUnitsFrozen), `App.tsx`.
- Status: open
- Found by: human

### 2026-11-20 · GSAP folds an element's CSS `scale` into its transform
- Area: js
- Scope: template-candidate
- Symptom: the Featured "Scroll" cursor bubble showed at 60 % size all the time (82 px instead of Figma's 120).
- Cause: CSS hid it with `scale: 0.6` and showed it with `scale: 1`; GSAP moving it with x/y read the computed `scale` into its own transform matrix and set the property to none, so the CSS show/hide scale never applied again.
- Fix: move the element with the CSS `translate` property, eased through a plain object (`gsap.quickTo(pos, 'x', { onUpdate })`), and leave `scale` to CSS. Or keep GSAP transforms and CSS `scale` on different elements. `prototype/src/modules/featured-slider.ts`.
- Status: open
- Found by: human

### 2026-11-20 · smooothy moves the slider on vertical page scrolls
- Area: js
- Scope: template-candidate
- Symptom: with the pointer over the Featured Projects slider, scrolling the page up or down made the slider glide.
- Cause: smooothy (0.0.35) always attaches virtual-scroll to its wrapper, whatever `scrollInput` says, and on a mouse or trackpad it applies the wheel's deltaX with no check that the gesture is sideways; a vertical trackpad scroll always carries a little deltaX.
- Fix: pass `virtualScroll: { mouseMultiplier: 0, touchMultiplier: 0, firefoxMultiplier: 0 }` to switch its wheel input off, and move the slider from our own wheel handler only on a clearly sideways swipe (|deltaX| ≥ 4 and more than 1.5 × |deltaY|). Its own drag and touch swipe are separate and still work. `prototype/src/modules/featured-slider.ts`.
- Status: open
- Found by: human

### 2026-11-20 · Entrances replay on a page swapped in by a transition
- Area: js
- Scope: template-candidate
- Symptom: with the stacked-cards transition, the arriving page showed its content during the transition, then its reveals hid it and played it in again (a flash right after the cards settled).
- Cause: page modules start after the swap (Barba recipe, afterEnter), so their start states (opacity 0, a load intro) apply to content the transition has already shown.
- Fix: `pageArrival.swapped` (motion.ts), set by transition.ts before a swapped page's modules start; stagger-scroll leaves on-screen items as they are and home-hero skips its load intro. Below-the-fold reveals play as usual. `prototype/src/modules/`.
- Status: open
- Found by: claude


### 2026-11-20 · Stacked-cards transition: the orange card has no height
- Area: js
- Scope: template-candidate
- Symptom: the Osmo "Stacked Cards" page transition ran (pages scaled and dropped) but showed no orange card between the pages; it looked like no transition at all.
- Cause: the transition sets `perspective` on the Barba wrapper, which makes it the containing block of its `position: fixed` children; with both pages fixed during the swap the wrapper (G | Page W) has no height, so the fixed `inset: 0` orange layer collapsed to 0 px. In Osmo's demo the wrapper is `<body>`, which keeps the viewport's height.
- Fix: set `minHeight: '100vh'` on the wrapper with the perspective, clear it afterwards. `prototype/src/modules/transition.ts`.
- Status: open
- Found by: human

### 2026-11-20 · Repo CSS written by page builders read Webflow variables
- Area: css
- Scope: template-candidate
- Symptom: 11 `repo-css` blocks (hover and focus states, accordion, deliverables, filters, the stage tabs' fill, the keyboard ring) used `var(--_colors…)`, `var(--_spacing…)`, `var(--_typography-role…)`, against AGENTS.md › CSS policy rule 4: a variable renamed in Webflow silently breaks them.
- Cause: subagents building sections copied the surrounding Designer classes' style into their repo-css rules; nothing checks the rule.
- Fix: each replaced by its approved value (24 references; semantic colours at their Base mode). A check in `pnpm check` that fails on `var(--_` inside a repo-css block would catch it (template).
- Status: open
- Found by: claude

### 2026-11-20 · Home hero snaps after arriving by page transition
- Area: js
- Scope: template-candidate
- Symptom: going to Home from another page, the hero photo snapped bigger the moment the page settled.
- Cause: the transition shows the arriving page before its modules start. The hero module then (1) zoomed the photo to its resting 1.15 and (2) enlarged its frame to the whole stage, drawing the rounded box with clip-path — the photo, cover-fit to a bigger box, grew. On a normal load both happen before the first paint.
- Fix: the resting zoom is on the class (Hero Img `transform: scale(1.15)`), and the transition calls the hero's `fillFrame()` on the arriving page before showing it. Any module that changes layout on start needs the same: its start state on the class, or a prepare step the transition calls. `prototype/src/modules/home-hero.ts`, `transition.ts`.
- Status: open
- Found by: human

### 2026-11-23 · Photos jump after a page transition on five pages
- Area: js
- Scope: template-candidate
- Symptom: arriving by page transition at the Service, Project, Company and Insights article pages, the hero photo jumped (≈ 9 % of the page width) the moment the cards settled; Contact's supplier photo by 2 %; Our Approach's Method band photos by 30 %. Home, What We Do, Projects, Insights and 404 were fine.
- Cause: the same as the Home hero snap: modules start after the arriving page is shown. Parallax enlarged its image to 1.1 and moved it to its scroll position on start (and a `fromTo` drew its 'from' state first); band-open redrew the band's photos a screen wide behind its rounded crop.
- Fix: both export a prepare step that draws the resting state (`prepareParallax`, `prepareBandOpen`), which transition.ts › prepare calls on the arriving page beside the hero's `fillFrame`; parallax also sets its resting state at once on start (no 'from' frame). Found with a check that pumps GSAP's ticker, arrives at each page through the transition and flags any image that moves more than 1 % of the page width in one frame. Any module that changes the top of a page when it starts needs the same prepare step.
- Status: open
- Found by: human
