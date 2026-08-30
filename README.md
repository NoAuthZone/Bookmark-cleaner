# Bookmark Cleaner

A single HTML file that cleans up browser bookmark exports. Drop in the HTML export from any
browser, decide what gets removed, and download a cleaned file you can import again.

Everything runs locally in your browser. No upload, no server, no build step, no dependencies —
open the file and it works, including offline.

Built by **NoAuthZone**.

## Why

Bookmark collections rot. The same link ends up in three folders, entries from 2011 point at
domains that no longer exist, and folder structures grow without anyone maintaining them.
Browsers offer no tooling for this beyond deleting entries one at a time.

This tool works on the universal `NETSCAPE-Bookmark-file-1` HTML format that every major browser
exports, so it does not care which browser the file came from — and it never touches your live
bookmarks. You import the result yourself, when you are ready.

## Quick start

1. Export your bookmarks from your browser as HTML.
2. Open `bookmark-cleaner.html` in any modern browser (double-click is enough).
3. Drop the export onto the page.
4. Pick folders, adjust the options, download the cleaned file.
5. Import the cleaned file back into your browser.

The page itself lists the export and import steps for Firefox, Chrome, Edge, Brave, Tor Browser
and Opera/Vivaldi.

## Features

### Duplicates

- Duplicates are detected **per folder**, among direct siblings only — subfolders are separate.
- Comparison mode per folder: by URL, by title, by both, or off. A global default sets all
  folders at once; individual folders can differ.
- Optional fuzzy URL matching that ignores `http`/`https`, a leading `www.` and a trailing slash.
- The first entry of a group is kept; click any duplicate's tag to keep that copy instead.
- Global link statistics: how many distinct links exist, how many appear exactly once, and how
  many have copies somewhere in the tree.

### Structure

- Folder tree with per-folder include/exclude. Unchecked folders are left completely untouched.
- Sort selected folders by title, by newest, or by oldest — applied both in the tree and in the
  export, so what you see is what you get.
- Subfolders before bookmarks, and dropping folders that end up empty, are separate switches.

### Editing

- Edit a bookmark's **title and its link**, or move it to another folder.
- **Select & move**: tick as many bookmarks and folders as you like — with shift-click for
  ranges — and move them together. Folders take their contents along; a folder cannot be moved
  into itself.

### Collecting into new folders

Rule-based collection that creates new folders from entries scattered across the tree:

- **By keyword**, one folder per line:

  ```
  Karriere: karriere, jobs, bewerbung
  Rezepte: rezept, kochen, backen
  python
  ```

  `Name: term, term` names the folder explicitly; a bare list takes its name from the first term.
  An entry lands in the first line it matches, which makes the line order your priority.
  "One folder per keyword" splits unnamed lines so every term gets its own folder.
- **Links that appear only once** / **links that appear more than once** — counted across the
  whole tree, not per folder.
- **Older than the age filter** and **unreachable links**.

New folders can be created at the top level or inside any existing folder, and they can either
move the entries or copy them.

### Dates and age

- Every row shows a date, taken from `LAST_MODIFIED` if the export has one, otherwise `ADD_DATE`.
- Filter the tree by a date range, with a switch for whether undated entries stay visible.
- Mark entries older than 1, 2 or 5 years — the date column turns amber and shows the age.
- Sort by date in either direction.

### Link check

An optional pass that requests every `http(s)` link and flags the ones that fail. It is
cancellable and shows progress.

> **Caveat:** browsers block the real status code for cross-origin requests (CORS), so
> "unreachable" means the request failed — not that the page is gone. Treat it as a hint, not a
> verdict.

### Multiple files

Load several exports at once and each becomes its own root folder. A comparison view then shows,
for folders with matching names, which entries exist in which file and where they overlap.

### Export

- **HTML** in the original bookmark format — the only one a browser can import.
- **CSV** and **JSON** as a flat list of everything that survives the cleanup.

## Performance

Tested against a synthetic export with 4,800 bookmarks in 200 folders:

- Collections above 800 bookmarks start collapsed, so the initial view builds ~40 rows instead
  of ~5,000. `Expand` / `Collapse` open and close everything.
- The tree never draws more than 1,200 filtered or 4,000 unfiltered rows; a note reports the
  rest. Undrawn entries are still counted, cleaned and exported.
- Heavy panels (duplicate list, domain statistics, file comparison) are only computed when their
  tab is open.
- Analysis and rendering are coalesced into one pass per animation frame; search input is
  debounced; URL normalisation and folder counts are cached.

Toggling a folder went from roughly 2,000 ms to under 200 ms on that file.

## Privacy

- No network requests, except the optional link check, which contacts exactly the sites you
  bookmarked.
- No storage, no cookies, no telemetry, no external fonts or scripts.
- Your bookmarks never leave the page. Closing the tab discards everything.

## Browser support

Any current Chromium-based browser, Firefox, or Safari. The tool uses standard DOM APIs,
`DOMParser`, `Blob` downloads and `AbortController`.

## Development

There is no build step and no dependency to install. `bookmark-cleaner.html` contains the markup,
styles and script, and can be edited directly.

## Known limitations

- The link check cannot distinguish "site is down" from "site blocks cross-origin requests".
- Duplicate detection is deliberately scoped to one folder at a time. To catch duplicates across
  the whole tree, collect the repeated links into one folder first, then let the duplicate check
  run inside it.
- Favicons (`ICON` attributes) are preserved but never displayed.
- Very large exports are rendered with a row cap rather than a virtualised list.


