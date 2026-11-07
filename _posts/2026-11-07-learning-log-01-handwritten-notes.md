---
layout: post
title: "Two ways to make handwritten-style notes with AI, and why I switched"
series: "Learning Log #1"
summary: "Everyone shares those beautiful handwritten study notes. Here is how I first made them with one long prompt, and the method I use now: about 30% fewer tokens, and free layout edits."
date: 2026-11-07 00:00:00 +0100
resources:
  - name: "Handwritten Notes Maker on GitHub"
    url: "https://github.com/vgs1004/MyProjects/tree/main/Handwritten%20Notes%20Maker%20%28Python%29"
    note: "Method 1 prompt and SQL notes, Method 2 notebook, installation guide and n8n notes"
image: /files/log-01-cover.jpg
---

**What I wanted:** handwritten-looking notes for my interview prep (DAX, SQL, Power BI), like the ones people share online, without drawing them by hand.

## Method 1: one long prompt, Claude makes the whole PDF

My first approach was one long prompt, the same one for every topic:

> Create a downloadable PDF called "[TOPIC]: Zero to Confident." Teach me [TOPIC] from scratch. Assume I'm a complete beginner with no prior knowledge of the subject or its tools.
>
> Check official documentation or other authoritative references before writing. Include a short list of the sources you used at the end.
>
> Make the PDF look like handwritten study notes in a spiral notebook, with ruled pages, colored headings, short explanations, and a boxed "Key Takeaway" on each page. Use as many pages as needed to explain the topic clearly.
>
> Start with what [TOPIC] is, why it matters, and how its core concepts fit together. Explain unfamiliar terms in plain English and use everyday analogies before introducing technical details. Build from the basics to practical applications, and label advanced material as optional.
>
> Walk me through a simple first exercise using sample data or materials where relevant. Then include a few practice exercises of increasing difficulty and two end-to-end projects or worked examples. For each one, show what I need, what to do, what result to expect, and how to check my work or troubleshoot common mistakes.
>
> Finish with a quick-reference cheat sheet, beginner Q&A, and a practice roadmap I can follow on my own.

I described the look in one sentence and left everything else (fonts, layout, how many pages) to Claude. Claude researched the topic, wrote the content, designed the pages and built the PDF, all in one go.

It works well for a single guide. The catch is that Claude also has to build the layout each time, and any layout fix (a box that overflows, text out of line) means another round in Claude.

## Method 2: the design is fixed, Claude writes only the content

**Step 1: The design is fixed once, in a Python notebook.**
- Fonts, colours and page style never change: Kalam handwriting, plus Delius for a clearer capital "I" so "I2" never reads as "12".
- Settings live in one cell: input file, output file and ink colours.

**Step 2: Claude writes the content only, in Markdown.**
- I give it my content prompt (about 1,000 tokens) plus my three-page format guide (about 1,200 tokens). The guide lists the markers the notebook understands, such as `==highlight==`, `> tip:`, `::: box`, `> takeaway:`, result chips and coloured code blocks.
- The prompt also sets colour rules: every page gets a subtitle, every result goes in a table or chips, and each page fits on one A4 sheet.
- Claude uses official docs as sources and returns one plain Markdown file. This is the only step that uses Claude, and Markdown is short.

**Step 3: The notebook builds the PDF on my laptop.**
- It turns the Markdown into an HTML page first, because HTML lines the text up properly, then prints that page to PDF.
- It doesn't matter how many HTML files or formats it creates along the way. This all runs locally, so it costs nothing.

**Why I prefer it:**
- **Alignment is handled for me.** Text, tables and boxes line up the same way on every page.
- **Layout edits are free.** Change a colour, a font or the page style in one setting and re-run. No new Claude chat.
- **Every guide looks the same,** so my notes feel like one notebook.
- **Claude's effort goes into the content:** research, explanations, examples and the Key Takeaway on each page.
- **Long topics are easy.** I split them into several Markdown files and build each one locally.

## The two methods as a flow

<div class="flows">
  <div class="flow m1">
    <p class="flow-title">Method 1 · Claude does everything</p>
    <div class="flow-row">
      <div class="node">One long prompt<small>same for every topic</small></div>
      <div class="arrow"></div>
      <div class="node hot">Claude researches, writes and designs<small>reference websites, as many pages as possible</small></div>
      <div class="arrow"></div>
      <div class="node hot">Claude creates the PDF<small>all inside Claude</small></div>
    </div>
    <div class="loop hot-loop">↺ Layout fix? Another round in Claude</div>
  </div>
  <div class="flow m2">
    <p class="flow-title">Method 2 · Claude writes, my notebook builds</p>
    <div class="flow-row">
      <div class="node once">Design fixed once<small>Python notebook: fonts, colours, layout</small></div>
      <div class="arrow"></div>
      <div class="node ai">Claude writes content only<small>Markdown, the only step that uses tokens</small></div>
      <div class="arrow"></div>
      <div class="node free">Markdown to HTML<small>on my laptop, free</small></div>
      <div class="arrow"></div>
      <div class="node free">HTML to PDF<small>on my laptop, free</small></div>
    </div>
    <div class="loop free-loop">↺ Want a change? Edit one setting and re-run, for free</div>
  </div>
</div>

## Average tokens per guide

A planning estimate for a 20-page guide with the same research and roughly the same amount of text. These are averages, not measured usage.

| Work Claude does | Method 1 | Method 2 |
|---|---:|---:|
| My instructions (prompt, plus format guide in Method 2) | ~300 | ~2,200 |
| Research and source reading | ~15,000 | ~15,000 |
| Writing the guide | ~7,000 | ~8,000 |
| Building the PDF, checking pages, fixing layout | ~10,000 | 0 (Python on my laptop) |
| **Total** | **~32,000** | **~25,000** |
| **Of which Claude writes** | **~17,000** | **~8,000** |

So Method 2 uses **about 20 to 30% fewer tokens per guide**, and Claude writes **about half as much**. The saving grows when a layout needs several rounds of fixes, because in Method 2 those happen on my laptop for free.

## What each method produced

<div class="compare">
  <figure>
    <a href="/files/log-01-method1-page.jpg"><img src="/files/log-01-method1-page.jpg" alt="A page from the Method 1 SQL notes"></a>
    <figcaption><strong>Method 1:</strong> SQL notes, 25 pages. Claude designed and built the PDF.</figcaption>
  </figure>
  <figure>
    <a href="/files/log-01-method2-page-v2.jpg"><img src="/files/log-01-method2-page-v2.jpg" alt="A page from the Method 2 n8n notes"></a>
    <figcaption><strong>Method 2:</strong> n8n notes, 42 pages. Same layout every time, built locally for free.</figcaption>
  </figure>
</div>

Both full PDFs are in the <a href="https://github.com/vgs1004/MyProjects/tree/main/Handwritten%20Notes%20Maker%20%28Python%29">project folder on GitHub</a>.

## What I learned

- **Separate content from design.** It's the same idea as a template in Word or a theme in Power BI.
- **Use the AI only where it adds value.** Writing the content needs Claude. Building the PDF doesn't.
- **A short "format guide" is a better prompt than a long description of the look.**
- **You don't need to be a developer.** I built the notebook by describing what I wanted to Claude and checking each page.
