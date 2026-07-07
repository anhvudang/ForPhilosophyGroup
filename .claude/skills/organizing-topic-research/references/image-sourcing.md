# Image Sourcing

Detailed guidance for Step 4 of the organizing-topic-research skill.

## When an Image Earns Its Place

Add an image only if it increases understanding. Good candidates:

- **Historical events** — a battle, a signing, a location as it looked at the time.
- **People** — portraits, busts, photographs of figures being discussed.
- **Artifacts & places** — manuscripts, buildings, maps, objects.
- **Demos & diagrams** — a process flow, timeline, or labeled illustration that a
  paragraph cannot convey as fast.

Skip: generic stock photos, decorative flourishes, low-relevance filler.

## Where to Look (prefer freely licensed)

| Source | Best for | Licensing note |
|--------|----------|----------------|
| Wikimedia Commons | Almost anything historical | Public domain / CC — check each file's page |
| Wikipedia article media | Figures, events | Usually mirrors Commons |
| National / museum archives | Artifacts, manuscripts | Many public-domain scans |
| Government / institutional archives | Historical photos, documents | Often public domain |

Avoid images whose license forbids reuse. When in doubt, prefer public-domain or CC0.

## Licensing & Attribution

For every image, record next to it (in the README or an `images/CREDITS.md`):

- Source page URL (not just the raw file URL)
- Author/creator if known
- License (e.g., Public Domain, CC BY-SA 4.0)

Public-domain images need attribution only as courtesy; CC-BY variants **require** it.

## Fetching & Placement

1. Download the image file into `<output>/images/` with a descriptive kebab-case name
   (e.g. `marcus-aurelius-bust.jpg`), not a hash or query-string name.
2. Reference with a relative path and descriptive alt text:
   `![Bust of Marcus Aurelius, Musei Capitolini](images/marcus-aurelius-bust.jpg)`
3. If the file cannot be downloaded, link to the stable source page instead of embedding
   a hotlinked URL that may break: `[See portrait (Wikimedia)](https://…)`.
4. Place each image next to the point it supports, not in a lump at the top.

## Failure Handling

- Image search returns nothing relevant → omit the image; the point stands on text.
- Download fails → link to the source page (see above).
- License unclear → do not embed; link out or skip. Never assume reuse is allowed.
