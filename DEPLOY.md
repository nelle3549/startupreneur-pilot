# Deploying the G1 L01 pilot to GitHub Pages

You don't need to install anything or use the command line. Everything below happens in a web browser.

## 1. Create a GitHub account (skip if you have one)

Sign up at https://github.com. Free plan is fine.

## 2. Create a new repository

1. Click the **+** icon in the top-right of GitHub → **New repository**.
2. **Repository name:** `startupreneur-pilot` (or any name you like — this becomes part of the URL).
3. **Description:** "Pilot deployment of Startupreneur G1 L01" (optional).
4. **Visibility:** Public (required for free GitHub Pages).
5. Check **Add a README file**. (We'll overwrite it in a moment.)
6. Click **Create repository**.

## 3. Upload the pilot files

1. On the new repo's main page, click **Add file** → **Upload files**.
2. In Finder, open `github-pilot-g1-l01/`. Select **all visible items inside it** (Cmd+A):
   - `G1_Lesson_01_v2.1.html`
   - `README.md`
   - `index.html`
   - `lesson_v3.css`
   - `fonts/` folder
   - `images/` folder
   - `.gitignore` (you may need to press Cmd+Shift+. in Finder to see it)
3. **Drag them all** into the GitHub upload area. Wait for the green checkmarks to confirm every file uploaded (~5 MB total — should take under a minute).
4. Scroll down. Under **Commit changes**, leave the default message or write "Initial pilot upload."
5. Click **Commit changes**. The browser will return to the repo home page.

> ⚠️ Important: upload the **contents** of the `github-pilot-g1-l01/` folder, not the folder itself. The HTML files should sit at the repo root, not inside another folder.

## 4. Enable GitHub Pages

1. On the repo page, click the **Settings** tab (far right of the top nav).
2. In the left sidebar, click **Pages**.
3. Under **Build and deployment** → **Source**, choose **Deploy from a branch**.
4. Under **Branch**, choose **main** and leave the folder as `/ (root)`. Click **Save**.
5. GitHub will say "Your site is live at `https://<your-username>.github.io/startupreneur-pilot/`" — but it usually takes 1–3 minutes for the very first build to finish. Refresh the Pages settings page until you see a green checkmark.

## 5. Visit the live site

Open the URL GitHub gave you. The landing page should briefly appear, then auto-redirect to the G1 L01 lesson with:
- Andika font
- All 4 images
- Properly styled lesson sections
- The YouTube video embed

If something looks broken, see Troubleshooting below.

## 6. Share the URL

The URL will look like:
```
https://<your-username>.github.io/startupreneur-pilot/
```

Anyone with the link can open it — no GitHub account required. Bookmark it for the LMS team to test embedding.

---

## Updating the lesson later

When you tweak the HTML or replace an image, you have two options:

**Option A — Re-upload via web (easy):**
1. On the repo, click the file you want to replace → **trash icon** to delete, then upload the new version. Or click the **pencil icon** to edit text files directly in the browser.
2. Commit the change. GitHub Pages redeploys automatically in 1–2 minutes.

**Option B — GitHub Desktop app (better for many changes at once):**
1. Install https://desktop.github.com.
2. Clone the repo to your Mac.
3. Edit files locally, then click **Commit** → **Push** in the app.

## Going private later

When you're ready to lock down access:
- **Cheapest:** Upgrade to GitHub Pro (~$4/mo). Lets a private repo serve a public Pages site.
- **Recommended for LMS:** Move to **Cloudflare Pages** (free, allows private source repos) or **Netlify** (free tier). Both can integrate with your LMS for token-gated access.
- **Alternative:** Have your LMS download and serve the lesson HTML directly from a private repo using a GitHub API token.

We can revisit the migration when the LMS is ready.

---

## Troubleshooting

**Images missing / 404 errors:** Check the repo file list. Each image must be under `images/` and the lesson HTML expects exact lowercase names. Filenames are case-sensitive on GitHub Pages even if they aren't on macOS.

**Font looks wrong:** Confirm the `fonts/` folder uploaded with all 4 `.woff2` files. If only the Google Fonts fallback loads, the font will look slightly different but still readable.

**404 on root URL:** Make sure `index.html` is at the repo root, not nested in a `github-pilot-g1-l01/` subfolder. Common upload mistake.

**Site doesn't update:** GitHub Pages caches aggressively. Hard-refresh with Cmd+Shift+R, or wait 5 minutes for the cache to expire.

**Want a custom domain (e.g., `pilot.startupreneur.ph`):** Settings → Pages → Custom domain. You'll add a CNAME DNS record on your domain registrar. Free to set up, lets you use HTTPS.
