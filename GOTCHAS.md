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
