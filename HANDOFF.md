# Claude Code handoff — G1 L01 GitHub Pages pilot

## Goal

Deploy the staged pilot folder at this path as a **public live website** so it's accessible at a URL anyone can open in a browser:

```
/Users/nelle/My Drive/000 Serial Disruptors/00 Startupreneur/000 HTML/SP v2 Improvement/github-pilot-g1-l01/
```

**End state to verify:** A live URL where loading the root shows G1 Lesson 01 with all 4 images, Andika font, lesson_v3.css styling, and a working YouTube embed.

## Preferred host

GitHub Pages (user said GitHub originally). If GitHub Pages turns out painful for any reason, Vercel or Cloudflare Pages are acceptable fallbacks — all three give a free public HTTPS URL for static sites. **Pick whichever you can fully automate end-to-end from the terminal.** GitHub CLI (`gh`) is usually preinstalled on macOS dev setups; if not, `brew install gh` or `npx vercel` are both fine.

## Context (what's already done)

1. **Pilot folder is fully staged.** All assets copied, paths verified, total ~5.6 MB. Structure:
   ```
   github-pilot-g1-l01/
   ├── index.html                       # landing → redirects to lesson
   ├── G1_Lesson_01_v2.1.html           # the lesson itself
   ├── lesson_v3.css                    # shared lesson stylesheet
   ├── fonts/                           # Andika webfont (4 .woff2 files)
   ├── images/                          # 4 PNGs the lesson references
   ├── README.md                        # repo description
   ├── DEPLOY.md                        # manual web-UI walkthrough (fallback)
   ├── HANDOFF.md                       # this file
   └── .gitignore                       # macOS junk filter
   ```
2. **External deps are CDN-hosted** (no need to bundle): Google Fonts (Andika), YouTube embed (Aling Nene's Sari-Sari Store video). The CSS also references local `fonts/Andika-*.woff2` as a faster path.
3. **`index.html` is a redirect** to `G1_Lesson_01_v2.1.html` so the root URL works cleanly. Don't rename the lesson file — the user's lesson naming convention preserves filenames across years and grades.
4. **macOS `Icon\r` files exist in the folder** (Finder custom-icon artifacts, zero bytes). `.gitignore` already excludes them via the `Icon` line. Confirm with `git status` before first commit that they're not staged. If they are, add `Icon?` or `Icon\r` patterns to .gitignore.
5. **Filename case sensitivity is locked in.** The lesson references `images/Store.png`, `images/g01_l01_case_aling_nene.png`, `images/man_woman.png`, `images/profit.png` exactly. GitHub Pages and Vercel are both case-sensitive even though macOS isn't — don't let Finder rename anything during a move.

## What to do

1. **Initialize git** in the pilot folder if not already:
   ```bash
   cd "/Users/nelle/My Drive/000 Serial Disruptors/00 Startupreneur/000 HTML/SP v2 Improvement/github-pilot-g1-l01"
   git init -b main
   git add .
   git status   # sanity check — confirm Icon files are NOT staged
   git commit -m "Initial pilot upload — G1 Lesson 01"
   ```

2. **Create a public GitHub repo and push.** Use `gh` if available:
   ```bash
   gh auth status   # confirm user is logged in; if not, gh auth login
   gh repo create startupreneur-pilot --public --source=. --remote=origin --push
   ```
   If `gh` isn't installed, fall back to creating the repo via Vercel's drag-drop deploy at https://vercel.com/new — it doesn't need a GitHub repo at all. Or `brew install gh` first.

3. **Enable GitHub Pages.** Either via CLI:
   ```bash
   gh api repos/{owner}/startupreneur-pilot/pages \
     -X POST -F build_type=legacy -F source[branch]=main -F source[path]=/
   ```
   Or via the web UI (Settings → Pages → Deploy from a branch → main / root → Save). First build takes 1–3 minutes.

4. **Get the live URL.** Either parse it from the Pages API response, or it follows the standard form:
   ```
   https://<github-username>.github.io/startupreneur-pilot/
   ```
   Confirm the user's GitHub username with `gh api user --jq .login` before asserting.

5. **Verify the deployment.** Poll the URL until it returns 200 OK (Pages can take a couple minutes on first build). Then:
   - `curl -sI <url>` returns 200
   - `curl -s <url> | grep -i "G1 Lesson 01"` finds the title
   - `curl -sI <url>images/Store.png` returns 200 (capital S — critical)
   - `curl -sI <url>lesson_v3.css` returns 200
   - `curl -sI <url>fonts/Andika-Regular.woff2` returns 200

6. **Report back to the user:**
   - The live URL
   - Confirmation that all 4 images + CSS + fonts return 200
   - The repo URL for future updates
   - A one-line reminder of the LMS migration plan (see "Future state" below)

## Constraints / non-goals

- **Don't add a build step** (no Vite, Next.js, etc.). This is plain HTML — Pages serves it directly. Any build tooling would over-engineer the pilot.
- **Don't rewrite the lesson HTML or CSS** in any way. The lesson file is production content with a fixed structure; this task is purely about hosting.
- **Don't include the parent `SP v2 Improvement/` folder.** The repo root must be `github-pilot-g1-l01/` so paths resolve correctly. Don't nest.
- **Don't make the repo private** for this pilot. User wants it public for testing. Private comes later (see Future state).
- **Don't push other lessons.** Pilot scope is G1 L01 only. If you find yourself thinking about G1 L02 or Y1, stop — that's the next milestone.

## Future state (not part of this task, but informs decisions)

- This is **one lesson of ~250+ planned**. The pilot proves the deploy pipeline. Once it works, we'll generalize.
- **End-state hosting will be private.** Lessons will live inside an LMS (third-party). The current public deploy is a *testing scaffold* only. When ready to migrate, options are:
  - GitHub Pro (~$4/mo) → keeps GitHub Pages, allows private source
  - **Cloudflare Pages** (free) → private source repos, supports token-gated access
  - LMS proxies/iframes content from a private GitHub repo via API token
- So don't invest in custom-domain setup yet — the URL will be ephemeral.

## If things go sideways

- **`gh auth status` says not logged in:** Run `gh auth login`, pick GitHub.com, HTTPS, authenticate via browser. Then re-try step 2.
- **`gh repo create` fails with "name already exists":** Pick a different name like `startupreneur-pilot-g1l01` and update the rest of the steps accordingly. Tell the user the name you picked.
- **Pages returns 404 forever:** Check Settings → Pages in the GitHub UI; confirm source is `main` branch + `/` root. Also confirm the file is named `index.html` (lowercase) at repo root.
- **Images load but font doesn't:** Check that `fonts/` was committed (sometimes `.gitignore` weirdness skips it). The site will still work — Google Fonts fallback in the CSS will load Andika from the CDN.
- **YouTube embed shows "Video unavailable":** That's a content issue on the YouTube side, not a deploy issue. Note it but don't try to fix it.

## Memory hooks for Claude Code

Add a project memory after success along the lines of:
- "G1 L01 deployed to GitHub Pages on YYYY-MM-DD at <url> as a deploy pipeline pilot. Public for testing; private LMS migration pending."

That's it. The pilot is small, the dependencies are mapped, the goal is one URL. Should be a 10-minute task end-to-end.
