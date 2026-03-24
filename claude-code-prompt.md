# Winnie the Pooh Audiobook Player — Build Spec

Build a single-file `index.html` audiobook player for mobile (primarily Android phone). The page will be hosted on GitHub Pages.

## Audio Source Data

Use the chapter data from `audiobook-config.json` (attached). All audio files and the cover image are hosted at `https://matthewbucklebury.github.io/winnie-the-pooh-audiobook/`.

## Page Basics

- **Page title (tab/browser):** Winnie the Pooh
- **Favicon:** `https://matthewbucklebury.github.io/winnie-the-pooh-audiobook/Favicon.png`
- **Background:** White (`#FFFFFF`)
- **PWA / Add to Home Screen:** Include a `manifest.json` with `"display": "standalone"`, the app name "Winnie the Pooh", theme colour forest green, background white, and icon pointing to `Favicon.png` at 512x512. Add the appropriate `<link rel="manifest">` and `<meta name="apple-mobile-web-app-capable">` / `<meta name="mobile-web-app-capable">` tags so the site can be saved as a home screen icon on Android (and iOS as a bonus) and opens without the browser chrome.

## Design

- **Colour scheme:** Forest green (`#2D5F2D` or similar) as the primary accent colour for the player controls, buttons, and text. The cover art image has an aged cream tone — the green should complement that. Use white for the page background and cream/off-white for any card or container backgrounds if needed.
- **Layout (mobile-first):** The page should be designed for a phone screen. Centre everything vertically and horizontally. No horizontal scrolling. The layout from top to bottom:
  1. **Cover art** — the Favicon.png image, displayed prominently, roughly 60-70% of screen width, with rounded corners and a subtle shadow.
  2. **Title** — "Winnie the Pooh" in a warm serif font (use Google Fonts — something like Playfair Display, Libre Baskerville, or similar storybook feel).
  3. **Dedication** — "To my beautiful wife, I hope this audiobook brings you peace and happiness." in italic, smaller text, forest green colour.
  4. **Current chapter display** — Show the chapter number and title of the currently playing/selected chapter. e.g. "Chapter 3: In which Pooh and Piglet go Hunting and nearly catch a Woozle". Use a readable but compact font size since some titles are long.
  5. **Progress bar** — A seek bar showing playback progress for the current chapter. Show elapsed time and total duration either side. Tappable/draggable on mobile.
  6. **Playback controls** — A single row, centred:
     - Previous chapter (skip back)
     - Rewind 15 seconds
     - Play / Pause (larger, central button)
     - Forward 15 seconds
     - Next chapter (skip forward)
  7. **Volume control** — A horizontal volume slider below the playback controls.
  8. **Repeat toggle** — A button that toggles between "Repeat All" (loops back to chapter 1 after chapter 10) and "Play Once" (stops after chapter 10). Default state: **Repeat All on**. Visually indicate which mode is active (e.g. the repeat icon is highlighted/green when on, greyed when off).

- **Icons:** Use inline SVGs or a lightweight icon approach (no external icon library dependencies). Clean, simple icons for play, pause, skip, rewind, forward, volume, repeat.
- **Typography:** Import one serif Google Font for the title and dedication. Use system sans-serif for chapter titles and controls to keep it clean and fast.

## Playback Behaviour

- **Auto-advance:** When a chapter ends, automatically start the next chapter.
- **Repeat All (default):** After chapter 10 finishes, loop back to chapter 1 and continue playing.
- **Play Once mode:** After chapter 10 finishes, stop playback.
- **Background playback:** Use the **Media Session API** so that:
  - Playback continues when the phone screen locks or the browser is in the background.
  - The phone's lock screen / notification shade shows media controls (play/pause, skip) with the chapter title and cover art.
  - Hardware media buttons (headphone controls, car bluetooth) work for play/pause/skip.
- **Remember position:** Use `localStorage` to save the current chapter index and playback position. When the page is reopened, resume from where she left off. Show a subtle "Resuming Chapter X" message if resuming.

## Progressive Web App (PWA)

Generate a separate `manifest.json` file with:
```json
{
  "name": "Winnie the Pooh",
  "short_name": "Pooh",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#FFFFFF",
  "theme_color": "#2D5F2D",
  "icons": [
    {
      "src": "Favicon.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

Do NOT include a service worker for offline caching (the audio files are too large and hosted externally). The manifest is just for the home screen icon and standalone display.

## Technical Constraints

- **Single HTML file** for the player (`index.html`), plus the separate `manifest.json`. No build tools, no npm, no frameworks. Vanilla HTML, CSS, and JavaScript only.
- **All CSS and JS inline** within the HTML file.
- **Mobile-first responsive:** Should look good on a phone screen (360-420px wide). Fine if it also works on desktop but phone is the priority.
- **No external JS dependencies.** Google Fonts CSS import is fine.
- **Audio format:** The files are `.mp3`. Use the HTML5 `<audio>` element.
- **CORS:** The audio files are on the same GitHub Pages domain as the player, so no CORS issues.

## Output

Produce two files:
1. `index.html` — the complete audiobook player
2. `manifest.json` — the PWA manifest

Both files should be ready to commit directly to the root of the GitHub repo at `https://github.com/matthewbucklebury/winnie-the-pooh-audiobook`.
