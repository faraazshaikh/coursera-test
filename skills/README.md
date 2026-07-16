# Skill library — updated and new skills

The live library at `/mnt/skills` is a read-only mount, so updates can't be applied
in place. Each folder here is a **complete, installable skill** — SKILL.md plus all
bundled resources — ready to drop into wherever your skills are managed (claude.ai
skill upload, `~/.claude/skills/`, or your skills repo).

## Updated (drop-in replacements for the originals)

| Skill | What changed |
|---|---|
| `brand-guidelines` | Feature list → color-role table, accent discipline, contrast judgment, worked examples (slide / chart / CSS), off-brand reject list |
| `theme-factory` | One-line "apply consistently" → four-role mapping (Background/Ink/accents), contrast rules, worked Ocean Depths example, custom-theme constraints. Themes and showcase unchanged |
| `internal-comms` | Router kept; added the judgment layer: outcomes not activity, numbers beat adjectives, honest Problems sections, weak-vs-strong worked example, failure-mode checklist. `examples/` format files unchanged |
| `mcp-builder` | Generic advice → worked before/after examples for tool descriptions, response shaping, and error messages; workflow-tool heuristic; description-only walkthrough test. References and eval format unchanged |

Bundled resources (`examples/`, `themes/`, `reference/`, `scripts/`,
`theme-showcase.pdf`) are copied verbatim from the originals.

## New (from the gaps identified in `../skills-vnext/AUDIT.md`)

- `travel-planning` — trip planning and booking in the house concierge style:
  flights → lodging → ground in dependency order, true-total-cost framing, running
  budget, explicit confirmation before any money moves, itinerary + calendar output.
- `meeting-follow-up` — post-meeting follow-through: transcript-anchored recap
  (decisions / actions / open questions), follow-up drafts in the user's voice,
  proposed calendar holds; drafts and proposals only until approved.

The audit and pre-promotion rewrite copies live in `../skills-vnext/`.
