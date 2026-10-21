# Quran in Shorts: website guide

A fast, mobile-friendly page with a card for every Surah (1 to 114) plus a "Lessons by Topic" tab. Every card shows the real YouTube thumbnail and opens the video in a popup. New videos from your channel are picked up automatically.

## What is in this folder

| File | What it does | Do you edit it? |
|---|---|---|
| `index.html` | The whole website (design, cards, player) | No |
| `links.js` | **Your list of video links.** Add a link, commit, done | **Yes** |
| `videos.js` | Titles, channel names and publish dates, written by the auto-sync | No (automatic) |
| `sync_videos.py` | The auto-sync program | No |
| `.github/workflows/sync-videos.yml` | Runs the sync on GitHub every 6 hours | No |

---

## 1. Put the site online (GitHub Pages, free)

1. On github.com press **New repository**, name it e.g. `quran-shorts`, keep it **Public**, press **Create**.
2. Press **uploading an existing file**, drag in **everything from this folder** (including the `.github` folder), then press **Commit changes**.
   - If the `.github` folder does not upload, press **Add file → Create new file**, type the name `.github/workflows/sync-videos.yml` (typing the slashes makes the folders), paste the file's contents, and commit.
3. Go to **Settings → Pages**. Under *Build and deployment* choose **Deploy from a branch**, branch **main**, folder **/ (root)**, press **Save**.
4. After a minute your site is live at `https://YOUR-USERNAME.github.io/quran-shorts/`.

## 2. Turn on auto-sync (one time)

This is what makes new videos appear by themselves and fills in all titles and thumbnails for visitors.

1. **Settings → Actions → General → Workflow permissions** → choose **Read and write permissions** → **Save**.
2. Open the **Actions** tab. If GitHub asks, press **I understand my workflows, enable them**.
3. Press **Sync videos** (left side) → **Run workflow** → **Run workflow**.
4. In about a minute it finishes (green tick) and `videos.js` now contains every title. The site refreshes by itself a minute or two later.

From now on the sync runs **every 6 hours**, and also **every time you change `links.js`**. You can always press **Run workflow** to run it immediately.

## 3. Adding more videos

### A. Just upload to YouTube (nothing else to do)
Within about 6 hours the new video appears on the page.
- If the title names the Surah, it lands on that Surah's card, e.g. `Surah Al-Ikhlas Summary in Urdu | سورۃ الاخلاص`, `سورۃ النصر کا خلاصہ`, or `Surah 112 ...`.
- If the title does not name a Surah, it appears under **Lessons by Topic → New uploads** (with a NEW badge for 14 days).
- The channel feed only holds your newest 15 videos, which is plenty for a 6-hour check. For older videos use option B.

### B. Paste links into `links.js` (best for many links, or to choose a topic)
On GitHub open `links.js` → pencil icon → add lines → **Commit changes**. The sync runs and adds the titles.

```js
window.SHORTS = [
  "https://www.youtube.com/shorts/VIDEO_ID",            // matched to its Surah from the title
  "https://www.youtube.com/shorts/VIDEO_ID|36",         // forced to Surah 36 (Ya-Sin)
];
window.LESSONS = [
  { url: "https://www.youtube.com/shorts/VIDEO_ID", topic: "Stories of the Prophets" },
];
```
Links can be `youtube.com/shorts/...`, `youtu.be/...` or normal `watch?v=` links. Keep the quotes and commas.

### C. From the page itself (owner tools)
Open your site once with `?owner` on the end, e.g. `https://YOUR-USERNAME.github.io/quran-shorts/?owner`. Two buttons appear for you only: **✎ Manage** and **+ Add links**. Visitors never see them. Add `?owner=off` to hide them again.
- **+ Add links**: paste one or many links, optionally type a topic, press **Add to page**. Thumbnails and titles appear straight away.
- **✎ Manage**: change a card's topic, assign it to a Surah, or remove it.
- These changes are saved **in your browser only**. To publish them for everyone, press **Copy links.js** in the Add dialog, open `links.js` on GitHub, select all, paste, and commit.

## 4. How thumbnails and details work

- The thumbnail is loaded straight from YouTube using the video id. The page tries the best size first, then smaller ones, and ignores YouTube's tiny grey "no thumbnail" image. If YouTube has no image at all, the Surah card keeps its Arabic artwork and lesson cards show a neutral play placeholder.
- Titles and channel names are saved in `videos.js` by the auto-sync, so the page opens instantly. If a video is not in `videos.js` yet (for example one you just added with the owner tools), the browser looks the title up by itself.
- Shorts are shown as tall posters and normal videos as wide ones. The sync detects which is which.

## 5. Hide a video

Put its id (the 11 characters after `shorts/`) in `links.js`:

```js
window.HIDDEN = [ "VIDEO_ID" ];
```

## 6. Previewing on your own computer

Opening `index.html` by double-click works, but YouTube refuses to play videos inside a page opened that way, so the popup shows a **Watch on YouTube** button. To preview properly, open a terminal in this folder and run `python3 -m http.server 8000`, then visit `http://localhost:8000`. Once the site is online (https) the player works normally.

## 7. Troubleshooting

| Problem | Fix |
|---|---|
| A card says **Coming soon** | No video is matched to that Surah yet. Add its link to `links.js` with `\|number`, or check the video title names the Surah. |
| A video is on the wrong Surah | Add `\|number` after its link in `links.js` to force the right one. |
| A video is under **Lessons › Unsorted** | Its title has no Surah name. Add `\|number` after its link, or give it a topic in the `LESSONS` list. |
| Two videos for one Surah | The first listed wins the card; the other goes to **Lessons › More Surah videos**. |
| Action fails with a "403" or "permission" error | Do step 2.1 (Read and write permissions), then **Run workflow** again. |
| Site does not show a new video | Check the **Actions** tab for a green tick, wait a couple of minutes, refresh. Press **Run workflow** to force it. |
| Scheduled sync stopped | GitHub pauses scheduled jobs on repositories with no activity for 60 days. Open **Actions → Sync videos → Enable workflow**. |
| Video popup shows "Error 153" | The page is opened from a file on your computer, see section 6. If it happens online, check the video allows embedding (YouTube Studio → video details → Show more → *Allow embedding*). |

## 8. Using a different channel

Open `sync_videos.py` and change `CHANNEL_ID` near the top (the channel's `UC...` id). Also update the Subscribe links in `index.html`.

---
Remember in your prayers **Zubair Ahmad**. May Allah accept this effort.
