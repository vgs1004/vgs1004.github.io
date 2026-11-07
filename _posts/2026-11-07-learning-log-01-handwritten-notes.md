---
layout: post
title: "Two ways to make handwritten-style notes with AI, and why one costs about 10x less"
series: "Learning Log #1"
summary: "Everyone shares those beautiful handwritten study notes. Here is how they are made, what the simple method costs in tokens, and the cheaper method I use."
date: 2026-11-07 00:00:00 +0100
---

**What I wanted:** handwritten-looking notes for my interview prep (DAX, SQL, Power BI), like the ones people share online, without drawing them by hand.

## Method 1: one big prompt, the AI does everything

The simple way is one long instruction. A typical example:

> "Create complete handwritten-style study notes on DAX as a single HTML file. Make it look like a real spiral notebook: handwriting font, wobbly boxes, highlighter, sticky notes in rotating colours, doodle icons, flow diagrams, a cover page and a contents page. Cover every function with worked examples. Keep all CSS and SVG inline."

**What happens:** the AI has to write the content and all the visual code (HTML, CSS, SVG doodles) in every answer.

**Token cost (my estimates):**
- **What the "look" costs:** the styling engine I use in Method 2 is about 41,500 characters of code, roughly 10,000 tokens. In Method 1 the AI rewrites something like that inside every answer.
- **Markup outweighs content:** markup is usually several times larger than the text it wraps, so a 10-page set of notes can easily run to tens of thousands of output tokens.
- **Every change costs the full amount again:** "make the headings blue" means the whole file is regenerated.
- **Long answers hit limits:** notes get cut off halfway, or the style drifts between runs.

## Method 2: fix the look once, let the AI write only the content

**Step 1: Fix fonts and requirements once (a Python notebook).**
- Handwriting fonts: Kalam, plus Delius for a clearer capital "I" so "I2" never reads as "12".
- Packages: `markdown-it-py`, `mdit-py-plugins`, `beautifulsoup4`, `pyyaml` and `playwright` (with Chromium to print the PDF). The first run installs everything; later runs take seconds.
- Settings live in one cell: input file, output file, ink colours, optional "my notes" lines at the bottom of each page.

**Step 2: Claude writes content only, using web sources.**
- I give it a short prompt (about 600 tokens) plus my two-page format guide (about 570 tokens). The guide lists the markers the notebook understands: `# Page`, `==highlight==`, `[[Ctrl]]` keys, `{r:red ink}`, `> tip:`, `::: box`, `::: steps`, `::: flow`, `> takeaway:` and so on.
- The prompt asks for the official docs, a real business dataset, worked examples with correct results, common traps, and a takeaway on every page.
- Claude returns one plain Markdown file. No styling code at all.

**Step 3: Run the notebook.**
- It reads the Markdown and draws the spiral binding, wobbly boxes, clouds, sticky notes and doodles, then prints the PDF locally with Playwright.
- Styling costs zero tokens. Re-render as often as you like.

## Side by side

| | Method 1: AI does everything | Method 2: content only + Python |
|---|---|---|
| What the AI writes | Content + all styling code | Content in plain Markdown |
| Output tokens | High, mostly markup | Roughly a tenth (estimate) |
| Change a colour | Pay again for the whole file | Edit one setting, re-run, free |
| Consistency | Varies run to run | Same look every time |
| Long topics | Gets cut off | Split into several Markdown files |
| Setup | None | One-time notebook setup |

## What I learned

- **Separate content from design.** It's the same idea as a template in Word or a theme in Power BI.
- **Spend AI tokens on thinking, not formatting.**
- **A two-page "format guide" is a better prompt than a long description of the look.**
- **You don't need to be a developer.** I built the notebook by describing what I wanted to Claude and checking each page.
