# Haunted Push/Pull — getting it online

Two pieces:

- **`index.html`** — the whole tracker. Static, no build step. Goes on GitHub Pages.
- **`Api.gs`** — a small Google Apps Script web app that owns the Sheet. This is the only server part; GitHub Pages can't write to a Sheet by itself.

The tracker works with no Sheet at all — it just saves to that phone's browser. Wire the Sheet up when you want the log to survive a cleared browser or move between devices.

## 1. Put it on GitHub Pages

1. New GitHub repo, e.g. `pushpull`. Public.
2. Upload `index.html`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` to the repo root. (`Api.gs` and this README can ride along; they're ignored by the site.)
3. Repo **Settings > Pages**. Source: *Deploy from a branch*. Branch: `main`, folder: `/ (root)`. Save.
4. Wait a minute. Your URL is `https://<username>.github.io/pushpull/`.

That URL is live on the internet and works on any phone.

## 2. Wire up the Google Sheet

1. New Google Sheet. Rename the first tab to **Log**.
2. **Extensions > Apps Script**, delete the sample, paste in all of `Api.gs`, save.
3. **Deploy > New deployment > Web app.** Execute as **Me**, Who has access **Anyone**. Authorize when asked.
4. Copy the `/exec` URL.
5. In `index.html`, find this line near the bottom and paste the URL between the quotes:

   ```js
   var API = '';
   ```

6. Commit the change. Reload the site on your phone, tap **Link**, and type a sync key — any word you'll remember (`sam`, `haunted`, whatever). Use the **same key** on every device and they share one log.

The dot next to the header tells you where you stand: grey = phone only, green = synced, orange = offline (still saved locally, pushes next time you're on).

If you edit `Api.gs` later, redeploy as a **New version** — Apps Script keeps serving the old code otherwise.

## 3. Add to the home screen

**iPhone (Safari):** Share > Add to Home Screen. **Android (Chrome):** ⋮ > Add to Home screen. It opens full screen with no browser bar.

## What changed from your Apps Script version

- Runs as one static file — no Apps Script hosting, no `<?= baseUrl ?>` templating, no `doGet` routing.
- Replaced the `window.claude.use` cloud block (which only worked inside the artifact viewer) with the Sheet sync above.
- Each set row now shows **last session's weight × reps** beside the inputs, and last session's numbers are the input placeholders.
- Home-screen manifest, icons, safe-area padding for notched phones, and taller inputs and buttons (44px+ tap targets).
- The program list (`Index.html`) is left out — this is the Push/Pull page only. Bikini Prep can be added later as a second page.
