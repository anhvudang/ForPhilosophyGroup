---
name: organizing-topic-research
description: "Use when the user wants to organize existing source data on a topic into a structured README with summary points and supporting images, augmenting it with information found online. Triggers: 'organize this data', 'summarize this topic into points', 'build a readme from these notes', 'research and arrange'. Do NOT use for verifying a single technical claim (use web-research) or producing a from-scratch multi-source report with no existing data (use deep-research)."
durability: encoded-preference
metadata:
  version: 1.0.0
---

# Organizing Topic Research

Turn a pile of source data on a topic into an organized, readable knowledge base:
a `README.md` of concise summary points, deeper detail files when a point runs long,
and relevant images — augmented with information found online.

## Step 0 — Require All Four Inputs (GATE)

Do NOT start work until all four inputs are confirmed. If any is missing, ask for it
and stop. Do not guess or infer a missing input.

| # | Input | What it means | Example |
|---|-------|---------------|---------|
| 1 | **Topic** | What to research/organize | "Roman Stoicism" |
| 2 | **Data location** | Where the source material already lives | `Private/Data/Rome_PH_Data/` |
| 3 | **Organization scheme** | How the user wants points arranged | "By philosopher, chronological" |
| 4 | **Output location** | Where to write the result | `PublicRead/stoicism/` |

Confirm the four back to the user in one line before proceeding, e.g.:
`Topic: X · Data: Y · Arrange by: Z · Output: W — starting.`

If two or more are missing, ask for all of them together (one message), not one at a time.

## Step 1 — Read the Source Data

- Read everything under the **data location**. Enumerate files first (glob), then read.
- Extract facts, names, dates, quotes, and claims. Keep track of what the source
  actually says vs. gaps the source does not cover.
- Note the source language; the output language follows the project's convention, not
  the source's.

## Step 2 — Augment From the Internet

Fill gaps and confirm shaky facts using web search/fetch.

- Search for information the source data lacks or states weakly (dates, context,
  connections, later developments).
- Apply the **Research Claim Verification** rule: a named API/event/date/GUID from a
  source that could not actually be read is a hypothesis, not a fact. If the cited page
  was unreachable, label the claim unverified rather than asserting it.
- Prefer primary/authoritative sources. Record where each augmented fact came from.
- If web tools fail, degrade gracefully (see `web-research`) — do not block the whole
  task on one unreachable page.

## Step 3 — Organize Into Points

Arrange the material **exactly per the user's organization scheme** (input #3). The scheme
dictates the section order and grouping — do not impose your own taxonomy.

Each point:

- Is a self-contained unit under the scheme (a philosopher, an event, a concept…).
- Target **under 500 words**; hard ceiling **1000 words**.
- Leads with the takeaway, then supporting detail. No filler.

**When a point exceeds the limit, or the user asks for depth:**

1. Keep a tight summary (≤500 words) in `README.md`.
2. Write the full treatment to a sibling detail file, e.g. `details/<point-slug>.md`.
3. Link from the README point to the detail file: `[Read more](details/<point-slug>.md)`.
4. The detail file links back to the README: `[← Back to overview](../README.md)`.

## Step 4 — Add Relevant Images

Include images only when they add understanding — **especially historical events, people,
places, artifacts, and demos/diagrams**. Skip decorative images.

- Prefer freely licensed sources (Wikimedia Commons, public-domain archives). Record the
  source URL and license/attribution next to each image.
- Save image files under an `images/` subfolder in the output location; reference with
  relative paths. Add descriptive alt text.
- If you cannot obtain an image file, link to a stable source page instead of embedding a
  hotlinked URL that may rot.
- See `references/image-sourcing.md` for source list, licensing, and placement rules.

## Step 5 — Write Output

- Write `README.md` at the **output location** with the organized points, inline images,
  and links to any detail files.
- Include a short header (topic + one-line scope) and, if points are many, a
  table of contents linking to each point.
- Add a `## Sources` section listing augmented references with URLs.
- Confirm to the user: files written, point count, detail files created, images added.

## Output Layout

```
<output location>/
├── README.md                 # summary points, ToC, images, sources
├── details/                  # only when a point overflows
│   └── <point-slug>.md
└── images/                   # only when images were added
    └── <descriptive-name>.<ext>
```

## Rules of Thumb

- Never fabricate to fill a gap. An honest "the source is silent on X" beats invention.
- The README is the index; detail files are opt-in depth. Do not duplicate full detail
  in both — summarize in README, expand in the detail file.
- Respect project data boundaries: read from wherever the source data lives, write only
  to the output location the user gave.
