# SLIDE.md

*[日本語版はこちら / Japanese version](README.md)*

**A design-spec format for building slides with AI, plus a Claude Code skill package that generates it.**

![Eyecatch](docs/slide-sample-01.png)

**→ <a href="https://sho-ai-magic.github.io/slide.md/lp.html" target="_blank">View the landing page</a>**

Inspired by [Google's DESIGN.md](https://stitch.withgoogle.com/docs/design-md/overview), this repository publishes the spec for "SLIDE.md" — a design-spec format built specifically for slides — along with Claude Code skills that generate it automatically.

---

## What is SLIDE.md?

Every time you ask an AI tool (Claude Design, NotebookLM, Google Slides, etc.) to generate slides, you have to repeat "use this color, this font, this layout." Worse, the design tends to come out different every time you ask, even for the same brief.

**SLIDE.md solves this by letting you define your design instructions once, as a reusable "spec file."**

### Made up of 4 file types

| File | Role |
|---------|------|
| `SLIDE.md` | Design system definition. Defines colors, fonts, spacing, title area, page numbers, etc. |
| `SLIDE-PATTERN-{name}.md` | Layout pattern definition. Defines the structure of the content area (number of columns, element placement). |
| `SLIDE-SCENARIO-{name}.md` | Scenario (outline). Verbalizes what you want to convey in the presentation, organized by agenda item. Serves as input when building `SLIDE-DECK.md` (optional). |
| `SLIDE-DECK.md` | Slide deck spec. Embeds the full design and pattern definitions, plus the slide outline and content templates in one brief. Hand this single file to an AI tool and it generates the slides. |

### Design principles

**Separate design from structure**: `SLIDE.md` centrally manages the brand's look (colors, fonts) and frame (title area, page numbers), while `SLIDE-PATTERN-*.md` defines only the layout structure of the content area. This lets you reuse the same pattern across different design systems.

**Self-contained in one file**: `SLIDE-DECK.md` embeds all design and pattern definitions. You only need to hand a single file to the AI tool.

---

## Format specification

### SLIDE.md

The design system definition file. Defines colors, fonts, layout, and the slide frame together.

```markdown
# SLIDE.md

## Overview
**Reference source:** (website / brand guideline you referenced)
**Matching scenes:** (use cases / occasions this design fits)

## Colors
| Role | Color name | Hex code |
|---|---|---|
| Primary | (name) | #XXXXXX |
| Secondary | (name) | #XXXXXX |
| Background | (name) | #XXXXXX |
| Accent | (name) | #XXXXXX |
| Text | (name) | #XXXXXX |
| Text Sub | (name) | #XXXXXX |
| Text Muted | (name) | #XXXXXX |

**Gradient:** Primary → Secondary → Accent, 90deg (horizontal), etc. / none

## Typography
| Role | Font | Size | Weight |
|---|---|---|---|
| Heading (H1) | (font name) | 48px | Bold |
| Heading (H2) | (font name) | 32px | Bold |
| Body | (font name) | 18px | Regular |
| Caption | (font name) | 14px | Regular |
| Monospace | (font name / none) | - | - |

## Layout
- **Slide size:** 16:9 (1920 × 1080 px)
- **Padding (vertical):** (px)
- **Padding (horizontal):** (px)
- **Text alignment:** left

## Slide Frame
(Definitions for title area, page number, brand footer, background accent, section bar)

## Component Style
(Definitions for card corner radius/shadow, bullet markers, number style)

## Do / Don't
(List of what to do and what to avoid in this design)
```

### SLIDE-PATTERN-{name}.md

The layout pattern definition file. Defines only the structure of the content area — no colors or fonts.

```markdown
# SLIDE-PATTERN-{name}

## Overview
**Pattern name:** (lowercase, hyphen-separated)
**Overview:** (description of this pattern)
**Suited for:** (where to use it)

## Structure
(The content area's layout structure, described in YAML)

## Elements
(Role and recommended character count for each UI element)

## Usage Guide
(Example prompt for the AI and notes)
```

### SLIDE-SCENARIO-{name}.md (optional)

The definition file for the slide's content (scenario). Created through a back-and-forth with `slide-scenario-creator`. When `slide-deck-builder` finds this file, it skips the brief interview and builds the deck spec directly from the scenario.

```markdown
# SLIDE-SCENARIO-{name}

## Brief
(Title, audience, purpose, goal, structure type, slide count)

## Storyline
(The overall story of the deck, in 2-4 sentences)

## Agenda
### 1. Agenda item name
(Key message, what you want the audience to understand, evidence, estimated slide count)

## Notes (optional)
(Points needing reinforcement, review feedback history, etc.)
```

### SLIDE-DECK.md

The final spec file handed to the AI tool. Embeds the full contents of `SLIDE.md` and the `SLIDE-PATTERN-*.md` files in use, plus a content template for each slide. Hand this single file to an AI tool and it generates the slides.

```markdown
# SLIDE-DECK-{name}

## Deck Info
(Title, audience, purpose, slide count)

## Design System
(Full contents of SLIDE.md embedded)

## Slide Patterns
(Full contents of the SLIDE-PATTERN-*.md files in use, embedded)

## Slides
### Slide 1 — Cover
(Pattern assignment + content template + instructions for the AI)

### Slide 2 — ...
(Repeat for the number of slides)
```

---

## About the skills in this repository

Since hand-writing the SLIDE.md format is difficult, **we built 4 skills that run on Claude Code.** Just hand over a slide image or presentation content, and the AI generates the SLIDE.md file set automatically.

### What the skills can do

1. **Analyze an existing slide or website's design** and auto-generate an AI-readable design definition file (SLIDE.md)
2. **Extract a slide's layout structure** and generate a reusable pattern definition file (SLIDE-PATTERN-\*.md)
3. **Verbalize a presentation's structure through a back-and-forth** to create a scenario (SLIDE-SCENARIO-\*.md), with support for reviewer-perspective checks and persona setup
4. **Take presentation content as input** to auto-generate a deck spec (SLIDE-DECK.md) that bundles design + pattern + slide outline

Just hand `SLIDE-DECK.md` to an AI tool like Claude Design, and you get design-consistent slides.

### Skill list

| Skill | What it does | Output file |
|--------|-----------|------------|
| `slide-md-creator` | Generates a design system from a slide, image, or website | `SLIDE-md/SLIDE-md-{name}/SLIDE.md` + `sample.html` |
| `slide-pattern-creator` | Analyzes a slide's layout structure and defines a pattern | `SLIDE-PATTERN/SLIDE-PATTERN-{name}/SLIDE-PATTERN-{name}.md` + `.html` |
| `slide-scenario-creator` | Creates and reviews a presentation's structure (scenario) through back-and-forth | `SLIDE-SCENARIO/SLIDE-SCENARIO-{name}.md` (+ `PERSONA/PERSONA-{name}.md`) |
| `slide-deck-builder` | Generates a slide deck spec from presentation content | `SLIDE-DECK/SLIDE-DECK-{name}/SLIDE-DECK-{name}.md` |

---

## How to use

---

### 1. Install the skills and sample files

**Method A: Install as a plugin (recommended, easiest)**

Just run these two commands in Claude Code.

```
/plugin marketplace add sho-ai-magic/slide.md
/plugin install slide-md@slide-md
```

This bundles the 4 skills along with 10 sample design systems and 127 slide patterns. **You don't need to copy the sample files into your project folder** — if the files aren't present in your project, the skills automatically fall back to the samples bundled in the plugin. Future updates can also be applied easily with `/plugin marketplace update slide-md`.

Once done, go straight to **Step 5**.

**Method B: Ask Claude Code (works on any OS)**

Open Claude Code in the project folder you want to work in, and just say:

> "Install the skills from this repository, and copy `docs/SLIDE-md/` and `docs/SLIDE-PATTERN/` into the current project folder: https://github.com/sho-ai-magic/slide.md"

Claude Code will automatically set up:
- The 4 skills, installed into `~/.claude/skills/`
- `docs/SLIDE-md/` copied into the project folder (10 sample design systems)
- `docs/SLIDE-PATTERN/` copied into the project folder (127 slide patterns)

Once done, go straight to **Step 5**.

**Method C: Download the ZIP from GitHub**

Steps for those less comfortable with command-line operations.

1. Open the [repository page](https://github.com/sho-ai-magic/slide.md), then download and unzip via "Code" → "Download ZIP".
2. Copy the 4 folders inside `skills/` in the unzipped folder (`slide-md-creator`, `slide-pattern-creator`, `slide-scenario-creator`, `slide-deck-builder`) to:
   - **Mac:** `/Users/(your username)/.claude/skills/`
   - **Windows:** `C:\Users\(your username)\.claude\skills\`
3. Copy `docs/SLIDE-md/` and `docs/SLIDE-PATTERN/` from the unzipped folder directly into the project folder you want to use.

---

### 2. Build a design system (if you want to customize)

Step 1 already set up 10 sample design systems. If you're fine using one as-is, skip to **Step 5**.

If you want an original design system that matches your own brand or existing slides, say this to Claude Code:

> "Create a design system for this slide" (attach an image, website, or PowerPoint)

This generates `SLIDE-md/SLIDE-md-{name}/SLIDE.md` and a `sample.html` for review.

**About sample.html:** A 6-page HTML slide deck for checking what the generated design system actually looks like. Page 1 is a spec overview (color palette, typography list); pages 2-6 render layouts for cover, section title, bullet list, data, and summary, using the colors, fonts, and spacing defined in SLIDE.md. Just open it in a browser to check.

> **Note:** This sample.html is a simple preview generated by Claude Code. Its implementation stays basic. The actual design quality of the generated slides depends on the AI tool (e.g. Claude Design) that you hand SLIDE-DECK.md to.

---

### 3. Add slide patterns (if you want to customize)

Step 1 already set up 127 slide patterns. If you're fine using them as-is, skip to **Step 5**.

Use this skill when you want to add a new layout or pattern you like.

> "Extract the slide pattern" (attach a slide image)

This generates `SLIDE-PATTERN/SLIDE-PATTERN-{name}/SLIDE-PATTERN-{name}.md` and a skeleton HTML (grayscale).

**About the skeleton HTML:** An HTML file for checking a pattern's layout structure (area divisions, element placement). It's rendered in grayscale, deliberately stripped of color, fonts, and decoration. This is by design — so the pattern can be combined with any design system, showing only structure. Applying actual colors and fonts to the slide is SLIDE.md's job.

---

### 4. Build a presentation scenario (optional)

> **This step is optional.** If you already have material where "what to convey" is settled, skip this step and go to **Step 5**.

Use this skill when what you want to convey isn't settled yet, or is hard to put into words.

> "Create a slide scenario"

Through a back-and-forth with Claude, this verbalizes "what you want to convey" by agenda item and generates `SLIDE-SCENARIO-{name}.md`. It also supports reviewer-perspective scenario checks (e.g. from a boss or client's viewpoint) and persona setup for whoever you're presenting to.

If a scenario exists, the next step, Step 5 (`slide-deck-builder`), skips its interview and builds the deck spec directly from the scenario's content.

---

### 5. Build the presentation deck spec

> "Build the presentation deck spec"

If a scenario from Step 4 exists, it's loaded; otherwise you'll be interviewed for a brief (title, audience, purpose, slide count). It then takes in the presentation content and generates SLIDE-DECK.md. Hand this single file to an AI tool and it generates the slides.

When patterns are assigned, `SLIDE-DECK/pattern-preview.html` is auto-generated. Open it in a browser to see thumbnails of the layout assigned to each slide plus alternative candidates, and give swap instructions like "Slide 3 → candidate B" while looking at them.

---

### 6. Generate the slides with an AI tool

> **Note: slide generation is not done in Claude Code — it's done with a separate AI tool.**

`SLIDE-DECK.md` can be handed to any AI tool (NotebookLM, ChatGPT, Gemini, etc.), but **for Claude Code users, [Claude Design](https://claude.ai/design) is recommended.** It specializes in generating visual designs and produces high-quality slides.

**Steps to generate slides with Claude Design:**

1. Open [claude.ai/design](https://claude.ai/design) (also accessible from the "Claude Design" menu in the top-left of [claude.ai](https://claude.ai)).
2. Use the attach button in the chat input to upload the **`SLIDE-DECK-{name}.md`** generated in Step 5.
3. Say the following:

   > "Generate slides following the attached SLIDE-DECK.md"

4. Claude Design reads the design system and pattern definitions and generates the slides.

## Workflow

```
Step 1: Install the skills + sample files (once, initially)
Step 2: slide-md-creator  → SLIDE.md (when building an original design system)
Step 3: slide-pattern-creator → SLIDE-PATTERN-*.md (when adding original patterns)
Step 4: slide-scenario-creator → SLIDE-SCENARIO.md (when building the structure through back-and-forth, optional)
Step 5: slide-deck-builder → SLIDE-DECK.md (generate the deck spec per presentation)
Step 6: Hand SLIDE-DECK.md to Claude Design or another AI tool → slides done
```

---

## Pattern gallery

Browse all 127 layout patterns in your browser.

**→ <a href="https://sho-ai-magic.github.io/slide.md/" target="_blank">https://sho-ai-magic.github.io/slide.md/</a>**

![SLIDE-PATTERN gallery 1](docs/Gallery01.png)
![SLIDE-PATTERN gallery 2](docs/Gallery02.png)

## Generated samples

### Sample 1: SLIDE.md skill introduction

An example of slides generated using this skill package ([full PDF here](examples/output/SLIDE.md%20スキル紹介.pdf)).

![Slide sample 1](docs/slide-sample-01.png)
![Slide sample 2](docs/slide-sample-02.png)
![Slide sample 3](docs/slide-sample-03.png)

---

### Sample 2: SLIDE.md — a design system built for the AI era

An example of slides generated with Claude Design, based on this skill package ([full PDF here](<examples/output/AI時代のスライド専用デザインシステム SLIDE.md.pdf>)).

![Slide sample 2-1](docs/slide-sample2-01.png)
![Slide sample 2-2](docs/slide-sample2-02.png)
![Slide sample 2-3](docs/slide-sample2-03.png)

## Sample files

**Design system samples (`docs/SLIDE-md/`):**

| Folder | Description |
|---------|------|
| `SLIDE-md-anthropic/` | Design system generated with reference to Anthropic's brand colors |
| `SLIDE-md-blue-simple-diagram/` | Simple, diagram-oriented design system based on blue (for education/training) |
| `SLIDE-md-blue-teal-recruitment/` | Blue × teal based. For recruiting / corporate branding |
| `SLIDE-md-corporate-red/` | Corporate red based. For earnings briefings / IR materials |
| `SLIDE-md-digital/` | Design system generated with reference to Japan's Digital Agency Design System (DADS) |
| `SLIDE-md-gemini-color-system/` | Design system generated with reference to Google Gemini's color system |
| `SLIDE-md-golden-yellow/` | Golden yellow based. For B2B business presentations / internal sharing |
| `SLIDE-md-green-blue-business/` | Business-oriented design system based on green and blue (for sales / service proposals) |
| `SLIDE-md-MintGreen/` | Soft-impression design system based on mint green |
| `SLIDE-md-sky-corporate/` | Sky blue based. For integrated reports / annual reports |

Each folder contains `SLIDE.md` (design definition) and `sample.html` (a 6-page HTML slide deck for review). After install, these are saved under `SLIDE-md/` in your project folder.

If you only want a specific design system, you can also click a card in the [gallery](https://sho-ai-magic.github.io/slide.md/), copy the file contents from the "SLIDE.md" tab, and save it as `SLIDE-md/SLIDE-md-{name}/SLIDE.md` in your project folder.

- `examples/output/SLIDE.md スキル紹介.pdf` — A full example presentation introducing this skill package
- `docs/SLIDE-PATTERN/` — HTML files for all 127 layout patterns (referenced from the gallery)

## Acknowledgments

- **[skanehira](https://github.com/skanehira)** — Published [slide-plugin](https://github.com/skanehira/slide-plugin), which reworked this repository into a Claude Code plugin ahead of us. This repository's plugin support and its "fall back to bundled files" mechanism were inspired by that fork. Thank you!

## License

MIT
