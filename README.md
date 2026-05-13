# Startupreneur — GitHub Pages pilot (G1 L01)

Pilot deployment of a single lesson (Grade 1, Lesson 01) to verify the GitHub Pages workflow before publishing the full course.

## What's in here

```
github-pilot-g1-l01/
├── index.html                       ← landing page, redirects to the lesson
├── G1_Lesson_01_v2.1.html          ← the lesson
├── lesson_v3.css                    ← shared lesson stylesheet
├── fonts/                           ← Andika webfont (4 woff2 files)
└── images/                          ← 4 PNGs referenced by the lesson
```

External dependencies (loaded from CDN, nothing to ship):
- Google Fonts (Andika)
- YouTube embed (Aling Nene's Sari-Sari Store video)

## Total size

~5 MB. Well under GitHub Pages limits (1 GB repo, 100 MB per file).

## Deploying

See the deployment walkthrough provided alongside this pilot. Short version:
1. Create a new public repo on GitHub
2. Push this folder's contents to it
3. Enable GitHub Pages in repo Settings → Pages (source: `main` branch, `/` root)
4. Visit the live URL after a few minutes

## Next steps after pilot

- Add remaining G1 lessons (L02–L12)
- Add frontmatter / backmatter
- Build a TOC in `index.html`
- Eventually: migrate to private hosting once LMS integration is ready
