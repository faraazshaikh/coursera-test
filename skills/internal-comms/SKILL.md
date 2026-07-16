---
name: internal-comms
description: A set of resources to help me write all kinds of internal communications, using the formats that my company likes to use. Claude should use this skill whenever asked to write some sort of internal communications (status reports, leadership updates, 3P updates, company newsletters, FAQs, incident reports, project updates, etc.).
license: Complete terms in LICENSE.txt
---

## When to use this skill

To write internal communications, use this skill for:
- 3P updates (Progress, Plans, Problems)
- Company newsletters
- FAQ responses
- Status reports
- Leadership updates
- Project updates
- Incident reports

## How to use this skill

To write any internal communication:

1. **Identify the communication type** from the request
2. **Load the appropriate guideline file** from the `examples/` directory:
    - `examples/3p-updates.md` - For Progress/Plans/Problems team updates
    - `examples/company-newsletter.md` - For company-wide newsletters
    - `examples/faq-answers.md` - For answering frequently asked questions
    - `examples/general-comms.md` - For anything else that doesn't explicitly match one of the above
3. **Follow the specific instructions** in that file for formatting, tone, and content gathering

If the communication type doesn't match any existing guideline, ask for clarification
or more context about the desired format. The guideline files own the format and tone
rules for their type — follow them exactly. What follows below is the judgment layer
that applies to *every* type.

## What separates a good internal comm from a bad one

The reader is busy and has less context than the author. Every choice below follows
from that.

- **Outcomes, not activity.** "Worked on the migration" tells the reader nothing;
  "migrated 6 of 9 services, on track for the 30th" tells them everything. If a
  sentence describes effort rather than a result or a decision, rewrite it or cut it.
- **Numbers beat adjectives.** "Significantly improved latency" → "cut p95 latency
  from 900ms to 210ms." If there's no number, name the concrete artifact ("shipped
  the v2 onboarding flow to 100% of new signups").
- **Lead with what the reader needs, not the chronology.** The decision, the ship,
  the risk — first. Background only if the comm fails without it.
- **Problems sections are for real problems, honestly stated.** A problems/risks
  section that says "none" every week is a tell that it's decorative. A real problem
  names the thing, the impact, and the ask: "Hiring: still down 2 backend engineers;
  Q3 roadmap slips ~3 weeks unless we borrow from Platform — decision needed by
  Friday." Softening a problem into vagueness ("some resourcing challenges") wastes
  the one channel that exists to surface it.
- **Calibrate to the least-context reader.** Leadership updates and newsletters go
  to people who don't know the team's acronyms — expand or drop them. A project
  update to the immediate team can use its shorthand.
- **Never invent facts.** Metrics, dates, and names come from the user or from
  gathered sources (Slack, docs, email — per the guideline file). A wrong number in
  a leadership update is worse than a missing one: if a figure is unconfirmed, mark
  it ("~40%, confirming with data team") or leave it out and say what's missing.

### Worked example (quality bar, any format)

Weak: "The team made great progress this week on several fronts. We continued
working on the new dashboard and had productive discussions about the API redesign.
There are a few challenges but we're working through them."

Strong: "Shipped the usage dashboard to all internal users (48 DAU in week one).
Chose option B for the API redesign after Tuesday's review — spec going out Monday.
Problem: the dashboard's export path depends on the deprecated reports service;
we need Infra's migration date before committing to external launch."

Same team, same week. The first is unfalsifiable filler; the second gives a reader
three things they can act on. Notice the strong version is also *shorter* — density,
not length, is what reads as thorough.

## Common failure modes to check before delivering

- Burying a decision or a risk in the middle of a paragraph
- Listing meetings held instead of what they concluded
- Praise-padding ("huge thanks to the amazing team!") in formats whose guideline
  file doesn't call for it — warmth belongs in newsletters, not status reports
- Hedging every claim ("we believe we may be on track") — commit or flag, don't blur
- Length creep: if the guideline file gives a length or read-time target, treat it
  as a hard budget and cut the weakest content to fit, not the formatting

## Keywords
3P updates, company newsletter, company comms, weekly update, faqs, common questions, updates, internal comms
