---
name: customer-branded-html-deck
description: Use this skill when the user wants a customer-branded, self-contained HTML presentation, including creating a deck from a customer website and linked content, converting or restyling a PPTX, or preparing an interactive decision deck. Apply it to research first-party branding and source material, embed verified official marks, build an accessible animated 16:9 deck, preserve claim traceability, and validate the result visually and technically.
compatibility: Requires network access for supplied URLs and brand assets. PPTX extraction requires ZIP support; optional rendering may use LibreOffice, PowerPoint, Chromium, or equivalent tools already available.
metadata:
  version: "1.0.0"
---

# Customer-branded HTML deck
Create an original, customer-specific HTML presentation grounded in supplied
content and the customer's current public visual identity. Treat the customer
website and any supplied deck as evidence, not as a page or template to copy.

## Inputs
Accept these inputs:

| Input | Required | Notes |
| --- | --- | --- |
| `customer_website` | Yes | Public customer website used to identify the current brand system and first-party assets. |
| `source_pptx` | No | Local `.pptx` path or user-supplied URL. Use it for content, assets, layouts, notes, and style evidence. |
| `content_urls` | No | One or more authoritative pages whose content should drive the story. |
| `brief` | No | Audience, meeting, objective, decision, required sections, tone, slide count, and output name. |
| `output_location` | No | Target folder or knowledge system. Confirm or infer it from the workspace. |

At least one content source is needed: a PPTX, a content URL, or a substantive
brief. If the website is the only input, ask for the subject or intended
decision before drafting.

If both a PPTX and content URLs are supplied:

1. Use the customer website as the authority for the current visual identity.
2. Use the PPTX as the authority for customer-provided content and internal
   context, unless a newer authoritative source contradicts it.
3. Use content URLs as the authority for current product facts, limits,
   availability, dates, and terminology.
4. Surface conflicts instead of silently choosing a convenient version.

## Required outcome
Deliver one self-contained HTML file unless the user asks for another package.
The deck should:

- Use the customer's actual logo from a first-party source.
- Use actual product and service logos from first-party brand or documentation
  sources; never replace them with initials or a hand-drawn approximation.
- Reflect the customer's visual language without reproducing its website.
- Preserve factual traceability and distinguish sourced claims from proposals.
- Work without third-party JavaScript, font, image, analytics, or CDN calls.
- Support keyboard, touch, fullscreen, overview, direct-link, reduced-motion,
  and print/PDF use.
- Remain readable if JavaScript, animations, or remote resources fail.

## Non-negotiable asset rules
1. **No approximated logos.** Do not draw a customer mark with CSS, substitute
   letter badges such as `ADO` or `GH`, or invent a look-alike icon.
2. **Use first-party provenance.** Prefer, in order:
   - The customer's or vendor's official website or brand toolkit.
   - An official product documentation repository or official icon pack.
   - An official open-source icon library owned by the vendor.
3. **Do not use logo-aggregation sites as proof of authenticity.** They may
   help locate an asset, but the final asset must be verified against a
   first-party source.
4. **Preserve geometry.** Keep the original `viewBox`, aspect ratio, paths,
   colors, and lockup. Do not stretch, skew, redraw, add effects, or recolor a
   mark unless the vendor provides that variant.
5. **Choose the correct contrast variant.** Use an official light mark on dark
   surfaces and an official dark mark on light surfaces. If only one variant
   exists, place it on a neutral container that preserves its official colors.
6. **Sanitize SVG safely.** Remove scripts, event handlers, tracking metadata,
   and unnecessary external references. Rename internal SVG IDs when embedding
   multiple assets so gradients, masks, filters, and clip paths cannot collide.
   Do not alter visible artwork.
7. **Respect font and image rights.** A public font URL proves technical
   availability, not redistribution permission. If embedding is not clearly
   permitted, use a metrically suitable local fallback and document the
   substitution. Use customer-provided or first-party imagery only when its use
   is appropriate for the requested deck.
8. **Do not fabricate an unavailable mark.** If no official asset can be
   verified, use the correctly styled product name as text, state that the mark
   is unavailable, and ask the user before using a third-party substitute.

## Detailed workflow
### 1. Establish the job and workspace
1. Record the audience, purpose, desired decision, presentation length, and
   output location from the brief.
2. Confirm that the customer website is public and that any supplied file or
   URL is accessible. Do not bypass authentication, paywalls, age gates,
   robots controls, or access restrictions.
3. Create a scratch directory outside the final customer folder.
4. Never edit the source PPTX in place. Copy it to the scratch directory and
   retain the original filename and checksum.
5. Inventory available tools before installing anything. Prefer existing:
   - HTML and CSS fetchers
   - Browser automation or a shared browser
   - ZIP extraction
   - Python with `python-pptx`
   - PowerPoint or LibreOffice rendering
   - Chromium or Edge headless rendering
   - Image inspection and hashing tools
6. Create an evidence log containing every fetched URL, local input path,
   retrieval time, checksum, and intended use.

### 2. Extract the customer website's visual system
Fetch the homepage and any clearly relevant brand, about, media, investor, or
design-system page. For each page:

1. Save the raw HTML.
2. Resolve and inspect linked stylesheets, manifests, favicons, Open Graph
   images, structured data, and first-party image assets.
3. Follow only assets required to understand the brand. Do not crawl the site.
4. Capture a full-page and viewport screenshot when a browser is available.
   Screenshots provide layout evidence; they are not a substitute for source
   assets.

Extract and document:

- **Logo system:** header and footer lockups, symbol-only variants, light/dark
  variants, SVG `viewBox`, intrinsic size, clear-space behavior, and source URL.
- **Typography:** font families, weights, fallback stack, capitalization,
  headline scale, body scale, letter spacing, and line height. Inspect
  `@font-face` declarations and CSS variables, but do not embed font files
  without appropriate permission.
- **Color palette:** CSS custom properties, repeated literal colors, gradients,
  neutrals, surface colors, interactive states, and semantic use. Record exact
  hex/RGB values and where each color appears.
- **Layout language:** content width, whitespace, grid, cards, borders,
  navigation density, corner radius, masks, curves, photography crops, and
  footer treatment.
- **Motion language:** easing, duration, direction, hover behavior, carousels,
  reveals, and reduced-motion handling.
- **Icon language:** stroke versus fill, line weight, container shape, and
  official icon sources.

Create a concise brand profile before designing:

```text
Customer:
Primary logo:
Logo source and checksum:
Official variants:
Primary colors and roles:
Gradient(s):
Headline font / fallback:
Body font / fallback:
Shape and layout motifs:
Motion characteristics:
Do-not-copy elements:
Rights or embedding constraints:
```

Do not infer a whole brand system from one screenshot. Verify important tokens
in HTML, CSS, SVG, or multiple visible components.

### 3. Extract a supplied PPTX
A PPTX is an Open Packaging Convention ZIP archive. Extract it to a dedicated
scratch directory while preserving the original file.

Inspect at minimum:

| PPTX path | Extract |
| --- | --- |
| `ppt/media/` | Original raster images, SVGs, audio, and video. Hash files to detect duplicates. |
| `ppt/theme/` | Theme colors, major/minor fonts, format schemes, effects, and defaults. |
| `ppt/slideMasters/` | Master backgrounds, placeholders, logos, recurring shapes, and global styling. |
| `ppt/slideLayouts/` | Layout-specific placeholders, geometry, and inherited design. |
| `ppt/slides/` | Text, shapes, fills, lines, coordinates, charts, tables, and speaker-facing content. |
| `ppt/slides/_rels/` | Relationships from shapes to media, charts, hyperlinks, and other parts. |
| `ppt/notesSlides/` | Speaker notes and source annotations. |
| `ppt/charts/` and `ppt/embeddings/` | Chart definitions and embedded workbooks or objects. |
| `ppt/fonts/` | Embedded font data. Inventory it; do not redistribute without permission. |
| `docProps/` | Title, author, dates, subject, keywords, and other document metadata. |

Use two complementary extraction passes:

1. **Package pass:** inspect the ZIP/XML directly so no media, relationship,
   theme, master, note, or embedded object is missed.
2. **Presentation-object pass:** use `python-pptx`, PowerPoint automation, or an
   equivalent library to capture slide dimensions, shape order, coordinates,
   text runs, font properties, fills, borders, grouping, and placeholders.

When a renderer is available, render every source slide to PNG or PDF. Compare
the rendering with extracted objects to understand:

- Cropping and focal points
- Layering and transparency
- Master/layout inheritance
- Font substitution
- Diagram meaning
- Which repeated graphics are logos versus decoration

Build these inventories:

1. **Slide content map:** slide number, title, purpose, body copy, notes,
   sources, and proposed reuse.
2. **Asset manifest:** filename, media type, dimensions, checksum, originating
   slide, crop/placement, likely role, and provenance.
3. **Style profile:** page ratio, theme fonts, theme colors, backgrounds,
   headline/body sizes, common margins, grids, cards, and recurring components.
4. **Logo candidates:** every possible logo asset, its source slide, format,
   size, and whether a better vector original exists.

Preserve original media bytes. Convert unsupported EMF, WMF, EPS, or PDF
artwork only for browser rendering, keep the original beside the conversion,
and document the conversion. Prefer a vector source over a screenshot or
upscaled raster logo.

### 4. Extract and verify source content
For each supplied content URL:

1. Fetch the canonical page and record its title, canonical URL, publisher,
   publication or update date, product version, and retrieval date.
2. Capture the sections directly relevant to the requested story.
3. Follow linked prerequisite, security, scope, limits, workflow, or
   post-migration pages only when needed to resolve a claim.
4. Separate:
   - Verbatim product facts
   - Paraphrased facts
   - Customer-provided context
   - Agent recommendations
   - Unknowns requiring confirmation
5. Build a claim ledger: claim, source URL, source section, date, confidence,
   and destination slide.
6. Do not invent customer scale, savings, timelines, readiness, or business
   outcomes. Label proposals and recommendations explicitly.

### 5. Verify every logo and key asset
Before building slides, create an asset provenance table:

| Asset | Role | First-party source | Variant | Checksum | Embedding decision |
| --- | --- | --- | --- | --- | --- |

For customer assets, compare the website asset with any PPTX version. Prefer the
current website lockup unless the user says the PPTX contains an approved
campaign or business-unit variant.

For third-party products and services:

1. Search the vendor brand toolkit or official site.
2. Search official documentation and official repositories.
3. Search official architecture/icon packs.
4. Confirm the mark visually and against vendor usage guidance.
5. Download the smallest appropriate vector original.
6. Record source and checksum.
7. Embed the exact artwork, with collision-safe SVG IDs.

Do not proceed with proxy artwork merely because it is faster.

### 6. Design the story before coding
Write a storyboard with one sentence per slide. Each sentence must state the
single message the audience should retain.

A common decision-oriented sequence is:

1. Customer-specific cover and meeting context
2. Executive proposition
3. Strategic choices or target operating model
4. Architecture or data flow
5. End-to-end journey
6. In-scope versus out-of-scope boundary
7. Scale, economics, or measurable limits
8. Security and governance
9. Readiness gates and risks
10. Recommended customer operating model
11. Roles and responsibilities
12. Decision and next step
13. Sources, design provenance, and assumptions

Adapt or remove sections that do not serve the brief. Do not pad the deck to
match this sequence.

For every slide, specify:

- Purpose and takeaway
- Source claims
- Customer-specific context
- Visual form
- Official assets
- Speaker note or caveat
- Whether the slide is fact, recommendation, or decision

### 7. Translate the brand into an original deck system
Use the extracted brand profile as design tokens:

```css
:root {
  --brand-primary: ...;
  --brand-secondary: ...;
  --brand-surface: ...;
  --brand-text: ...;
  --brand-gradient: ...;
  --font-heading: ...;
  --font-body: ...;
  --motion-ease: ...;
}
```

Create an original presentation grammar from the customer's observable
principles: contrast, scale, whitespace, color roles, geometry, image treatment,
and motion. Do not clone the website header, page sections, or exact layouts.

Use a consistent 16:9 grid, fixed safe margins, typography scale, footer,
section label, slide number, and source treatment. Keep dense reference content
in the appendix or notes rather than shrinking body text excessively.

### 8. Build a self-contained HTML presentation
Use semantic HTML:

- One `<section>` per slide
- Real headings and lists
- Descriptive `aria-label` values
- Buttons for controls
- An `aria-live` region for slide changes
- Meaningful alternative text or hidden labels for informative artwork

Embed local CSS, JavaScript, sanitized SVG, and small images directly in the
HTML. Use data URIs for raster assets when practical. Do not retain analytics,
tracking scripts, remote fonts, or runtime CDN dependencies from source sites.

Implement:

- Responsive 16:9 stage
- Arrow, Page Up/Down, Space, Home, and End navigation
- Fullscreen control
- Slide overview
- URL hash per slide
- Touch swipe
- Progress indicator
- Print stylesheet with one slide per page
- `prefers-reduced-motion` support

Motion must be progressive enhancement:

- Base content is visible.
- Entrance animations may move or reveal content, but must never leave it
  permanently transparent.
- Direct-linked slides must render completely without waiting for a previous
  slide.
- Interrupted animations, headless rendering, print, and reduced-motion mode
  must still show all content.

### 9. Add traceability inside the deck
Add concise source links in slide footers for factual slides. Include a final
source and assumptions slide containing:

- Primary content sources and update dates
- Customer website used for visual analysis
- Official asset sources
- Font substitutions or embedding constraints
- Recommendations clearly identified as recommendations
- Product preview or availability caveats
- Retrieval date

Do not make the appendix a substitute for slide-level attribution when a claim
could be misunderstood without its source.

### 10. Validate technically
Run the smallest available checks that cover the artifact:

1. Parse the HTML with a standards-tolerant parser.
2. Extract inline JavaScript and compile it with a JavaScript parser or
   `new Function(...)` without executing page-derived scripts.
3. Confirm the expected slide count and unique slide labels.
4. Check local links, IDs, SVG references, gradients, masks, filters, and clip
   paths.
5. Search for placeholder badges, approximated logo classes, temporary paths,
   debug text, and remote runtime dependencies.
6. Confirm all embedded asset dimensions and manifest checksums.
7. Confirm the output file size is suitable for its target system.

### 11. Validate visually and functionally
Serve the deck locally rather than opening it only as `file://`.

Render at least:

- Cover
- Architecture or logo-heavy slide
- Densest content slide
- One dark slide
- One light or brand-color slide
- Decision slide
- Sources slide

Prefer rendering every slide at 1600x900. Inspect screenshots for:

- Clipping or overflow
- Font fallback and line-wrap changes
- Logo correctness, contrast, aspect ratio, and clear space
- Overlapping controls
- Low-contrast text
- Inconsistent margins
- Distorted images
- Unresolved SVG definitions
- Content hidden by animation timing

Test direct links to non-first slides. Exercise keyboard navigation, overview,
fullscreen, touch, reduced motion, browser refresh, and print/PDF output.

If a visual check exposes a defect, fix the root cause and rerender the affected
slide plus one neighboring slide. Do not accept a deck merely because the HTML
parses.

### 12. Publish, synchronize, and hand off
1. Resolve the exact customer folder before publishing.
2. If the target application has a persistence API or content model, use it
   instead of copying bytes behind its back.
3. Preserve the final filename, MIME type, byte size, timestamps, and content
   path in the target manifest.
4. Commit only the intended deck and required manifest changes.
5. Pull or fetch before push, avoid force-pushing, and confirm local `HEAD`
   matches the target remote after push.
6. Reload the frontend with a cache-busting revision and open the published
   artifact, not the scratch copy.
7. Verify the published deck still contains the official embedded assets and
   that its source links work.
8. Remove scratch extraction, render, and browser-profile files.

Report:

- Final location
- Slide count
- Main source inputs
- Official marks embedded
- Important assumptions or substitutions
- Commit or publication identifier, when applicable

## Failure and edge-case handling
### Website is heavily scripted or blocks raw fetching
Use a normal browser or user-shared page to inspect rendered styles and network
assets. Do not circumvent access controls. If first-party logo or font assets
still cannot be verified, explain the gap and request an approved brand pack.

### PPTX is encrypted, protected, corrupt, or incomplete
Do not guess missing content. Report the exact extraction failure and ask for
an unlocked or repaired copy. Preserve any successfully extracted evidence
without claiming the extraction is complete.

### Website and PPTX branding differ
Treat the public website as current by default, but flag the difference. A
customer-provided PPTX may intentionally represent a business unit, campaign,
or approved older system. Ask which identity governs the output if the choice
changes the deck materially.

### Embedded font cannot be redistributed
Do not copy it into the HTML. Choose an available fallback with similar width
and weight, rerender all slides because line wrapping can change, and disclose
the substitution.

### Official product logo cannot be sourced
Use the product's official name as text. Never create a proxy icon. Record the
missing asset in the source slide and handoff.

### Source content changes during production
Refetch before final validation. If limits, availability, dates, or terminology
changed, update the claim ledger and affected slides before publication.

### Asset is too large
Preserve an original copy in scratch, then create a presentation-safe derivative
with documented dimensions and compression. Never rasterize a vector logo just
to reduce implementation effort.

## Completion checklist
- [ ] Customer website and content inputs are recorded.
- [ ] PPTX package, objects, notes, masters, layouts, and media were extracted when provided.
- [ ] Brand profile and claim ledger exist.
- [ ] Customer logo is official and first-party.
- [ ] Product and service marks are official and first-party.
- [ ] No approximated logo, initial badge, or stretched mark remains.
- [ ] Font and image embedding rights were considered.
- [ ] Storyboard has one message per slide.
- [ ] Facts, customer context, recommendations, and unknowns are distinct.
- [ ] HTML is self-contained and has no tracking or runtime CDN dependency.
- [ ] Base content remains visible without animation.
- [ ] Navigation, overview, direct links, fullscreen, touch, reduced motion, and print work.
- [ ] Technical checks pass.
- [ ] Representative or all-slide screenshots were inspected.
- [ ] Sources, asset provenance, assumptions, and substitutions are in the deck.
- [ ] Published file and manifest metadata agree.
- [ ] Final commit or publication is synchronized.
- [ ] Scratch files are removed.

## Example invocation
```text
Create an animated customer-branded HTML deck.

customer_website: https://www.example.com/
source_pptx: ./inputs/current-strategy.pptx
content_urls:
  - https://vendor.example.com/product/overview
  - https://vendor.example.com/product/security
brief:
  audience: Customer engineering leadership
  objective: Decide whether to authorize a pilot
  length: 12-15 slides
output_location: Customers / Example Customer / Program
```
