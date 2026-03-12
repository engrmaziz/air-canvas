<h1 align="center">
  ✋ Air Canvas
</h1>

<p align="center">
  <em>Draw in the air with your bare hands — no stylus, no mouse, just gestures.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5"/>
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript"/>
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3"/>
  <img src="https://img.shields.io/badge/MediaPipe-00897B?style=for-the-badge&logo=google&logoColor=white" alt="MediaPipe"/>
  <img src="https://img.shields.io/badge/Canvas_API-FF6F00?style=for-the-badge&logo=html5&logoColor=white" alt="Canvas API"/>
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="MIT License"/>
  <img src="https://img.shields.io/github/stars/engrmaziz/air-canvas?style=for-the-badge" alt="GitHub Stars"/>
</p>

---

## 🌟 Project Overview

**Air Canvas** is a real-time, gesture-driven drawing application that runs entirely in your browser. Point your finger at the screen and paint — no physical contact with any device required.

Powered by **Google MediaPipe Hands** for sub-millisecond hand-landmark detection and the browser's native **Canvas API** for rendering, Air Canvas translates the position and shape of your hand into drawing commands at up to 60 FPS.

> **Problem it solves:** Traditional digital drawing requires a physical input device. Air Canvas removes that barrier, enabling touchless, hands-free creative expression — useful for accessibility, interactive installations, classroom demonstrations, and just plain fun.

---

## ✨ Features

| Feature | Description |
|---|---|
| ☝️ **Gesture-based drawing** | Raise your index finger to draw smooth, anti-aliased strokes |
| ✌️ **Gesture eraser** | Raise index + middle fingers to erase paint under your fingertip |
| 🤏 **Color cycling** | Pinch thumb and index finger together to cycle through 7 vivid colors |
| 🖖 **Dynamic brush size** | Raise 3 fingers and spread/close them to resize your brush in real-time |
| 🖐️ **Clear canvas** | Hold an open hand steady for 2 seconds to wipe the canvas clean |
| ✊ **Pen lift** | Make a fist to move your hand freely without drawing |
| 🎨 **7-color palette** | Cyan · Magenta · Yellow · Green · Orange · Purple · White |
| 💡 **Glowing neon strokes** | GPU-accelerated shadow blur creates a vibrant neon aesthetic |
| 📷 **Live camera preview** | Mirrored webcam thumbnail shows exactly what the model sees |
| 📊 **Real-time stats** | Live FPS counter and stroke count displayed on screen |
| 🖥️ **Zero installation** | Pure browser app — open `index.html` and go |

---

## 🎬 Demo / Preview

> **Screenshot / GIF placeholder** — add your demo media here.

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│   ✋  AIR CANVAS                                    │
│   ──────────────────────────────────────────────   │
│                                                     │
│   [ Gesture panel ]    [ Drawing area ]   [ Cam ] │
│                                                     │
│   ☝️  DRAW             ~~~~ neon strokes ~~~~       │
│   ✌️  ERASE                                         │
│   🤏  COLOR                                         │
│   🖖  BRUSH SIZE                                    │
│   🖐️  CLEAR                                         │
│   ✊  PEN LIFT                                      │
│                                                     │
│   [ Color palette ── ● ● ● ● ● ● ● ]              │
└─────────────────────────────────────────────────────┘
```

To add your own screenshot, place an image in the repo root and update this section:

```markdown
![Air Canvas Demo](demo.gif)
```

---

## ⚙️ How It Works

```
Webcam Feed
    │
    ▼
MediaPipe Hands  ──►  21 hand landmarks (x, y, z) per frame
    │
    ▼
Gesture Classifier
    │   index up only?         → DRAW
    │   index + middle up?     → ERASE
    │   thumb-index pinch?     → COLOR
    │   3 fingers spread?      → SIZE
    │   all 4 fingers up?      → CLEAR (hold 2 s)
    │   fist?                  → LIFT
    ▼
Canvas Renderer (HTML5 Canvas API)
    │   source-over blending   → draw glowing strokes
    │   destination-out        → erase pixels
    │   smoothPoint() buffer   → 4-frame trail average for smooth lines
    ▼
Display @ up to 60 FPS  (draw-canvas + cursor-canvas overlay)
```

### Key implementation details

- **Mirrored coordinates** — the raw landmark `x` is flipped (`1 - lm.x`) so movement on screen matches the mirrored video feed.
- **Smoothing** — a 4-frame rolling average removes jitter from landmark predictions.
- **Glow effect** — `ctx.shadowBlur` is set to `brushSize × 2.5` before each stroke for the neon look.
- **Eraser** — switches the composite operation to `destination-out` and fills a circle to punch transparent holes.
- **Hold-to-clear** — a 2-second progress ring (`SVG stroke-dashoffset`) prevents accidental canvas wipes.

---

## 🛠️ Tech Stack

| Technology | Role |
|---|---|
| **HTML5 / CSS3** | Single-file UI structure and styling |
| **Vanilla JavaScript** | Application logic, gesture classification, rendering |
| **[MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)** | Real-time 21-point hand landmark detection (runs fully in-browser via WebAssembly) |
| **WebRTC (`getUserMedia`)** | Webcam capture |
| **HTML5 Canvas API** | 2D drawing, erasing, compositing |
| **Web Fonts** | Bebas Neue + Share Tech Mono for the HUD aesthetic |

> No server, no Python, no build step — everything runs client-side.

---

## 🚀 Installation

### Option 1 — Open directly (simplest)

```bash
# Clone the repository
git clone https://github.com/engrmaziz/air-canvas.git

# Open in your browser
cd air-canvas
open index.html          # macOS
xdg-open index.html      # Linux
start index.html         # Windows
```

> ⚠️ Some browsers restrict `getUserMedia` on `file://` URLs. If the camera does not start, use Option 2.

### Option 2 — Serve over localhost

```bash
# Using Python (no install needed)
cd air-canvas
python3 -m http.server 8080
# Then open http://localhost:8080 in your browser
```

```bash
# Or using Node.js / npx
npx serve .
# Then open the URL shown in the terminal
```

### Option 3 — Deploy to GitHub Pages / Netlify / Vercel

The app will is live at `https://engrmaziz.github.io/air-canvas/`.

---

## 🖐️ Usage

1. **Allow camera access** when the browser prompts.
2. Click **▶ LAUNCH APP** on the start screen.
3. Hold your hand up in front of the webcam — the gesture panel on the left will highlight the detected mode.
4. Use the gestures below to interact:

| Gesture | Hand Shape | Action |
|---|---|---|
| ☝️ Draw | Index finger only | Paint a neon stroke |
| ✌️ Erase | Index + middle finger | Erase pixels under fingertip |
| 🤏 Color | Pinch thumb + index | Cycle to next color |
| 🖖 Brush Size | 3 fingers extended, spread/close | Increase / decrease brush |
| 🖐️ Clear | Open hand (all 4 fingers) | Hold 2 seconds to wipe canvas |
| ✊ Pen Lift | Fist | Move hand without drawing |

**Tips:**
- Keep your hand 30–60 cm from the webcam for best tracking.
- Good, even lighting dramatically improves detection accuracy.
- Only the **first detected hand** is tracked; keep other hands out of frame.

---

## 📁 Project Structure

```
air-canvas/
├── index.html      # Complete application (HTML + CSS + JS in one file)
└── README.md       # This file
```

The entire application lives in a single `index.html` file, which contains:

```
index.html
├── <head>
│   ├── MediaPipe Hands CDN script
│   ├── Camera Utils CDN script
│   └── <style>  — all CSS (dark HUD theme)
└── <body>
    ├── #draw-canvas       — persistent drawing surface
    ├── #cursor-canvas     — fingertip cursor overlay
    ├── #cam-container     — live webcam thumbnail
    ├── #mode-display      — current gesture label (top center)
    ├── #gesture-panel     — gesture reference card (left)
    ├── #palette           — color swatches (bottom center)
    ├── #size-indicator    — brush size preview (bottom right)
    ├── #hold-ring         — SVG clear-progress ring
    ├── #stats             — FPS + stroke counter (top right)
    ├── #start-overlay     — splash / permission screen
    └── <script>           — all application JavaScript
```

---

## 🔭 Future Improvements

- [ ] **Shape recognition** — snap freehand strokes to circles, rectangles, lines
- [ ] **Undo / Redo** — two-finger swipe left/right to step through history
- [ ] **Save artwork** — fist + hold to export canvas as PNG
- [ ] **Multi-hand support** — use both hands simultaneously
- [ ] **AI-assisted drawing** — suggest and auto-complete shapes using a lightweight on-device model
- [ ] **Mobile / tablet support** — adapt to front-facing cameras on phones
- [ ] **Collaborative canvas** — real-time multi-user drawing via WebSockets
- [ ] **Custom color picker** — gesture-based HSL selector
- [ ] **Accessibility mode** — larger targets and high-contrast palette

---

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

1. **Fork** the repository on GitHub.
2. **Clone** your fork:
   ```bash
   git clone https://github.com/<your-username>/air-canvas.git
   ```
3. **Create a feature branch**:
   ```bash
   git checkout -b feature/my-awesome-feature
   ```
4. **Make your changes** to `index.html`.
5. **Test** your changes in a browser (see [Installation](#-installation)).
6. **Commit** with a descriptive message:
   ```bash
   git commit -m "feat: add undo gesture support"
   ```
7. **Push** and open a **Pull Request** against `main`.

Please keep pull requests focused on a single feature or bug fix. For larger changes, open an issue first to discuss the approach.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

```
MIT License  ©  Musharraf Aziz
```

---

## 👤 Author

**Musharraf Aziz**

- GitHub: [@engrmaziz](https://github.com/engrmaziz)
- Project: [github.com/engrmaziz/air-canvas](https://github.com/engrmaziz/air-canvas)

---

<p align="center">
  Made with ❤️ and a pinch 🤏 of computer vision magic.
</p>
