# CLAUDE.md

Guidance for working in this repository.

## What this repo is

- Personal portfolio site for Saehun Sean Oh, deployed via **GitHub Pages** at
  `seanoh1989.github.io`. This is a **user site**, so `master` is production — anything
  pushed goes live.
- **`index.html` is NOT hand-written HTML.** It is a compiled, self-unpacking "DC"
  artifact bundle. A bootstrap script decodes gzip+base64 assets from a
  `<script type="__bundler/manifest">` tag and renders the real page from a JSON-encoded
  string inside `<script type="__bundler/template">` (one very long line). The real
  source is a React-rendered component using `<x-dc>`, `<sc-for>`, `<sc-if>`,
  `style-hover`, and `{{ }}` bindings, with a `class Component extends DCLogic` data block
  at the bottom.

## Editing the site

- To read the real source, extract the `<script type="__bundler/template">` content and
  JSON-decode it.
- Edit **surgically** with exact string replacement inside the bundle — do not try to
  rewrite the file by hand. In the encoded blob, newlines are `\n`, double quotes are
  `\"`, and `/` inside closing tags is escaped (`/`).
- After any edit, re-decode the template, confirm it still parses as JSON, and confirm the
  manifest tag is intact.
- **Verify rendered changes in a real browser before committing** — the bundle needs
  `DecompressionStream` and blob URLs, so it only works in a browser. Serve the folder with
  a static server and load the page.
- Keep the `<noscript>` fallback block, the `<title>`, meta description, and JSON-LD in
  sync with the visible content.
- Keep the resume link's filename and the actual PDF committed in the repo in sync.

## Non-negotiables

1. **Accessibility is the #1 concern.** Every change must be audited: readable font sizes,
   screen-reader support, alt text where needed, correct ARIA roles/labels, sufficient
   color contrast, keyboard and other non-pointer input, and visible focus states.
2. **Fully responsive** across mobile, tablet, and desktop. Test all breakpoints.
3. **Never commit or expose API keys or secrets.** Any client-side call must be keyless.
4. **Be skeptical of heavy external scripts/files.** Do not blindly add large third-party
   dependencies that slow the page; justify and measure anything non-trivial.

## Additional guidance

- Any external/API call must be **CORS-enabled and keyless**, and the page must **degrade
  gracefully** if it fails (e.g. the world-time fetch only corrects a value already
  computed locally, so a failure is invisible).
- Don't rely solely on `mailto:` / `tel:` for contact CTAs — they do nothing when the
  visitor has no registered handler. Provide a fallback (the email buttons also copy the
  address to the clipboard with visible feedback).
- Links that open a new tab (`target="_blank"`) must include `rel="noopener noreferrer"`.
  Internal actions like `mailto:` / `tel:` should stay in the same tab (`_self`) so they
  don't spawn empty tabs.
- This is a static deploy with no build step. Prefer keeping it that way.
