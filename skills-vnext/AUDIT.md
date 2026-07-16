# Skill Library Audit — 2026-07-16

Scope: all 33 skills under `/mnt/skills/examples/` and `/mnt/skills/public/`.
Score = how much a rewrite of the SKILL.md would improve actual output quality
(High / Medium / Low), not how "unfinished" the file looks.

## Ranked list

### High — rewritten in this pass (see sibling folders)

1. **brand-guidelines** — Used every time anything needs the company look, but it's a
   feature list ("Smart Font Application", "Maintains color fidelity") with zero
   application judgment: no color roles, no contrast rules, no accent discipline, no
   examples. Every use forces improvisation with 7 hex codes.
2. **theme-factory** — The apply step is one sentence ("apply colors and fonts
   consistently, ensure proper contrast"). Theme files give 4 colors with no role
   mapping, so decks come out with accent-colored body text and unreadable pairings.
   Highest ratio of output variance to instruction in the library.
3. **internal-comms** — High-visibility recurring output (leadership reads it). The
   router works, but neither it nor the example files encode what separates a good
   update from a bad one: outcomes vs. activity, metrics vs. adjectives, honest
   Problems sections. No worked before/after anywhere.
4. **mcp-builder** — Solid four-phase skeleton, but the advice is generic ("clear
   descriptions", "actionable error messages") exactly where MCP quality is won or
   lost. Zero worked examples of a good vs. bad tool description, error message, or
   response shape.

### Medium — real gains available, not top-4

5. **canvas-design** — The "countless hours / master craftsman" incantation repeats ~8
   times; repetition burns context without adding control. Concrete craft checks
   (margins, overlap, palette limits — which it does have) would do the work better.
   Output is subjective, so gain is less certain than the top 4.
6. **algorithmic-art** — Same incantation problem, but the viewer template + parameter
   discipline already carry most of the quality; lower variance than canvas-design.
7. **doc-coauthoring** — Works, but rigid and verbose: every step spelled out twice
   (artifact vs. file branches). Tightening + judgment on when to skip stages would
   improve the *experience* more than the *output*.
8. **skill-creator** — High meta-leverage (shapes all future skills) and the content is
   sound, but chatty framing ("Cool? Cool.") and buried best practices. Moderate gain.
9. **event-planning** — Good checklist scaling; would benefit from budget-tradeoff
   examples, but the flow is sound.
10. **benepass-reimbursement** — Brittle by nature (pixel-level UI walkthrough); a
    rewrite can't fix UI drift, and the troubleshooting section already covers the
    known quirks.
11. **slack-gif-creator, financial-calculator, return-refund, hire-help, file-form,
    file-expenses, grocery-shopping, meal-delivery** — Consistent concierge/toolkit
    family; each is serviceable with clear flows. Marginal, diffuse gains only.

### Low — leave alone

- **learn** — Exemplary. Encodes diagnosis-before-teaching, pressure-handling, failure
  modes. This is the bar the rest of the library should meet.
- **morning** — Deeply engineered: verified render pipeline, voice rules, injection
  guardrails. Don't touch.
- **setup-writing-style** — Thorough and security-conscious (PII, heredoc injection,
  consent gates). Don't touch.
- **frontend-design** — Already encodes anti-default calibration and self-critique.
- **call-to-book, cancel-unsubscribe, prescription-refill** — Newest-generation
  concierge skills; they already encode judgment (impatience vs. stuck, tier labels,
  fallback planning).
- **docx, pptx, xlsx, pdf, pdf-reading, file-reading** — Dense, battle-tested gotcha
  lists. Every line is load-bearing.
- **product-self-knowledge** — A router doing its one job (defer to live docs).
- **web-artifacts-builder** — Thin, but the scripts do the work; the SKILL.md is glue.

## Gaps — at most 2 net-new skills worth building

**1. travel-planning** — The concierge suite covers food, errands, hired help, returns,
forms, calls, and cancellations, but not its highest-stakes vertical: trips. A
`travel-planning` skill in the house concierge style would work backward from fixed
constraints (dates, budget, loyalty programs from memory), search flights/hotels,
present 2–3 options per leg with total-cost framing, hold everything for explicit
confirmation before booking, and assemble a day-by-day itinerary artifact with
confirmation numbers, calendar events, and check-in reminders. It earns its place
because trips are multi-leg and stateful — exactly where an unguided session loses
track of budget and constraints — and because the existing skills (call-to-book,
event-planning) each cover only a fragment. Not built: it needs real decisions about
which booking platforms to target and how far autonomy should go with money.

**2. meeting-follow-up** — `morning` looks forward; nothing looks backward. With
Fireflies + Gmail + Calendar connected, a `meeting-follow-up` skill would take a
meeting (or "yesterday's meetings"), pull the transcript, and produce the three things
people actually owe after a meeting: a crisp recap (decisions, owners, deadlines — not
a transcript summary), drafted follow-up messages in the user's voice (via
my-writing-style when present), and proposed calendar holds for committed work. Voice
rules from `morning` apply: observe and hand over, never scold. It earns its place
because follow-through is the single most repeated post-meeting task and every piece
of it is already reachable through connected tools. Not built: transcript access
patterns and send-vs-draft boundaries deserve a deliberate spec, not a guess.

No other net-new skills recommended — the library already over-covers document
production and personal errands, and padding it would dilute triggering accuracy.
