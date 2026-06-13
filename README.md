# ✦ Writer's Blocks

**A single-file, zero-dependency creative writing front-end for [LM Studio](https://lmstudio.ai).**

Writer's Blocks is a distraction-free workspace for AI-assisted long-form storytelling. Configure your story's world, characters, and voice — then write with your local model. Everything runs in your browser with no installation, no API keys, and no internet connection required after the page loads.

---

## Screenshot

> *A warm parchment or dark-mode writing environment with a collapsible sidebar, page-outlined story canvas, multi-story switcher in the header, resizable prompt panel, and SD-Forge-Neo integration in Settings.*

---

## Features

### ✦ Multi-Story Support
A **story switcher dropdown** in the header lets you manage multiple books or projects without ever leaving the app.

- Create, rename, duplicate, and delete stories from the dropdown
- Switch between stories instantly — the current story saves automatically before switching
- Each story maintains its own sections, sidebar context, characters, genres, and model settings independently
- A **Duplicate** option copies the full story context and sidebar into a new story with a blank canvas — useful for exploring alternate directions from the same setup
- Story list shows word count and last-modified time for each entry ("2h ago", "yesterday")
- Deletion is guarded by a confirmation dialog; the last remaining story cannot be deleted

### ✦ localStorage Autosave
Your work is saved automatically as you write. There is no Save button — everything persists across tab closes, browser restarts, and reboots.

- **Debounced 800ms autosave** — fires 800ms after any change, so rapid keystrokes don't thrash storage
- **Immediate save on exit** — a `beforeunload` listener flushes any pending changes before the tab closes
- **Immediate save on tab switch** — a `visibilitychange` listener catches background tab scenarios
- An amber dot on the story switcher button signals unsaved changes; it clears when the save fires
- A **storage usage indicator** in the dropdown footer shows current usage against the ~5 MB browser limit
- **"Clear All Saved Data"** in Settings wipes the slate with a confirmation guard

> **Where is the data stored?** In your browser's localStorage, keyed to the file path you open the HTML from. It survives browser restarts but lives inside the browser's profile — not in a folder you can browse directly. See the Storage section below for details and recommendations.

### ✦ Story Context Depth Slider
A new slider in Model Settings gives you direct control over how much story text is sent to the model with each generation request. Seven presets replace the previous hardcoded 2,500-character limit.

| Position | Setting |
|---|---|
| 0 | Last section only |
| 1 | ~1K characters |
| 2 | ~2.5K characters *(default — matches previous behavior)* |
| 3 | ~5K characters |
| 4 | ~10K characters |
| 5 | ~20K characters |
| 6 | Full story *(no limit)* |

The current setting is shown as a live badge in the toolbar ("Context: ~5K") so you always know what's being sent without opening Model Settings. The depth setting is saved per story and persists across sessions.

### ✦ SD-Forge-Neo / Automatic1111 Integration
Configure your Stable Diffusion server alongside LM Studio in the **Settings modal**.

- Separate URL/port fields for LM Studio and SD-Forge-Neo
- **Test Connection** for each — shows live status, connected model name (reads `sd_model_checkpoint` from the options endpoint for SD, and the loaded model ID from LM Studio)
- Dual **status lights** in the header — LM and SD — each independently colored (green = connected, red = error, amber pulsing = testing)
- Integration URLs are stored as **global preferences** separate from per-story data, so they persist across story switches

### ✦ appState Architecture
The app was fully refactored to use a single `appState` object as the source of truth. Every function reads from and writes to this object rather than scraping the DOM.

- DOM inputs are bound to state via `bindStateInputs()` — each `[data-state]` element writes back on `input` or `change`
- `syncStateToDom()` pushes the current state into all DOM elements — used after story switches and context imports
- Characters are a first-class `characters[]` array in state, not DOM card elements being parsed
- Genres are a `genres[]` string array in state, not checkbox reads
- System prompt and user prompt builders are **pure functions** of `appState` — no DOM reading in the generation path

### Story Setup (Sidebar)
- **Story Title** — displayed as a heading in the story canvas; used in all exported filenames
- **Genre** — multi-select checkbox dropdown with 34 options across Fiction, Speculative, and Other groups, including Erotic and Taboo; selected genres shown as removable amber tag chips
- **Narrator Persona** — define the LLM's authorial voice and role
- **Background** — world-building lore, history, rules, and tone
- **Setting** — time period and location, plus a scene description
- **Characters** — add as many as needed; each card has a name and freeform description
- **Writing Guidelines** — style rules, point of view, and narrative tense
- **Model Settings** — Temperature, Max Tokens, Top-P, Repetition Penalty, and Story Context Depth; all panels collapsible

### Story Generation
- **Streaming output** — text streams live as the model writes with an animated cursor
- **Page outline** — generated text appears inside a centered, lightly-bordered content box styled like a page
- **Two writing modes** — Continue (builds on existing story) · Rewrite (reworks a passage)
- **Stop generation** — interrupt mid-stream; partial output is automatically saved as a section
- **Auto-scroll** — canvas scrolls to new content as it streams in

### Section-Based Story Management
Each LLM response is saved as an independent section. Hover any section to reveal its action bar.

| Action | What it does |
|---|---|
| **Copy** | Copies section text to clipboard |
| **TXT** | Downloads the section as a timestamped plain-text file |
| **Edit** | Replaces section with an editable textarea; supports Ctrl/Cmd+S and Escape |
| **Save** | Commits edits back to state and re-renders |
| **Delete** | Removes that section; other sections are unaffected |

- **Clear All** — wipes all sections for the current story (context and settings are preserved)
- **Live word count** in the toolbar

### Export & Import

| Action | Button | Description |
|---|---|---|
| **Export RTF** | Green — header | Full story as RTF: Times New Roman 12pt, double-spaced, `* * *` section breaks, title heading. Opens in Word, LibreOffice, Pages, WordPad. Timestamped filename. |
| **Save Context** | Indigo — header | Exports all sidebar fields to a `.json` file. Timestamped filename. Does not include story sections — context only. |
| **Load Context** | Amber outline — header | Imports a context JSON file **as a new story** — never overwrites your current work. |
| **TXT** *(per section)* | Hover divider | Downloads that section as a timestamped plain-text file. |

### Appearance
- **Dark / Light theme** — toggle in the header; persists across sessions as a global preference
- Smooth 0.35s CSS transitions on all theme-aware elements

---

## Getting Started

### Prerequisites
- [LM Studio](https://lmstudio.ai) installed, running, with a model loaded and the local server enabled (default port `1234`)
- A modern web browser (Chrome, Edge, Safari, Firefox)

### Usage

1. **Download** `writers-blocks.html` from this repository
2. **Open** it in your browser — double-click, or `File → Open`
3. **Start LM Studio**, load a model, and enable the local server
4. In Writer's Blocks, click **Test** to verify the connection
5. Fill in the sidebar: title, genre, persona, background, setting, characters
6. Type a direction in the prompt box and press **Enter** (or **✦ Generate**)

Your work saves automatically. Come back tomorrow and it will be exactly where you left it.

---

## Interface Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│  ✦ Writer's Blocks  [▾ Story Title ●]  [🌙]  [⚙ Settings]             │
│  [Export RTF] [Save Context] [Load Context]   LM ● SD ● [Test]        │
├──────────────────────┬──────────────────────────────────────────────────┤
│                      │  ┌──────────────────────────────────────────┐   │
│  ▾ Story Title       │  │  Story Title                             │   │
│  ▾ Narrator Persona  │  │  ──────────────────────                  │   │
│  ▾ Background        │  │                                          │   │
│  ▾ Setting           │  │  Generated prose appears here, flowing   │   │
│  ▾ Characters        │  │  continuously downward as you write…     │   │
│  ▾ Writing Guidelines│  │                                          │   │
│  ▸ Model Settings    │  │  ─── ✦ [Copy] [TXT] [Edit] [Delete] ─── │   │
│                      │  └──────────────────────────────────────────┘   │
│                      ├─────────────────── ▲ drag ──────────────────────┤
│                      │  [Clear All]    Context: ~5K        1,024 words │
│                      ├──────────────────────────────────────────────────┤
│                      │  Text Generation                                 │
│                      │  [Continue] [Rewrite]          MODEL ▾  [↺]    │
│                      │  ┌──────────────────────────┐  [✦ Generate]    │
│                      │  │ Prompt…                  │  [■ Stop    ]    │
│                      │  └──────────────────────────┘                  │
└──────────────────────┴──────────────────────────────────────────────────┘
```

---

## Multi-Story Workflow

```
Header story switcher
         │
         ▼
┌─────────────────────────────┐
│  My Stories          [+ New]│
│ ─────────────────────────── │
│ ▶ The Atlas of Lost Cities  │  ← active story (amber)
│   12,847 words · just now   │
│ ─────────────────────────── │
│   The Cartographer's War    │
│   3,204 words · 2h ago      │
│ ─────────────────────────── │
│   Untitled Story            │
│   0 words · yesterday       │
│ ─────────────────────────── │
│  Storage: 48 KB / ~5 MB ▓░░ │
└─────────────────────────────┘

Each story row: [name + meta] [✎ rename] [⧉ duplicate] [🗑 delete]
```

---

## Story Context Depth

The slider controls how much of your story's existing text is included in each generation request. This is the primary quality lever for long stories.

```
Slider position → What gets sent to the model
─────────────────────────────────────────────
0  Last section    → Only the most recent generated section
1  ~1K chars       → Last ~250 words
2  ~2.5K chars     → Last ~625 words  (default)
3  ~5K chars       → Last ~1,250 words
4  ~10K chars      → Last ~2,500 words
5  ~20K chars      → Last ~5,000 words
6  Full story      → Everything (careful with small context windows)
```

The current setting appears as a live badge in the toolbar and is saved per story.

---

## Storage Details

### How it works
Data is stored in your **browser's localStorage** — inside the browser's internal profile, not in a folder you can browse to in Explorer. It's keyed to the exact file path you open the HTML from.

### Physical location (read-only reference)
**Chrome:** `C:\Users\YourName\AppData\Local\Google\Chrome\User Data\Default\Local Storage\leveldb\`
**Edge:** `C:\Users\YourName\AppData\Local\Microsoft\Edge\User Data\Default\Local Storage\leveldb\`

These are binary LevelDB databases — not directly editable.

### Practical considerations

| Scenario | Behaviour |
|---|---|
| Browser restart / reboot | ✓ Data survives |
| Move the HTML file to a new folder | ⚠ App sees it as a new origin — old data still exists under the old path |
| Clear browsing data in browser settings | ✗ localStorage is wiped |
| Open in a different browser | ✗ Each browser has its own isolated storage |
| Private / Incognito mode | ✗ Data is wiped when the window closes |
| Two tabs open simultaneously | ⚠ Last write wins |

### Recommendations
- Use **Save Context** (JSON) and **Export RTF** as your real backups — these are portable files you own
- After a good session, export a context JSON and store it in Documents or a cloud drive
- For truly path-independent storage, serve the file from a local web server: `python -m http.server 8000` then open `http://localhost:8000/writers-blocks.html`

---

## Context File Format

```json
{
  "_version": "2.0",
  "_app": "Writer's Blocks",
  "_exported": "2025-10-14T18:32:00.000Z",
  "title": "The Atlas of Lost Cities",
  "genres": ["Gothic", "Magical Realism"],
  "persona": "You are a lyrical literary fiction writer…",
  "background": "In this world, cartography is a sacred profession…",
  "settingTime": "Turn of the 20th century, Central Europe",
  "settingPlace": "A cramped cartographer's studio above a cobbled square…",
  "characters": [
    { "name": "Mira Voss", "description": "A journeyman cartographer…" },
    { "name": "Director Hal", "description": "Head of the Imperial Survey…" }
  ],
  "guidelines": "• Show, don't tell\n• Ground magic in sensory detail…",
  "pov": "Third person limited",
  "tense": "Past tense",
  "lmUrl": "http://localhost:1234",
  "sdUrl": "http://localhost:7860",
  "contextDepthIdx": 3
}
```

Loading a context file **always creates a new story** — it never overwrites your current work. The `v1.0` single-genre string format (from earlier versions) is still supported on import.

---

## RTF Export

Generated `.rtf` files are formatted for manuscript editing:

- **Times New Roman, 12pt**
- **Double-spaced** lines
- **First-line indents** on paragraphs after the opening
- Story title as a centred heading
- `* * *` ornamental breaks between sections

Opens natively in Microsoft Word, LibreOffice Writer, Apple Pages, and Windows WordPad.

---

## Writing Tips

- **Persona is the most powerful setting.** A specific, well-crafted narrator voice shapes output quality more than any other field. "Terse Hemingway-style realist" or "gothic Victorian novelist" produce noticeably different prose than a generic instruction.
- **Use the context depth slider actively.** For short stories, Full Story keeps the model oriented. For long novels, 5K–10K chars is usually the sweet spot — enough context for coherence without burning the whole context window.
- **Duplicate before experimenting.** Before trying a risky narrative direction, duplicate the story. Explore in the copy. Merge back what you like.
- **Stop and keep.** If the model veers off course mid-stream, hit Stop — the partial text is saved as a section. Delete just that section and try a different prompt direction.
- **Export context early.** Once your sidebar is set up, export a context JSON before you start generating. Sidebar context is autosaved, but an export gives you a portable backup you can share or open on another machine.
- **Temperature ~0.85** produces fluent, creative prose. Go higher (1.1–1.3) for more surprising word choices; lower (0.5–0.7) for tighter, more predictable output.
- **Genre tags inform the model.** Adding "Gothic" and "Erotic" as genres sends those terms directly into the system prompt, nudging the model's tone and content without you having to restate it in every prompt.

---

## Technical Notes

- **Single HTML file** — no build step, no framework, no CDN, no dependencies; everything runs in the browser
- **1,651 lines** of HTML/CSS/JS, ~90 functions
- **appState architecture** — one object is the source of truth; DOM renders from it, functions read from it, localStorage persists it
- **localStorage keys** — `wb-global` (theme, URLs), `wb-stories` (registry), `wb-active` (last story), `wb-story-{uuid}` (per-story data)
- **RTF export** — generated from scratch in-browser using RTF 1.x syntax; no library
- **Streaming** — uses the OpenAI-compatible `/v1/chat/completions` streaming endpoint exposed by LM Studio
- **Stop** — uses the browser's `AbortController` API; partial output is preserved
- **SD-Forge-Neo** — uses the A1111-compatible `/sdapi/v1/options` and `/sdapi/v1/txt2img` endpoints
- Tested with LM Studio `0.3.x` and later

---

## Version History

| Version | Key changes |
|---|---|
| v17 | Multi-story support, localStorage autosave, appState refactor, story switcher dropdown, storage indicator, per-story context depth |
| v16 | appState single source of truth, context depth slider, genre array in state, pure system prompt builder |
| v14 | Timestamps on all exports, genre in context export/import, context window budget removed |
| v13 | Resizable prompt panel, genre multi-select (34 options inc. Erotic/Taboo), genre in system prompt |
| v11 | SD-Forge-Neo integration in Settings, dual status lights (LM + SD), Settings modal |
| v9  | Context window budget panel, Brainstorm feature *(later removed)* |
| v8  | Scrolling story canvas (removed book pagination), removed Dialogue/Outline/Brainstorm modes |
| v7  | Edit/Save per section with inline textarea, Ctrl+S shortcut |

---

## Contributing

Issues and pull requests are welcome. The single-file constraint is intentional — features that require a build step or external dependencies won't be accepted, but everything else is fair game.

---

## License

MIT — do whatever you like with it.
