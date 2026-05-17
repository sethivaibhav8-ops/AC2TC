# AC2TC → Test Cases Generator

A single-page web app that turns Acceptance Criteria into a structured 6-column
test-case table (TC ID · TestcaseName · Objective · Pre-Conditions · Steps ·
Expected Result), powered by your own LLM API key (Anthropic Claude, Google
Gemini, or Groq).

- **No backend** — everything runs in your browser.
- **No build step** — one HTML file, ~120 KB, zero dependencies.
- **Bring your own key** — keys are stored only in your browser's
  `localStorage`. They're sent directly to the LLM provider, never to any
  third party.

---

## Live deployment

Once published on GitHub Pages this app lives at:

```
https://<your-github-username>.github.io/<repo-name>/
```

A push to `main` is auto-deployed by the included GitHub Actions workflow.

---

## Access

The app is gated by a hard-coded team login screen. Default credentials:

```
Username : QATC1234
Password : QATC1234
```

> **Note:** this is a UX lock, not network security — the credentials are
> visible to anyone who opens DevTools or reads `index.html`. To change them,
> search for `QATC1234` inside `index.html` and update both occurrences (one
> in the `AUTH` IIFE constants, one — optional — in the `?bypass=` escape
> hatch).

Once signed in, the session is held in `sessionStorage` so users don't get
re-prompted within the same tab. A **↪ Sign out** button in the header clears
the session.

For automated testing or trusted iframe embeds, append
`?bypass=QATC1234` to the URL to skip the modal.

---

## Features

- **Multiple LLM providers** — Anthropic Claude (Opus 4.7, Sonnet 4.5, Sonnet
  3.7, Haiku 3.5), Google Gemini (2.5 Pro, 2.5 Flash, Flash Lite), and Groq
  (Llama 3.3, Llama 4, DeepSeek R1) on the free tier.
- **Vision support** — upload, paste, or drag-drop reference screenshots
  (Figma exports, UI mockups). The model uses them as visual context for
  step-level and expected-result phrasing.
- **Test-type & domain selectors** — Functional / SIT / E2E / Performance /
  Security · None / SCC B2C / SCC B2B / Mobile / API.
- **Custom instructions** — free-form prompt tweaks layered on top of the
  built-in QA contract.
- **Inline edit mode** — click cells to edit, add or remove paired
  Steps↔Expected rows, then **Save** / **Revert**. Auto-renumbers TC IDs on
  add/delete.
- **Frozen table header** — the column header stays visible during scroll;
  rows scroll cleanly behind it without bleed-through.
- **Stop button** — cancel an in-flight generation mid-stream (real
  AbortController, no zombie request).
- **Refine** — give the model targeted feedback to regenerate the table with
  edits applied (e.g. "add more boundary tests", "make mobile-specific").
- **Export** — copy as Excel-friendly TSV or HTML to clipboard, or download as
  CSV or `.xlsx`.

---

## Quick start (no GitHub needed)

Just open `index.html` in any modern browser:

```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

That's it. Sign in with the credentials above, paste your API key in
**Settings ⚙️**, fill in Persona / User Story / Acceptance Criteria, and click
**Generate Test Cases**.

To try it without an API key, open `index.html?demo=1` — a sample table is
pre-loaded so you can play with Edit mode, export, etc.

---

## Local development

A static file server is recommended over `file://` because `localStorage`,
clipboard, and AbortController all behave better over HTTP:

```bash
# Python 3
python3 -m http.server 8000

# Node
npx serve .

# any other static server
```

Then visit <http://localhost:8000/>.

---

## Getting an API key

Pick whichever LLM you prefer; all three offer a free tier.

| Provider     | Sign up                                                        | Free tier?                                    |
| ------------ | -------------------------------------------------------------- | --------------------------------------------- |
| **Groq**     | <https://console.groq.com/keys>                                | Yes — generous free tier on Llama / DeepSeek. |
| **Gemini**   | <https://aistudio.google.com/app/apikey>                       | Yes — Gemini 2.5 Flash is free for personal use. |
| **Anthropic** | <https://console.anthropic.com/settings/keys>                 | $5 credit on signup; paid after.              |

Paste the key in **Settings ⚙️ → API Key**. It is stored only in your browser
and sent directly to the provider's API over HTTPS.

---

## Privacy

- Your API key, persona, user story, acceptance criteria, custom instructions
  and the last generated table are stored in **your browser's
  `localStorage`** — they never leave your machine except as part of the
  request sent directly to your chosen LLM provider.
- This repo serves the app from GitHub Pages — GitHub does not see any of
  your prompts, keys, or generated test cases.
- The login modal flag lives in `sessionStorage` and is cleared when you
  close the tab (or click **Sign out**).

---

## Deploy to GitHub Pages

1. Create a new public repo on GitHub.
2. Unzip this package into the repo root and push:

   ```bash
   git init
   git add .
   git commit -m "Initial commit: AC2TC public build"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```

3. In the GitHub UI: **Settings → Pages → Build and deployment → Source:
   GitHub Actions**.
4. The included workflow (`.github/workflows/pages.yml`) auto-deploys on
   every push to `main`. After the first run the URL appears in
   **Settings → Pages**.

You can also drag-and-drop the zip contents into a new repo via the GitHub
web UI if you prefer.

---

## Customisation

- **Change the team password**: search `QATC1234` in `index.html` (2 hits)
  and replace both.
- **Disable the login gate entirely**: in `index.html`, find the
  `if (!AUTH.isAuthed()) reveal(); else dismiss();` line and replace with
  `dismiss();` — and (optional) remove the `<div id="authBackdrop">` block.
- **Change branding**: edit the `<h1>` and `<div class="logo">` in `<header>`.
- **Add a default provider**: open Settings, pick provider + paste key, and
  it's remembered for next time.

---

## License

MIT — see [`LICENSE`](./LICENSE).

---

## Acknowledgements

- Anthropic, Google, and Groq for the LLM APIs.
- All the QA engineers whose written-by-hand test cases trained the underlying
  models 😄.
