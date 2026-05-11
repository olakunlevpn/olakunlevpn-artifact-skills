# olakunlevpn-artifact-skills

A lightweight CSS library plus a set of Agent Skills that teach an AI agent to produce polished, single-file HTML artifacts — status reports, post-mortems, implementation plans, comparison sheets, roadmaps, annotated code reviews, concept explainers, and interactive editors.

The library is `artifact.css`. The skills are folders that the agent reads before generating output. Both halves work together: the library provides the visual vocabulary; the skills tell the agent which patterns to reach for and when.

## What's here

```
olakunlevpn-artifact-skills/
├── README.md
├── PLAN.md                                  master plan / decisions / roadmap
├── artifact.css                             canonical library (1 file)
│
├── artifact-css/                            foundation skill (load first)
│   ├── SKILL.md
│   ├── artifact.css
│   └── examples/
│       ├── basic.html                       every component on one page
│       └── kitchen-sink.html                expanded preview
│
├── artifact-report/                         status / post-mortem / PR writeup
│   ├── SKILL.md
│   ├── artifact.css
│   └── templates/
│       ├── status-report.html
│       ├── incident-report.html
│       └── pr-writeup.html
│
├── artifact-plan/                           plans / comparisons / roadmaps
│   ├── SKILL.md
│   ├── artifact.css
│   └── templates/
│       ├── implementation-plan.html
│       ├── comparison-sheet.html
│       └── roadmap.html
│
├── artifact-review/                         reviewer-side annotated PR reviews
│   ├── SKILL.md
│   ├── artifact.css
│   └── templates/
│       └── annotated-review.html
│
├── artifact-explainer/                      concept primers with progressive disclosure
│   ├── SKILL.md                             uses jQuery + jQuery UI
│   ├── artifact.css
│   └── templates/
│       └── explainer.html
│
├── artifact-editor/                         interactive editors with export
│   ├── SKILL.md                             uses jQuery + jQuery UI
│   ├── artifact.css
│   └── templates/
│       ├── triage-board.html
│       ├── feature-flag-panel.html
│       └── prompt-tuner.html
│
├── artifact-deck/                           slide decks · arrow-key navigation
│   ├── SKILL.md
│   ├── artifact.css
│   └── templates/
│       └── arrow-key-deck.html
│
├── artifact-flow/                           clickable prototypes · multi-screen
│   ├── SKILL.md
│   ├── artifact.css
│   └── templates/
│       └── clickable-flow.html
│
├── artifact-diagram/                        flowcharts · module maps · SVG figure sheets
│   ├── SKILL.md
│   ├── artifact.css
│   └── templates/
│       ├── flowchart.html
│       ├── module-map.html
│       └── svg-figure-sheet.html
│
├── artifact-design/                         design system + component variants
│   ├── SKILL.md
│   ├── artifact.css
│   └── templates/
│       ├── design-system.html
│       └── component-variants.html
│
└── artifact-sandbox/                        animation playground · live CSS spec
    ├── SKILL.md                             uses jQuery + jQuery UI sliders
    ├── artifact.css
    └── templates/
        └── animation-sandbox.html
```

## The CSS library at a glance

`artifact.css` is a single file. No JavaScript, no build step, no external dependencies.

It ships:

- A token system on `:root` — color, type, spacing, radius, shadow. Override any token to retheme.
- Base typography for h1-h4, body, `.eyebrow`, `.lead`.
- Section chrome (`.sec-head`, `.sec-intro`, `hr.rule`).
- Surfaces (`.panel`, `.callout`, `.note` variants, `.prompt-box`).
- Inline atoms (`.tag`, `.pill`, `.badge`, `.avatar`, `.dot`).
- Stats (`.stats` + `.stat`) and key-value strips (`.kv-strip`, `.kv-strip-stack`).
- Tables (`.table`) with tabular figures via `.num`.
- Timelines (`.timeline` + `.milestone`).
- Code blocks (`.code-block`) and diffs (`.diff` + `.diff-row` variants).
- Layout helpers (`.grid-2/3/4/auto`, `.row*`, `.stack*`).
- Small utility set (`.muted`, `.accent`, `.mono`, `.mt-*`, `.mb-*`).
- Print styles.

## The interactivity layer (optional)

Two skills opt-in to a small interactivity layer: **`artifact-explainer`** (collapsible sections, tabbed code samples, glossary tooltips) and **`artifact-editor`** (drag-and-drop boards, sliders, dialogs, datepickers).

The layer is **jQuery 4.0.0** + **jQuery UI 1.14.2** loaded from the official CDN with SRI integrity hashes. Other skills (report, plan, review) are pure HTML + CSS and load no JavaScript.

```html
<link rel="stylesheet"
      href="https://code.jquery.com/ui/1.14.2/themes/base/jquery-ui.min.css"
      integrity="sha384-pUvA/6DQjteMxpaV6uGxZ1QuYrFLJgrLMvBWf06VcJIg6ky/Y5m3UZJlrv11V1I+"
      crossorigin="anonymous">
<script src="https://code.jquery.com/jquery-4.0.0.min.js"
        integrity="sha384-fgGyf7Mo7DURSOMnOy7ed+dkq5Job205Gnzu6QIg0BOHKaqt4D76Dt8VlDCzcMHV"
        crossorigin="anonymous"></script>
<script src="https://code.jquery.com/ui/1.14.2/jquery-ui.min.js"
        integrity="sha384-tBcEcHGtNy7/Mx08+YxuvQ6v6s0N2jgehtFiT+bLtGwTj/txXtB/L5GqXfggm5sS"
        crossorigin="anonymous"></script>
```

Every interactive template degrades gracefully: with JS disabled, accordions render fully expanded, tabs render stacked, sliders fall back to `<input type="range">`, dialogs fall back to native `<details>`/`<summary>` markup. See `PLAN.md` for the full fallback contract.

## How an agent uses it

1. Agent reads `artifact-css/SKILL.md` first — design tokens, page skeleton, component vocabulary, voice rules, and the optional interactivity layer.
2. Agent reads the pattern-specific skill for the archetype it's producing (`artifact-report/SKILL.md` for a weekly status, `artifact-plan/SKILL.md` for an implementation plan, etc.).
3. Agent picks the matching template from `<skill>/templates/`, fills in the `{{PLACEHOLDER}}` tokens, and either links to `../artifact.css` (for shipped folders) or inlines the CSS contents into a `<style>` block (for single-file portability).

## Install

### Claude / Claude Code

Drop the whole folder into `~/.claude/skills/` (or wherever your Claude install reads skills from). Each `artifact-*/` folder is a skill the agent can load on demand.

Or clone:

```sh
git clone https://github.com/olakunlevpn/olakunlevpn-artifact-skills ~/.claude/skills/olakunlevpn-artifact-skills
```

### skills.sh marketplace

Install from the registry once published:

```sh
skills.sh install olakunlevpn-artifact-skills
```

### Using the CSS alone (no agent)

Host `artifact.css` anywhere or inline it into any HTML file. The templates under `artifact-*/templates/` are usable starting points with `{{PLACEHOLDER}}` tokens you replace by hand or via your favorite template engine.

## Skill catalogue

| Skill | Triggers on | jQuery? |
|---|---|---|
| `artifact-css` | Foundation — always loaded for any HTML artifact | optional |
| `artifact-report` | Status reports · post-mortems · incident timelines · PR writeups | no |
| `artifact-plan` | Implementation plans · comparison sheets · roadmaps | no |
| `artifact-review` | Annotated PR reviews · code-review writeups · inline diff feedback | no |
| `artifact-explainer` | Concept primers · feature explainers · "how X works" deep-dives | **yes** |
| `artifact-editor` | Triage boards · feature-flag panels · prompt tuners · any interactive editor | **yes** |
| `artifact-deck` | Slide decks · presentations · "Friday demo" · lightning talks | no |
| `artifact-flow` | Clickable prototypes · multi-screen mockups · UX walkthroughs | no |
| `artifact-diagram` | Flowcharts · module maps · architecture diagrams · SVG figure sheets | no |
| `artifact-design` | Design systems · component variants · brand kits · style guides | no |
| `artifact-sandbox` | Animation playgrounds · motion spec tuning · CSS effect demos | **yes** |

**OUTPUT CONTRACT (all skills, no exception)** — every `artifact-*` skill produces a single `.html` file. Never `.md`. Never markdown. Never plain text. The file may contain multiple internal views (slide decks, clickable flows, multi-step wizards) but the deliverable is one `.html` file.

## License

MIT. See `LICENSE` (if present) or treat the contents as MIT-licensed.

## Related skills by the same author

- [olakunlevpn-clean-code-skills](https://github.com/olakunlevpn/olakunlevpn-clean-code-skills)
- [olakunlevpn-clean-export-skills](https://github.com/olakunlevpn/olakunlevpn-clean-export-skills)
- [olakunlevpn-laravel-skills](https://github.com/olakunlevpn/olakunlevpn-laravel-skills)
- [olakunlevpn-filament-skills](https://github.com/olakunlevpn/olakunlevpn-filament-skills)
- [olakunlevpn-git-skills](https://github.com/olakunlevpn/olakunlevpn-git-skills)
- [olakunlevpn-deployment-skills](https://github.com/olakunlevpn/olakunlevpn-deployment-skills)
- [olakunlevpn-react-skills](https://github.com/olakunlevpn/olakunlevpn-react-skills)
- [olakunlevpn-frontend-first-skills](https://github.com/olakunlevpn/olakunlevpn-frontend-first-skills)
