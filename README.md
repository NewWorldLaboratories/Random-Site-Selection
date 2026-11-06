<p><img src="icons/logo.png" alt="New World Laboratories" height="90"></p>

# Random Site Locator

A phone web app for drawing random sampling and test sites on road and pavement lots. Technicians open it on site, enter the lot, draw the sites, lay them over a photo of the supplied drawing, record what happened at each site, and save a one-page report.

**Live app:** `https://<your-github-account>.github.io/<repo-name>/` (see [Publishing](#publishing-with-github-pages))
**Field guide for technicians:** [guide.html](guide.html), also opened from the **Help** button in the app

## What it does

- **Methods:** WA 0.1 stratified (cl 4.2) and unstratified (cl 4.1), AS 1289.1.4.1 (random number method) and AS 1289.1.4.2 (stratified random number method).
- **Site selection by location** for cores and nuclear density.
- **Multi-lane lots:** each lane or paver run is entered in the order it was laid. Stratified sub-lots are equal lengths and carry on from one run into the next (WA 0.1 Figure 3).
- **Width that changes along a run:** width points with a straight-line change between them. The lateral offset is a random number times the testable width at the site.
- **Live updates:** sites are drawn as soon as the lot is complete. Correcting the lot afterwards moves the sites using the same random numbers, and the report records the change.
- **Joint and edge clearances:** sites can't land within a set distance of longitudinal joints, edges, transverse joints, lot ends or excluded zones.
- **Excluded zones** such as bridges, intersections and pits, recorded on the report.
- **Plan overlay:** a photo, dropped file or pasted image of the supplied drawing, lined up from two or more chainage points tapped on it, with the lot and numbered sites drawn on top. Extra points make it follow curves.
- **Measured widths:** tap a site's width in the Sites table to enter what was measured; the offset recalculates from the same random number (AS 1289.1.4.1 cl 3.2(d)(iii)).
- **Report:** one complete A4 portrait page (lot plan, sites table, marked-up drawing, notes, signatures), with every draw and redraw reason recorded. Also exports CSV and a lot file (JSON).
- **Several lots per trip:** lots save on the phone as the tech works, with or without signal. All of a day's reports can be saved in one file back at the lab.
- **Offline:** once opened with signal, the app opens and works with no signal. It can be installed to the home screen.

Not covered: stockpile sampling (WA 0.1 cl 4.1.3 / 4.2.3) and vertical location.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app: page, styles and code in one file |
| `guide.html` | Field guide for technicians |
| `sw.js` | Offline support (caches the app on the phone) |
| `manifest.webmanifest` | Home-screen app name, colours and icons |
| `icons/` | App icons and the logo used in this README |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |
| `verification/` | Simulation check records (single paver run, v1.0.1) |

There is no server, database, build step or account system. GitHub Pages only serves the files.

## Publishing with GitHub Pages

1. Create a repository (for example `random-site-locator`) and upload all the files in this folder, keeping the `icons` folder.
2. In the repository go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**, **Branch** to `main` and the folder to `/ (root)`, then **Save**.
4. After a minute or two the page shows the address: `https://<account>.github.io/<repo-name>/`. Send that address to the technicians with the field guide.

GitHub Pages is free for **public** repositories. A private repository needs a paid GitHub plan (Pro, Team or Enterprise) to use Pages. The app holds no job data, so a public repository only exposes the code and the logo.

## Updating the app

1. Edit `index.html` (or `guide.html`).
2. Raise the version in **two** places so phones fetch the new copy and reports show which version was used:
   - `const APP_VERSION = "1.0.0";` in `index.html`
   - `const VERSION = "rsl-1.0.0";` in `sw.js`
3. Commit to `main`. GitHub Pages republishes in a minute or two.
4. Phones pick up the update the next time the app is opened with signal. Sometimes it needs closing and opening twice.

Lots already saved on phones are kept through updates.

## Where data is kept

- Everything a technician enters stays **on their phone**, in the browser's storage for this site (IndexedDB). Nothing is sent to a server.
- Lots stay until the technician deletes them. They are lost if the browser's data is cleared, or if a private or incognito tab is used.
- On iPhone, Safari can clear a site's storage if it isn't opened for 7 days. A home-screen install avoids that. Reports should be saved at the lab the same day.
- The only outside request is for fonts from Google Fonts. If that fails, the app uses the phone's own fonts.

## Random numbers

Random numbers come from the browser's cryptographic generator (`crypto.getRandomValues`), rounded to 4 decimal places. Each draw is unseeded, so it can't be repeated or predicted (WA 0.1 cl 4.3.2, AS 1289 cl 3.1(b)). Every draw and redraw, with its reason, is listed on the report.

## Before first use

Have the lab's quality manager check the app against the standards and the lab's procedures. Run several real lots alongside the current spreadsheets and compare the results. Record the app version that was checked. Each report prints the app version it was made with.

## Changes

- **1.1.0**: Report includes every working (parameters, testable stretches, and per-site distance, stretch, exact and set-out chainage, widths, r₁, r_w, offsets) so it can be back-calculated. Save PDF (made in the app) and Save HTML (carries the lot; import it to reopen). Redraw button removed (lot edits update live; changing N or method draws new numbers automatically). Report is one complete A4 portrait page. On site tab removed (widths entered live in the Sites table). Plan tab takes a dropped or pasted drawing and draggable chainage points. Lot drawing is a plan view to scale (soils lots look like the lot). Measured widths editable per site and shown on the drawing. Sites table fits a phone. Field guide rewritten.
- **1.0.2**: Lot and Draw are one screen with live updates. Width can change along a run. Sub-lots are equal lengths for all methods; width is random only. Site chainages rounded to the nearest metre (never into a clearance). Random subset and time/tonnage removed. Phone layout fixes.
- **1.0.1**: Sites are now tied to the exact lot they were drawn for. Changing a run's chainages, adding a paver run, or changing a clearance or excluded zone marks the sites out of date. Site cards are hidden, sites come off the map, and the report shows NOT VALID until a redraw. This fixes sub-lots and sites staying on the first paver run. Single-run simulation check added in `verification/`.
- **1.0.0**: First release.

## Standards referenced

- Main Roads Western Australia **Test Method WA 0.1** (2019), Random Sampling and Test Site Selection
- **AS 1289.1.4.1** (2019), Selection of sampling or test sites: random number method
- **AS 1289.1.4.2** (1998, reconfirmed 2013), Selection of sampling or test sites: stratified random number method

---
© New World Laboratories. Internal use.
