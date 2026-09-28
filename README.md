# UI Constructor — visual web-interface builder

A single self-contained HTML file (`index.html`). Open it in a browser and start working. No build step, no dependencies, no internet required.

[Русская версия: README.ru.md](README.ru.md)

## Why

**The main goal** — quickly sketch what a web interface should look like and hand the mockup to an AI without explaining in words "which button goes where".

The workflow:

1. Draw the interface: drag elements onto the canvas, arrange them, label them, add hand-drawn notes with the pencil if needed.
2. Save the project as **JSON** (exact structure: sizes, positions, texts, colors).
3. Export the canvas as **PNG** (transparent background) or **JPG** (white) — the visual mockup.
4. Send both files to an AI with a prompt like:

> "Build a web page from this mockup. The PNG shows the appearance, the JSON contains exact coordinates, sizes, texts, and colors of all elements. Generate HTML/CSS from this specification."

The AI gets both the picture (what it looks like) and the JSON (what elements, where, how big) — no verbal explanation needed.

## Getting started

Open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari). That's it.

## Features

- **11 built-in elements** in the "Elements" panel on the left: button, text input, label, textarea, radio, checkbox, switch, select, slider, image, panel.
- **Custom PNG elements**: the "➕ Add PNG" button — upload an image (up to 4 MB) and it appears in the "Custom PNG" section, then works like any other element.
- **Editing**: drag to move, resize with handles, rename, text, colors, border radius — in the properties panel on the right. Delete with Del/Backspace, deselect with Esc.
- **Pencil ✏️** — draw on the canvas with a color picker (notes, arrows, annotations). **Eraser 🧽** — erases pencil strokes.
- **Background**: any canvas color, or "⌀" — no background (PNG exports transparent).
- **Saving**: JSON file (download) + autosave to browser localStorage — the project survives page reloads.
- **Canvas export**: PNG (transparent or colored, per settings) and JPG (white or colored). Only the canvas is exported, without the editor panels.

## How to use

1. On the left — the "Elements" panel. Drag an element onto the canvas or click it to add.
2. Click an element on the canvas to select it: properties (name, text, size, colors) open on the right.
3. Drag the corner handles to resize, drag the body to move.
4. Canvas background color and the pencil/eraser tools are in the top toolbar.
5. "Save JSON" and "PNG" / "JPG" are in the top toolbar. Files download to your downloads folder.

## JSON format

```json
{
  "app": "ui-constructor",
  "version": 1,
  "savedAt": "2026-09-28T12:00:00.000Z",
  "name": "My project",
  "canvas": {"w": 800, "h": 600, "bg": "#f5f5f2"},
  "elements": [
    {
      "id": 1,
      "type": "button",
      "name": "Button",
      "x": 40, "y": 40, "w": 120, "h": 40,
      "text": "Button",
      "bg": "#2f6fed", "color": "#ffffff", "radius": 8
    },
    {
      "id": 2,
      "type": "custom-img",
      "name": "Logo",
      "x": 200, "y": 40, "w": 160, "h": 120,
      "src": "data:image/png;base64,…"
    }
  ],
  "customImages": [{"id": 1, "name": "Logo", "src": "data:image/png;base64,…"}],
  "strokes": [{"color": "#e5484d", "width": 4, "points": [[10, 10], [50, 30]]}]
}
```

- `canvas.bg` — `null` when the background is off (transparent PNG).
- `type` — one of: `button`, `input`, `label`, `textarea`, `radio`, `checkbox`, `switch`, `select`, `slider`, `image`, `panel`, `custom-img`.
- `strokes` — pencil strokes (annotations on the mockup); the AI should not implement them in code, they are notes.
- `customImages` — user-uploaded PNGs (data URLs), so `src` in elements isn't duplicated.

## Limitations

- Everything lives in a single file and browser localStorage. JSON with large PNG data URLs may hit the localStorage limit (~5–10 MB) — the browser will show a save error if exceeded.
- One custom PNG — up to 4 MB.
- This is a mockup builder, not a code generator: it describes the interface; the AI (or a human) writes the code from the mockup.

## Testing

The file is covered by e2e tests (headless Chrome via CDP): elements, properties, pencil/eraser, custom PNGs, autosave, PNG/JPG export with pixel verification. Latest runs: 27/27.
