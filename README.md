# SkyRadar

SkyRadar is a browser-based tactical ADS-B and AIS tracker for viewing nearby aircraft and ships on an interactive map. It uses your browser location (or a manually entered position), queries public aircraft data, and presents live traffic with a radar-inspired dashboard, aircraft list, system logs, and map overlays.

The application is currently a static HTML app in `index.html` that loads its UI dependencies from public CDNs, including Tailwind CSS and Leaflet.

## Features

- Live nearby aircraft display using a configurable search radius that is remembered in the same browser.
- Optional AISstream.io WebSocket overlay for vessels inside the same radar radius.
- Interactive Leaflet map with custom aircraft markers and heading-aware styling.
- Browser geolocation with a visual OpenStreetMap picker, place search, Google Maps/OpenStreetMap link parsing, and manual coordinate override support; the last radar center is saved locally for future sessions.
- Sidebar aircraft list and system log views.
- Auto-refresh controls for repeated traffic updates, with adaptive backoff when public ADS-B or CORS pathways are blocked.
- Top-bar visibility toggles for hiding military, emergency, grounded, or airborne aircraft categories from the map and list, with preferences saved between sessions.
- Cockpit view with synthetic runway cues sourced from nearby OpenStreetMap aeroway data when available, plus forward AIS ship cues when vessels are in range.
- Dark tactical HUD-style interface optimized for quick scanning.

## Getting Started

Because SkyRadar is a static web app, no build step is required.

### Option 1: Open directly

Open `index.html` in a modern browser.

> Note: Browser geolocation may require a secure context. If location access does not work from a local file, run a local web server instead.

### Option 2: Run a local server

From the project root, start any simple static file server, for example:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

## Basic Controls

- **Allow Location**: When prompted, allow browser geolocation so SkyRadar can center traffic around your current position.
- **Radius**: Choose a scan radius from 10 to 150 miles.
- **Show Military / Emergency / Grounded / Airborne**: Toggle these top-bar buttons off to de-select those aircraft categories from both the map and aircraft list.
- **Show Panel / Hide Panel**: Toggle the aircraft and system log sidebar.
- **Zoom + / −**: Adjust the map zoom level.
- **Adjust Location**: Search for a place, paste a Google Maps or OpenStreetMap link, click the OpenStreetMap selector map, choose a preset, or enter coordinates manually when you do not want to use browser geolocation or need to scan another area.
- **Auto Updates**: Use the refresh control to keep nearby aircraft data current. SkyRadar uses a 30-second baseline interval and backs off up to 2 minutes after repeated blocked fetches to reduce public API/CORS proxy throttling. When all aircraft fetch pathways are temporarily cooling down, the status changes to `Cooling Down` and suppresses repeated browser notifications until a retry is due.
- **AIS Ships OFF / ON**: Enter your own AISstream.io API key to subscribe to live vessel position reports for the current radar range. The key is stored only in your browser localStorage; no API keys are committed or bundled.
- **Aircraft View**: Review detected aircraft and select entries for more detail. Selecting an aircraft opens the cockpit view, which attempts to draw nearby airport runways in the forward window when OpenStreetMap aeroway data is available and overlays AIS vessels that fall within the pilot-relative forward display range.
- **System Logs**: Inspect app messages, API status, and warnings.


## Saved Browser Preferences

SkyRadar stores user preferences in the same browser using `localStorage` so repeated visits on the same machine keep the previous setup. Saved preferences include the last radar center, selected radius, auto-update state, search/filter settings, sort order, aircraft visibility toggles, and AIS enablement/API key settings. These values stay local to your browser and are not committed to the repository or sent to any SkyRadar backend.

## Data Sources and Limitations

- Aircraft traffic uses public ADS-B-derived data sources from the browser.
- Ship traffic uses AISstream.io over a browser WebSocket when you enable AIS and provide your own API key. SkyRadar subscribes to a bounding box around the selected radar center and filters vessels to the configured circular range.
- The location search and picker use OpenStreetMap tiles and Nominatim search from the browser; search availability depends on network access and public service limits.
- Cockpit runway cues query OpenStreetMap Overpass for nearby `aeroway=runway` geometry and render a simplified synthetic view; runway availability depends on network access, Overpass rate limits, and OpenStreetMap coverage. Failed runway lookups are cached for the current cockpit area and pause additional Overpass requests briefly so selecting multiple aircraft does not repeatedly hammer a blocked endpoint.

## Roadmap

- Add a small automated test suite for UI behavior and data parsing.
- Split the single-page implementation into maintainable modules.
- Add user-configurable data sources and API settings.
- Add user-configurable map style presets.
- Improve mobile layout and accessibility for keyboard and screen-reader users.

## License

SkyRadar is released under the MIT License. See [LICENSE](LICENSE) for details.
