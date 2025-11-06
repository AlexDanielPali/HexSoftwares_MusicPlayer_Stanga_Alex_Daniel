# HexSoftwares Music Player

A small, responsive music player built with Vue 3 and Vite. This project was created as part of the HexSoftwares internship and demonstrates a compact, modern UI with audio playback features (progress, time display, repeat, seek) and a responsive two-column layout for desktop and stacked layout for smaller devices.

## Technologies & Frameworks

- Vue 3 (Single File Components)
- Vite (dev server / build tool)
- Tailwind CSS (loaded via CDN for quick utilities)
- Vanilla CSS for the main visual theme (`src/components/style.css`) — a custom "techy" look with gradients and a visualizer
- PostCSS / Autoprefixer are included as devDependencies (project can be extended with a local Tailwind build)

## What I built

This app implements a compact music player with the following features:

- Playlist panel with clickable tracks and active state
- Now playing panel with album art, title and artist
- Audio playback using the native HTMLAudioElement
- Play / Pause, Previous / Next controls
- Clickable progress bar with seek support
- Current position (minutes:seconds) on the left and total duration on the right
- Repeat toggle (repeat current track when enabled)
- Responsive layout: two-column on wide screens, stacked layout on small screens

## Project structure (important files)

- `src/App.vue` — root app wrapper that mounts the player layout
- `src/components/SongList.vue` — playlist and navigation logic
- `src/components/SongPlayer.vue` — audio player UI, audio element, time/progress/repeat logic
- `src/components/style.css` — main visual theme (colors, layout, responsive rules)
- `src/assets/main.css`, `src/assets/base.css` — general app styles
- `index.html` — includes Tailwind CDN link for utility classes used across small parts of the project

## Data shape

Tracks use a simple object shape used by `SongList` and `SongPlayer`:

```json
{
  "id": 1,
  "name": "Track title",
  "artistName": "Artist",
  "albumName": "Album",
  "year": 2025,
  "src": "https://.../cover.jpg", // cover image URL
  "songSrc": "https://.../song.mp3" // audio URL
}
```

## How the player works (logic)

- The playlist (`SongList.vue`) keeps an array of track objects and the `currentSongIndex`.
- When a user clicks a track, `SongList` updates `currentSongIndex` and the selected song is passed to `SongPlayer` as a prop.
- `SongPlayer` uses a hidden `<audio>` element (`ref="audioPlayer"`) and listens to these events:
  - `loadedmetadata` to read total duration
  - `timeupdate` to update the current playback position
  - `ended` to either repeat the song (if repeat is enabled) or emit `next` to the parent
- The progress bar is clickable; clicking calculates the percentage of the width and sets `audio.currentTime` accordingly.
- Times are displayed in `M:SS` format (minutes:seconds).

## How to run

Install dependencies and start the dev server (PowerShell):

```powershell
npm install
npm run dev
```

Open the app in the browser at the address Vite prints (usually `http://localhost:5173`).

Notes:
- Some tracks in the example playlist use remote audio URLs — if the browser blocks playback due to CORS or autoplay policies, either provide a local audio file or interact with the page (click Play) to allow playback.
- Tailwind is included via CDN in `index.html` for convenience. If you'd prefer to build Tailwind locally for customization, I can add a proper Tailwind/PostCSS setup and move utilities into generated CSS.

## Known limitations & suggestions

- Autoplay: modern browsers often block autoplay with sound; the player attempts to play but may require a user gesture.
- Offline packaging: remote images and audio are loaded at runtime; for a production build consider bundling or hosting assets to ensure availability.
- Accessibility: keyboard controls and ARIA labels can be improved (I can add these if you want).

## Extending the project

- Add persistent state (localStorage) to remember the last played song and volume.
- Add shuffle and queue management.
- Integrate a streaming backend or local file picker to load user songs.

## Thank you

Big thanks to the HexSoftwares team for the opportunity! 

If you want, I can also:
- Convert the UI entirely to Tailwind utilities (requires installing Tailwind locally),
- Add keyboard shortcuts and accessibility improvements, or
- Add unit tests for key logic (time formatting, repeat behavior, seeking).

---

Feel free to tell me which follow-up you prefer and I will implement it next.



[VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Recommended Browser Setup

- Chromium-based browsers (Chrome, Edge, Brave, etc.):
  - [Vue.js devtools](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) 
  - [Turn on Custom Object Formatter in Chrome DevTools](http://bit.ly/object-formatters)
- Firefox:
  - [Vue.js devtools](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
  - [Turn on Custom Object Formatter in Firefox DevTools](https://fxdx.dev/firefox-devtools-custom-object-formatters/)

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
