# ✦ Writer's Blocks

**A lightweight, single-file creative writing front-end for [LM Studio](https://lmstudio.ai).**

Writer's Blocks gives you a beautiful, distraction-free workspace for AI-assisted storytelling. Set your narrator's voice, build your world, define your characters, and establish writing guidelines — then generate, refine, and export your story, all from one self-contained HTML file with zero installation required.

---

## Screenshot

> *A parchment or dark-mode writing environment with a collapsible sidebar, a book-page story canvas, and a full header toolbar.*

---

## Features

### Story Setup (Sidebar)
- **Narrator Persona** — Define the LLM's role, voice, and authorial style
- **Background** — Establish world-building lore, rules, history, and tone
- **Setting** — Specify the time, place, and atmosphere of the current scene
- **Characters** — Add as many characters as needed, each with a name and description card; cards can be removed individually
- **Writing Guidelines** — Set freeform style rules, point of view, and narrative tense
- All sidebar panels are collapsible to keep the workspace tidy

### The Book Page
Text generates inside a styled book-page canvas — a parchment-toned card with a left margin rule, faint horizontal guide lines, and a layered drop shadow. The goal is to make the writing feel like it belongs on a page, not a screen.

### Generation
- **Streaming output** — Text streams live as the model writes, with an animated cursor
- **Five writing modes** — Continue · Rewrite · Brainstorm · Dialogue · Outline
- **Stop generation** — Interrupt mid-stream at any time; partial output is automatically saved as a section
- **Model settings** — Tune Temperature, Max Tokens, Top-P, and Repetition Penalty via sliders

### Story Management
- **Section-based editing** — Each LLM response is stored as its own independent section
- **Per-section actions** — Hover any section to reveal three action buttons:
  - **Copy** — copies that section's text to the clipboard
  - **TXT** — downloads that section as a plain-text file
  - **Delete** — removes that section without touching the rest of the story
- **Clear All** — wipes all sections and resets the canvas
- **Live word count** — running total shown in the bottom toolbar

### Export & Import

| Action | Button | Description |
|---|---|---|
| **Export RTF** | Green — header | Saves the full story as an RTF file; opens in Word, LibreOffice, Pages, and WordPad. Generated entirely in the browser — no library or server needed. |
| **Save Context** | Indigo — header | Exports all sidebar fields (persona, background, setting, characters, guidelines, POV, tense) to a `.json` file for later reuse. |
| **Load Context** | Amber outline — header | Imports a previously saved `.json` context file, restoring all fields and rebuilding character cards automatically. |
| **Copy** *(per section)* | Hover divider | Copies that individual section to the clipboard. |
| **TXT** *(per section)* | Hover divider | Downloads that individual section as `section-N.txt`. |

### Appearance
- **Dark / Light theme** — A toggle in the header switches between the warm parchment light theme and a near-black dark theme. All colours, the book page, sidebar, and prompt area transition smoothly. The icon switches between a moon (switch to dark) and sun (switch to light).

### LM Studio Integration
- **Test** — Dedicated button to ping your LM Studio server and verify connectivity; updates the status indicator and refreshes the model list on success
- **Auto-connect** — Automatically attempts to fetch available models on page load
- **Model selector** — Dropdown populated live from your running LM Studio instance, with a manual refresh button
- **Configurable server URL** — Defaults to `http://localhost:1234`; change it to match your setup

---

## Getting Started

### Prerequisites
- [LM Studio](https://lmstudio.ai) installed and running with a model loaded
- The LM Studio local server enabled (default port `1234`)
- A modern web browser (Chrome, Firefox, Edge, Safari)

### Usage

1. **Download** `writers-blocks.html` from this repository
2. **Open** it in your web browser — double-click it, or use `File → Open`
3. **Start LM Studio**, load a model, and enable the local server
4. In Writer's Blocks, click **Test** to verify the connection — the model dropdown will populate automatically
5. Fill in your story setup in the sidebar panels
6. Type a direction in the prompt box and press **Enter** (or click **✦ Generate**)

No npm install. No Python environment. No API key. No internet connection required after the page loads.

---

## Interface Overview

```
┌──────────────────────────────────────────────────────────────────────────┐
│  ✦ Writer's Blocks   [🌙 Dark] | [Export RTF] | [Save Context]          │
│                      [Load Context] | [Server URL] [Test]  ●  Connected  │
├──────────────────────┬───────────────────────────────────────────────────┤
│                      │  ╔═══════════════════════════════════════╗        │
│  ▾ Narrator Persona  │  ║                                       ║        │
│  ▾ Background        │  ║  Story text streams here, inside      ║        │
│  ▾ Setting           │  ║  the book page canvas…                ║        │
│  ▾ Characters        │  ║                                       ║        │
│  ▾ Writing Guide     │  ║  ── ✦ [Copy] [TXT] [Delete] ── ✦ ──  ║        │
│  ▸ Model Settings    │  ║                                       ║        │
│                      │  ║  Each section has its own actions.    ║        │
│                      │  ╚═══════════════════════════════════════╝        │
│                      ├───────────────────────────────────────────────────┤
│                      │  [Clear All]                       1,024 words    │
│                      ├───────────────────────────────────────────────────┤
│                      │  [Continue][Rewrite][Brainstorm][Dialogue]…       │
│                      │  ┌───────────────────────────┐  [✦ Generate]     │
│                      │  │ Prompt input…             │  [■ Stop     ]    │
│                      │  └───────────────────────────┘                   │
└──────────────────────┴───────────────────────────────────────────────────┘
```

---

## Writing Modes

| Mode | What it does |
|---|---|
| **Continue** | Appends a new section, passing the full story context to the model |
| **Rewrite** | Asks the model to rework a passage based on your direction |
| **Brainstorm** | Generates ideas — plot twists, character arcs, world details |
| **Dialogue** | Writes a focused dialogue scene between characters |
| **Outline** | Produces a structured scene or chapter outline |

---

## Context Files

The **Save Context / Load Context** workflow lets you build reusable story bibles — world and character setups you can reload into any new writing session without retyping anything.

A saved context file looks like this:

```json
{
  "_version": "1.0",
  "_app": "Writer's Blocks",
  "_exported": "2025-10-14T18:32:00.000Z",
  "persona": "You are a gothic Victorian novelist…",
  "background": "The city of Dunmorrow sits at the edge of a collapsed empire…",
  "settingTime": "Autumn, 1887 — London",
  "settingPlace": "A fog-drenched alley behind Aldgate Station…",
  "characters": [
    { "name": "Inspector Hazel Voss", "description": "Sharp, sardonic, haunted by a case she never closed…" },
    { "name": "Edmund Cray",          "description": "A disgraced surgeon hiding something behind his courtesy…" }
  ],
  "guidelines": "• Show, don't tell\n• Vary sentence rhythm…",
  "pov": "Third person limited",
  "tense": "Past tense"
}
```

You can hand-edit these files in any text editor — useful for fine-tuning character descriptions or sharing a setup with collaborators.

---

## RTF Export

The exported `.rtf` file uses:
- **Times New Roman, 12pt**, ink-coloured text
- **Double-spaced** lines
- **First-line indents** (0.5 in) on all paragraphs after the opening
- `* * *` ornamental breaks between story sections

It opens without plugins or conversion in Microsoft Word, LibreOffice Writer, Apple Pages, and Windows WordPad. The RTF is generated entirely client-side — no library, no server call.

---

## Themes

The **Dark / Light** toggle in the header switches the entire UI between two fully realised themes:

| Element | Light Theme | Dark Theme |
|---|---|---|
| Background | Warm parchment | Near-black |
| Book page | Cream with subtle rule lines | Dark charcoal |
| Accent colour | Amber gold | Brighter amber |
| Sidebar | Semi-transparent warm white | Semi-transparent dark |
| Text | Deep ink brown | Warm off-white |

All transitions are animated (0.35 s) so the switch feels smooth rather than jarring. The theme is stored in a `data-theme` attribute on the root `<html>` element, cascading instantly to all CSS custom properties.

---

## Tips

- **Persona is the most powerful setting.** A well-crafted narrator persona — "gothic Victorian novelist," "terse Hemingway-style realist," "lyrical magical-realist" — shapes output quality more than any other field.
- **Stop and keep.** If the model veers off course mid-stream, hit **Stop**. The partial text is saved as a section. Delete just that section and try a different prompt direction.
- **Per-section TXT for workshopping.** Download individual sections to paste into your editor or share with a writing group without exporting the whole story.
- **Save your context early.** Once you have a solid world, characters, and guidelines set up, save the context file before you start generating. It's easy to lose sidebar work if you close the tab.
- **Build context gradually.** Use *Continue* mode to grow the story section by section. The model receives the last ~2,500 characters of story text with each generation, so momentum carries forward naturally.
- **Temperature ~0.85** tends to produce fluent, creative prose. Go higher (1.1–1.3) for more surprising word choices; lower (0.5–0.7) for tighter, more controlled output.
- **Export RTF for final editing.** When you're ready for a proper editorial pass, export to RTF and open in Word or LibreOffice — the formatting is clean and ready for revision.
- **Use dark mode for late-night writing.** The dark theme is easy on the eyes and keeps the book-page feel intact.

---

## Technical Notes

- **Single HTML file** — everything runs in the browser; no build step, no framework, no dependencies to install
- **RTF export** is generated from scratch using standard RTF 1.x syntax — no CDN, no library
- **Context JSON** is plain text; the format is stable and human-editable
- **Dark / Light theme** uses CSS custom properties (`--ink`, `--page`, `--bg`, etc.) toggled via a `data-theme` attribute on `<html>`; no JavaScript style manipulation
- **Generation** uses the OpenAI-compatible `/v1/chat/completions` streaming endpoint exposed by LM Studio
- **Stop** uses the browser's `AbortController` API to cancel the in-flight fetch; partial output is preserved as a section
- Tested with LM Studio `0.3.x` and later

---

## Contributing

Issues and pull requests are welcome. This is an intentionally minimal project — feature suggestions that can be implemented without adding external dependencies or a build step are most likely to be accepted.

---

## License

MIT — do whatever you like with it.
