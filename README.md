# Bachon Ka Magic Whiteboard — ABC Learn Edition

Kids' magic whiteboard with on-draw ABC letter recognition + voice (frontend-only).

Kids draw a letter on the canvas. The app recognises it, shows a big animated letter, and says something like "A. A for Apple." out loud. It also has the usual drawing tools, so it works as a plain whiteboard too.

**Live demo:** https://YOUR-USERNAME.github.io/bachon-ka-magic-whiteboard/

## Features

- **Drawing tools:** Pencil (smoothed strokes), Magic (shape snapping for lines, circles, ovals and rounded rectangles), Eraser, Fill bucket, and Undo.
- **Colors and size:** 12 color swatches and a brush-size slider.
- **ABC recognition:** Draw a letter (A–Z) and the app recognises it automatically. It shows an animated overlay and speaks the letter and its word.
- **Alphabet panel:** Tap any letter to hear it spoken and see it stamped on the canvas.
- **Voice styles:** Professional or Baby voice, using pitch and rate presets.
- **Voice and ABC Learn toggles:** Turn speech or automatic recognition on and off.
- **Final Dekho:** A preview modal with confetti, Save as Image, and Wapis Edit Karo.

## Running locally

Everything runs in the browser from a single file. There is no build step and no backend.

Option 1: open `index.html` directly in your browser.

Option 2: serve the folder with a local server, which is more reliable for speech-voice loading:

```bash
npx serve
```

Then open the address it prints (usually http://localhost:3000).

No API keys are needed. The only network request is the Google Fonts stylesheet. The app works offline once it has loaded.

## How it works

1. **Stroke capture.** Pointer events record each stroke as a list of points. Recognition runs after each stroke finishes.
2. **Normalisation.** The strokes are rendered onto a 64×64 canvas. They are scaled and centred by their bounding box, so letter size and position do not matter.
3. **Template matching.** The normalised drawing is compared against pre-rendered A–Z template canvases, which are built once at startup from the Fredoka font.
4. **F1 score with dilation.** Both images are binarised. Each is dilated by a small radius (3 px) to allow tolerance for small offsets. Coverage (drawn pixels near the template) and precision (template pixels near the drawing) are combined into an F1 score.
5. **Threshold and margin.** The best match is accepted only if its score is at least 0.5 and it beats the second-best match by at least 0.04. Otherwise nothing is announced.
6. **Speech.** The accepted letter is spoken through the Web Speech API. The voice style sets pitch and rate: Professional uses pitch 1.0 and rate 0.95, and Baby uses pitch 1.6 and rate 0.8.

## Browser support

| Browser | Drawing and recognition | Speech (Web Speech API) |
|---|---|---|
| Chrome / Edge | Yes | Yes |
| Safari (macOS / iOS) | Yes | Yes |
| Firefox | Yes | Yes, depending on OS voices |

Voice quality and available voices depend on your operating system and browser. The app picks the best English voice it finds and falls back to the default voice if none is available. If speech is unavailable, the app keeps working silently.

## Roadmap

- Lowercase letter recognition (a–z)
- Number recognition (0–9)
- Multilingual voice (Urdu, Hindi)
- Tracing mode (trace a dotted letter)
- Optional on-device ML model (for example TensorFlow.js) for better recognition of messy handwriting

## Project structure

```
index.html     # The whole app (HTML, CSS and JavaScript)
404.html       # Redirects unknown paths back to the app
.nojekyll      # Tells GitHub Pages to serve files as-is
LICENSE        # MIT license
README.md      # This file
```

## License

MIT License. Copyright (c) 2026 YOUR FULL NAME. See [LICENSE](LICENSE).
