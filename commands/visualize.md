---
description: Manually generate a visual reel for the previous response (or a topic you specify).
---

# /visualize

Generate a reel on demand, bypassing the smart Stop-hook decision.

## Usage
- `/visualize` — generate a reel for your most recent response
- `/visualize <topic>` — generate a reel explaining `<topic>` from scratch

## Behavior
1. Load the `visual-generation` skill.
2. Produce up to 10 slides per the skill's guidelines.
3. Write to `.visualize-anything/reel-{timestamp}.html` and open in browser.

Useful when:
- The Stop hook decided to skip and you disagree
- You want a reel for an older message in the conversation
- You want to visualize a topic Claude hasn't yet explained
