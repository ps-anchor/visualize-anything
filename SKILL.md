---
name: visual-generation
description: "Use this skill when the visualize-anything Stop hook has decided to generate a reel. Covers slide structure, image sourcing strategy (Unsplash primary, generated SVG fallback), parallax pattern, and the responsive reel template that switches between vertical (mobile, Snapchat/Instagram-style) and horizontal (web, inline-in-chat carousel)."
---

# Visual Reel Generation

## Goal
Turn the prior response into a 1–10 slide reel a visual learner can flip through. Each slide should add a *new piece of understanding*, not restate text.

## Slide budget
- Hard ceiling: **10 slides**
- Soft target: **3–7 slides** for most topics
- One concept per slide. If a concept needs two slides, split it; if two concepts fit one image, merge.

## Slide types (pick what fits)
- **Title slide** — topic + one-line hook
- **Anchor image** — a real-world photo that grounds the concept (parallax-enabled)
- **Diagram** — SVG showing structure, flow, or comparison
- **Analogy** — visual mapping of the unfamiliar to the familiar (left side: concept; right side: everyday equivalent)
- **Step** — one frame of a process; sequence multiple step slides for processes
- **Timeline** — for historical or sequential topics
- **Closer** — single takeaway sentence; no decoration

## Image sourcing strategy
1. **Try Unsplash first** via the Unsplash API (key in `UNSPLASH_ACCESS_KEY` env var). Search query should be the slide's core noun, not the whole concept. For "the water cycle: evaporation," search `ocean steam` or `sunlight water`, not `water cycle evaporation`.
2. **Fall back to generated SVG** when Unsplash returns nothing relevant or the concept is abstract (legal frameworks, software architecture, math). Generate the SVG inline using design tokens from the template.
3. **Never** use placeholder gray boxes. If both fail, skip the image and use a typographic slide (large pull-quote on solid background).

## Parallax pattern
For Unsplash slides, the image sits in a layer that translates on scroll/swipe at 0.6× the foreground layer. The template handles this — just provide:
- `bgImage`: the Unsplash URL
- `fgText`: the slide caption (max ~20 words)
- `fgAccent`: optional small element (icon, number, label)

## Reel template
Located at `${CLAUDE_PLUGIN_ROOT}/templates/reel.html`. It's a single self-contained HTML file that:
- Detects viewport width and switches layout: **<768px = vertical full-screen slides (mobile reel)**, **≥768px = horizontal carousel inline-style (web)**
- Mobile: tap left third = previous, tap right third = next; replay button appears on the last slide
- Web: left/right arrow buttons + keyboard arrows; replay button on the last slide
- Slides are passed in as a JSON array at the top of the file — that's all the generator needs to populate

## Caption discipline
- Max **20 words per slide caption** (mobile readability)
- Plain language, no jargon the prior response didn't already define
- One idea per caption; if you want to add detail, add another slide

## What NOT to do
- Don't recap the entire response. The reel complements the text; it doesn't duplicate it.
- Don't include code blocks in slides. Code belongs in the chat response.
- Don't use stock-photo clichés (handshakes, lightbulbs, generic "team meeting") unless they're genuinely the best fit.
- Don't generate more than 10 slides even if the topic is rich — pick the 10 that matter most.

## Output checklist before opening browser
- [ ] Slide count ≤ 10
- [ ] Every slide has either a real image, a generated SVG, or an intentional typographic layout
- [ ] Captions ≤ 20 words each
- [ ] Last slide has a clear takeaway, not a fade-out
- [ ] HTML file written to `.visualize-anything/reel-{timestamp}.html`
- [ ] Single status line printed: `📽  Reel ready: <path>  (N slides)`
