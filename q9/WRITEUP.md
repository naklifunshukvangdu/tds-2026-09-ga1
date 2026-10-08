# Q9. GitHub Pages (1)

**Exam id:** `q-github-pages`  
**Status:** Accepted. Saved 22/22.

## Requirements

A public GitHub Pages site whose HTML contains the email wrapped so Cloudflare Email Obfuscation does not rewrite it:

```html
<!--email_off--><course-email><!--/email_off-->
```

The live HTML must still contain those comments and must not contain `data-cfemail`.

## Files

- `q9/index.html` — copy of the published page
- `q9/WRITEUP.md`

Repo: https://github.com/<github-username>/tds-portfolio

## Packages

GitHub REST API (or the GitHub website). `gh` was not installed on this machine.

## Setup

1. Create public repo `tds-portfolio` (auto-init).
2. Put `q9/index.html` on `main` (commit message used: `Add portfolio index.html with email_off wrapped email`).
3. Enable Pages from `main` `/`.
4. Wait until Pages status is `built`. Confirm the `email_off` comments survive.

Do not store a GitHub token in this folder.

## Code

Contact block from `q9/index.html`:

```html
<div class="card">
  <h2>Contact</h2>
  <p>Email: <!--email_off--><course-email><!--/email_off--></p>
  <p>GitHub: <a href="https://github.com/<github-username>"><github-username></a></p>
</div>
```

## Accepted answer

```text
https://<github-username>.github.io/tds-portfolio/
```

If the grader sees a cached page, append `?v=1`.
