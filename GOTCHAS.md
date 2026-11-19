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

<!-- Add new entries here, newest first. -->
### 2026-11-19 · Icons rebuilt as SVG / custom elements instead of the Icon component
- Area: designer
- Scope: template-candidate
- Symptom: on earlier builds Claude exported Figma icons as SVG or made custom elements, although the starter site has a Phosphor Icon component.
- Cause: `webflow-prototype/figma.md` (Assets: "icons as SVG") and `prototype/README.md` (`images/`: "icons as SVG") point the prototype at SVG exports, and nothing said to check the site's components first.
- Fix: AGENTS.md › Reuse what the site already has: query the site before making anything; every UI icon is an Icon instance (`ph ph-<name>`); SVG only for logo and illustrations. The two skill lines should be changed in the template.
- Status: open
- Found by: human
