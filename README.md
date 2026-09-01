<div align="center">

# Bookmark Cleaner

**Clean up, de-duplicate and sort your browser bookmarks — entirely offline, in a single HTML file.**

[![Version](https://img.shields.io/badge/version-1.1.0-blue)](https://github.com/NoAuthZone/bookmark-cleaner/releases)
[![No dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)](#)
[![Offline](https://img.shields.io/badge/runs-100%25%20offline-success)](#privacy)
[![Single file](https://img.shields.io/badge/build%20step-none-lightgrey)](#)

</div>

---

Export your bookmarks, drop the file onto the tool, decide what should happen, download a cleaned file, import it back. No install, no server, no account, no upload — just one HTML file you open in your browser.

> [!NOTE]
> Your original export is never modified. The tool works on a copy in memory and writes a new file when you ask it to.

## Table of contents

- [Quick start](#quick-start)
- [Features](#features)
- [Privacy](#privacy)
- [Browser support](#browser-support)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

## Quick start

1. **Download** [`Bookmark-Cleaner.html`](https://github.com/NoAuthZone/bookmark-cleaner) and open it in your browser (double-click — no web server needed).
2. **Export your bookmarks.** The tool has step-by-step instructions built in for Firefox, Chrome, Edge, Brave, Tor Browser and other Chromium-based browsers.
3. **Drop the exported `bookmarks.html`** onto the page.
4. **Pick your options** and review the tree.
5. **Download the cleaned HTML** and import it back into your browser.

## Features

<details open>
<summary><b>Clean up</b> — duplicates, sorting, editing</summary>

<br>

| Feature | What it does |
|---|---|
| Per-folder duplicate detection | Compare by URL, title, both, or off. Set it globally, override individual folders. |
| Fuzzy URL matching | Optionally ignore `http`/`https`, a leading `www.` and a trailing slash when comparing. |
| Folder selection | Only checked folders get cleaned and sorted — unchecked ones pass through untouched. |
| Sorting | Original order, title A–Z, or by date (newest/oldest first), optionally subfolders first. |
| Empty folder removal | Folders left with nothing after cleanup can be dropped from the export. |
| Manual editing | Rename a bookmark, fix its link, edit its tags, or move it to another folder. |
| Select & move | Pick several entries and relocate them together. |
| Search & bulk remove | Filter the tree by title, link or tag — then remove every match at once. |

</details>

<details>
<summary><b>Date &amp; age</b> — find the stale stuff</summary>

<br>

- **Date range filter** — show only entries within a given period, with an option to keep undated entries visible.
- **Age marking** — flag everything older than 1, 2 or 5 years, based on the last change date or, where the export has none, the date added.

</details>

<details>
<summary><b>Link checking</b> — find dead links</summary>

<br>

- **Checked per address, not per copy.** Each distinct URL — including its full path, not just the domain — is fetched once; identical copies inherit the verdict.
- **Two distinct outcomes.** `unreachable` for requests that fail outright, `no answer` for requests that time out after 10 seconds, so slow sites are not written off as dead.
- **Resumable.** "Only unchecked links" lets a second run pick up what is new or was interrupted.

> [!WARNING]
> Browsers block the real HTTP status code (CORS), so a failed request is a **hint**, not proof that a page is gone. Verify before deleting.

</details>

<details>
<summary><b>Insights</b> — see what your collection actually looks like</summary>

<br>

**Overview** — totals, distinct links, duplicate groups, what will be removed, empty folders and link-check results at a glance.

**Top domains** and **Top links (full URL)** — where your bookmarks actually point, ranked by frequency. The link list groups by host *and* path/query, not just the domain.

Both lists support three filter modes plus a free-text filter:

| Mode | Example use |
|---|---|
| `Show top…` | The 100 most frequent links |
| `Occurs…` | Exactly once · twice and up · between 2 and 5 times |
| `Percentile…` | The 30 %–40 % slice of the ranked list |
| Text filter | Narrow either list to a specific site, e.g. `heise` |

From either list you can **remove every bookmark** of a domain or link in one click, or **turn the current filter result into a new folder** (copying by default, so originals stay put).

</details>

<details>
<summary><b>Collect into a new folder</b> — regroup in bulk</summary>

<br>

Group entries into new folders **by keyword** (one folder per line, with optional explicit names) or **by rule**:

- links that appear only once
- links that appear more than once
- entries older than the age filter
- links that failed the check

Choose where the folder is created and whether entries are moved or copied.

</details>

<details>
<summary><b>File comparison &amp; exports</b></summary>

<br>

**File comparison** — load a second export to compare two files and see what is only in one of them.

**Exports:**

| Format | Purpose |
|---|---|
| HTML | The cleaned bookmark file, importable back into any browser |
| CSV | Flat list of everything that survives the cleanup |
| JSON | Same, for scripting and further processing |

</details>

<details>
<summary><b>Interface</b></summary>

<br>

- **Dark and light theme** — dark by default, toggled from the top bar, remembered between sessions.
- **Collapsible option groups** — every closed group shows a one-line recap of its current setting, so nothing is hidden without a trace.
- **Remembered settings** — theme, Insights filters and which option groups are open persist across reloads.
- **Tag support** — bookmark tags from Firefox exports are shown, searchable, editable and preserved on export.

</details>

## Privacy

Everything runs locally in the page. Your bookmark file is read in the browser and **never uploaded**.

The only outgoing network requests are the ones the link checker makes to the sites you bookmarked — and only when you click **Check links**.

Theme choice, Insights filter settings and which option groups are open are stored in your browser's `localStorage`. Nothing else is persisted, and nothing is sent anywhere.

## Browser support

Any current desktop browser: Firefox, Chrome, Edge, Brave, Tor Browser, Opera, Vivaldi, Safari. The layout adapts to narrow screens, but the tool is most comfortable in a desktop-sized window.

## FAQ

<details>
<summary>Does it modify my original export?</summary>
<br>
No. The file you drop in is read into memory and never written to. Cleaning produces a new file that you download explicitly.
</details>

<details>
<summary>Are Firefox tags preserved?</summary>
<br>
Yes. Tags are parsed, displayed, searchable, editable in the tool, and written back unchanged on export. Chrome and Edge exports have no tags to begin with.
</details>

<details>
<summary>Does the link checker check full URLs or just domains?</summary>
<br>
Full URLs, including path and query. A deleted subpage on a live domain is detected — within the limits CORS imposes.
</details>

<details>
<summary>Why does a site I know is fine show up as unreachable?</summary>
<br>
Browsers restrict what a page may learn about cross-origin requests. Some sites refuse the kind of request the checker makes, even though they work fine in a normal tab. Treat the result as a shortlist to review, not a verdict.
</details>

<details>
<summary>Can I run it from a USB stick / air-gapped machine?</summary>
<br>
Yes. It is one self-contained HTML file with no external resources. Only the link checker needs a network connection, and it is optional.
</details>

## Contributing

Issues and pull requests are welcome. Since the project is a single self-contained HTML file, keep changes dependency-free and make sure the tool still works when opened directly from disk (`file://`).

---

<div align="center">
Built by <b>NoAuthZone</b> · Runs entirely offline · Nothing leaves your browser
</div>
