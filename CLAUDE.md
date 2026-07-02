# CLAUDE.md

Guidance for working in this repository.

## What this repo is

- Personal portfolio site for Saehun Sean Oh, deployed via **GitHub Pages** at the custom
  domain **`saehunseanoh.me`** (see `CNAME`; the Pages origin is `seanoh1989.github.io`).
  This is a **user site**, so `master` is production — anything pushed goes live.
- **`index.html` is a single, hand-authored static page** — plain semantic HTML, one inline
  `<style>` block, and one inline vanilla-JS block at the end of `<body>`. No build step, no
  framework, no bundler. Edit it directly.
- Design: dark "terminal" aesthetic. Accent color is the CSS variable `--ac` (`#5CE585`).
  Fonts are Space Grotesk (body/headings) and JetBrains Mono (mono/labels), loaded from
  Google Fonts with a system-font fallback so the page still works if the CDN is unreachable.

### History (context, not instructions)

`index.html` used to be a compiled, self-unpacking "DC" artifact bundle that rendered the
page client-side with React. It was replaced with this static page because nothing on a
portfolio warrants client-side rendering, and the framework had a loop-binding bug that
corrupted the impact numbers. Do **not** reintroduce a bundler/framework.

## Editing the site

- Just edit `index.html` directly with normal HTML/CSS/JS. Content is inline, so update the
  markup in place.
- The small JS block handles five things only: years-of-experience (computed from the date,
  optionally corrected by a keyless CORS time fetch), count-up on the impact numbers,
  reveal-on-scroll, the mobile-nav toggle, and clipboard-copy on the email CTAs. Keep it
  minimal and dependency-free.
- **Reveal-on-scroll must never leave content hidden.** Elements start at `opacity:0` only
  under the `.js` html class and are revealed by an IntersectionObserver **plus** a
  timeout safety net. If you touch this, preserve a fallback that force-reveals everything.
- **Verify rendered changes in a real browser before committing.** Serve the folder with a
  static server (`python -m http.server`) and load the page. Note: automated preview
  environments may freeze CSS transitions / IntersectionObserver / rAF — verify end-states
  with computed styles (`transition:none`) rather than trusting a mid-transition reading.
- Keep the resume link's filename and the actual committed PDF (`RESUME_Saehun_Sean_Oh.pdf`)
  in sync. Keep `<title>`, meta description, Open Graph tags, and the JSON-LD block in sync
  with the visible content.

## Non-negotiables

1. **Accessibility is the #1 concern.** Every change must be audited: readable font sizes,
   screen-reader support, alt text where needed, correct ARIA roles/labels, sufficient
   color contrast, keyboard and other non-pointer input, and visible focus states.
2. **Fully responsive** across mobile, tablet, and desktop. Test all breakpoints (the page
   has `max-width:900px` and `max-width:620px` breakpoints; the body must never scroll
   horizontally).
3. **Never commit or expose API keys or secrets.** Any client-side call must be keyless.
4. **Be skeptical of heavy external scripts/files.** Do not blindly add large third-party
   dependencies that slow the page; justify and measure anything non-trivial. The only
   external request today is Google Fonts (keyless, and it degrades gracefully).

## Additional guidance

- Any external/API call must be **CORS-enabled and keyless**, and the page must **degrade
  gracefully** if it fails (e.g. the time fetch only corrects a value already computed
  locally, so a failure is invisible; the font CDN falls back to system fonts).
- Don't rely solely on `mailto:` / `tel:` for contact CTAs — they do nothing when the
  visitor has no registered handler. The email and phone CTAs also copy the value to the
  clipboard with a visible toast.
- Links that open a new tab (`target="_blank"`) must include `rel="noopener noreferrer"`.
  Keep `mailto:` / `tel:` in the same tab so they don't spawn empty tabs.
- Prefer keeping this a static, no-build deploy.
