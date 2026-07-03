# AGENTS.md

Guidance for AI coding agents working on SkyRadar.

## Project Overview

SkyRadar is a static browser application for tracking nearby ADS-B aircraft on a Leaflet map. The current app is implemented in `index.html` and loads third-party browser dependencies from CDNs.

## Working Guidelines

- Keep the app runnable as a static site unless a task explicitly introduces a build system.
- Prefer small, focused changes that preserve the existing single-page behavior.
- Do not add secrets, API keys, or private credentials to the repository.
- Avoid wrapping imports in `try`/`catch` blocks.
- If adding new generated artifacts or dependency directories, update `.gitignore` first.
- Keep documentation in sync when controls, setup steps, or supported data sources change.

## Local Verification

Use one of these quick checks before submitting changes:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` and verify the page loads without console errors.

For documentation-only changes, at minimum run:

```bash
git diff --check
```

## Pull Request Notes

In PR descriptions, include:

- A short summary of the change.
- Any manual or automated checks performed.
- Known limitations, especially browser, geolocation, or public API constraints.
