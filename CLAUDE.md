# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file deep-link landing page for the **Aplikite** Android app (Indonesian market). When a user visits a URL like `/app/{appId}`, the page immediately attempts to open the native app via a custom URI scheme, with fallback UI to open manually or download from Google Play.

## Development

No build step, no dependencies. Open `index.html` in a browser directly, or use a local server:

```
npx serve .
```

Deployed on Vercel. The `vercel.json` rewrites all routes to `index.html` so deep-link paths like `/app/some-id` are handled client-side.

## Architecture

Everything lives in `index.html` — markup, styles (embedded `<style>`), and logic (embedded `<script>`). There are no separate JS or CSS files.

### Deep-link logic (`<script>` at bottom of index.html)

- Reads `appId` from `location.pathname.split('/')[2]` (i.e. `/app/{appId}`)
- Constructs the deep link: `aplikite://app/{appId}`
- Sets it as the `href` of `#openAppBtn` for manual tap
- After 1 second, triggers `location.href = deepLink` via `setTimeout` to auto-redirect
- If `appId` is absent, the deep link is not constructed and the page degrades gracefully

### Key elements

- `.loader` — spinning CSS animation shown while the redirect is in progress
- `#openAppBtn` — manual open button; `href` is set dynamically to the deep link
- `.download-btn` — static link to the Google Play listing (`com.aplikite.app`)

## Language

All user-facing copy is in Indonesian (Bahasa Indonesia).
