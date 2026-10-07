# Q-Share

Qortal's public file-sharing Q-App. Publish files with a title, description and category, browse and search everyone's shares, preview and download them, comment, follow publishers, and save shares to collections.

**Version 2.0** is a redesign first published as Q-Share+ 1.0.0 (2026-09-30), built by Simon James in [SJQortal/Q-Apps-Plus](https://github.com/SJQortal/Q-Apps-Plus). It reads and writes the same QDN data as Q-Share 1.0: shares, files and comments made with either version show up in both, and nothing needs migrating. `CHANGELOG.md` lists every change.

## What's new in 2.0

- **The current stack:** React 19.3, MUI 9.4, Redux Toolkit 2, Vite 8 and TypeScript 5.9.
  - A Quill 2 editor that still stores descriptions in the Quill 1 markup 1.0 reads.
  - ESLint 9 with the React hooks rules, and about 490 vitest tests.
- **Built for phones and GO:**
  - a bottom bar and a Share button;
  - a header that hides as you scroll;
  - filters in a bottom sheet, and pull-to-refresh;
  - full-screen Share and Edit forms with Publish kept above the keyboard;
  - 44 px targets, and a compact layout for phones held sideways.
- **Four themes** (Hub 3.0, Q-Share Classic, Black, White):
  - they follow Hub's light/dark switch without a reload;
  - text contrast is at least 4.5:1;
  - Settings → Appearance switches between them.
- **Finding shares:**
  - a Following feed;
  - My shares for one or all of your names;
  - name suggestions in the publisher filter;
  - list or grid view;
  - hidden names.
- **Share pages:**
  - previews for images, text, audio and video, and PDFs in Hub's own reader;
  - Fetch all, and Save all as .zip;
  - safe rendering of descriptions (DOMPurify 3.4, links built on the DOM).
- **Publishing:**
  - drag and drop, sizes and a total;
  - a draft that survives closing;
  - Hub's progress per file;
  - retries that check QDN first, so a fee is never paid twice.
- **Collections:** named lists of shares, on their own page and on profiles.
- **Settings:** name switcher, content options, blocked names, statistics, and an optional Sync of settings to QDN.
- **Lighter on Qortal:**
  - paged and cached searches, with no unlimited (`limit: 0`) queries;
  - lazy avatars;
  - polling that stops when the tab is hidden;
  - a first download about a quarter of 1.0's size.

## Develop

```bash
npm ci
npm run dev          # Qortal calls need Hub (Dev Mode); tests use the mocks in src/test/setup.ts
npm test
npm run lint
npm run build
node e2e/screens.mjs # every screen at 5 sizes in 4 themes, with overflow, accessibility (axe) and call-count checks
```

The screenshot check needs Playwright, either a global `playwright`, or `playwright-core` plus a Chromium-based browser in `QPLUS_CHROMIUM`. See the header of `e2e/screens.mjs`.

To publish, zip the contents of `dist/` with `index.html` at the root, and publish the zip in Hub as an `APP` resource under the name `Q-Share`.

## Data

Shares, files and comments use the same services, identifiers and JSON shapes as 1.0:
- `qshare_file_…` DOCUMENT and FILE resources;
- BLOG_COMMENT comments;
- descriptions in Quill 1 markup.

Two kinds of data are new, and 1.0 ignores both:
- collections: DOCUMENT resources named `qshare_collection_…`;
- the optional settings sync: a DOCUMENT named `qshare_settings` under the user's name.

## Notes for maintainers

- **Theme kit:** `src/hub-theme/` is the theme kit from [SJQortal/Q-Apps-Plus](https://github.com/SJQortal/Q-Apps-Plus) (`shared/hub-theme`), copied in. Edit it here, or copy in later versions from there.
- **Link name:** copied `qortal://APP/…` links use `PUBLISHED_APP_NAME` in `src/utils/qortalLinks.ts`. Hub never decodes the app name, so it's written exactly as registered.
- **History:** every change in this history is its own commit, with a message that explains it.
- **Records:** the full audit, data contract, Hub test records and the list of Hub pitfalls found are in [docs/apps/Q-Share+.md](https://github.com/SJQortal/Q-Apps-Plus/blob/main/docs/apps/Q-Share%2B.md).
- **Removed** (all unused, listed in `CHANGELOG.md`):
  - code carried over from Q-Tube that never ran (its video player and playlist screens);
  - unused fonts;
  - the moment, react-quill, react-rnd, compressorjs and ts-key-enum dependencies.
