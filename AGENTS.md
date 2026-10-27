# AGENTS.md

Guidance for AI coding agents working with this repository – either **using** these snippets to build a form for a user, or **maintaining** the repository itself.

## What this repository is

Copy-paste HTML form snippets that submit to [Form.taxi](https://form.taxi), an EU-hosted, GDPR-compliant form backend. Submissions are delivered by email, webhook or integration (e.g. Zapier). No server-side code is needed.

- `snippets/` – one self-contained HTML file per form type
- `README.md` – human-facing overview and snippet table
- `llms.txt` – compact summary for LLMs

There is no build step, no dependencies and no test suite.

## Using a snippet for a user

1. Pick the snippet in `snippets/` that matches the user's need and copy it.
2. Set the form `action` to the user's endpoint: `https://form.taxi/s/FORM_CODE`.
   Never invent a `FORM_CODE`. If the user has none, see "Creating an endpoint" below.
3. Keep `method="POST"`.
4. Keep the `_gotcha` honeypot field and keep it hidden. It is the spam protection.
5. Set `_next` to the user's thank-you page, or remove it to use the default confirmation page.
6. For file uploads, keep `enctype="multipart/form-data"` on the `<form>`.
7. Keep every `<label for>` / `id` pair intact when restyling. Swap inline styles for the project's framework (Tailwind, Bootstrap, …) as needed.
8. Field `name` attributes become the field labels in the notification email – use readable names.

### Creating an endpoint

If the user has no Form.taxi account or endpoint yet:

- **Manually:** https://form.taxi/panel/create-endpoint
- **Via API:** `POST https://form.taxi/api/v1/create-endpoint`
  - Required: `email`, `form_name`, `language`
  - Optional: `website_url`, `timezone`
  - `201` → response contains `endpoint_url`; use it as the form `action`
  - `409` → an account with this email already exists; tell the user to log in and copy the endpoint from the panel
  - `422` / `400` / `415` → validation, content or content-type error
  - Full spec: https://docs.form.taxi/APIs/CreateEndpoint/

Only call the API with the user's own email address and with their consent – it creates an account in their name.

### Frameworks

- Static sites (GitHub Pages, Netlify, Vercel, Cloudflare Pages): paste the snippet directly.
- WordPress / CMS: use a Custom HTML block.
- React / Vue / Svelte: reuse the markup; for SPA behaviour submit via `fetch()` to the same endpoint.

### Privacy

Form.taxi processes submissions in the EU. If the user's site has a privacy policy, point them to the ready-made privacy notice text and the data processing agreement (AVV/DPA) in the Form.taxi panel under "Privacy".

## Maintaining this repository

When adding or changing a snippet:

- One form per file in `snippets/`, kebab-case file name ending in `-form.html` (exception: `newsletter-signup.html`).
- Start with the standard header comment: title, one-line description, setup steps, docs link.
- Use `https://form.taxi/s/FORM_CODE` as the placeholder action – never a real form code.
- Include `_gotcha` and an optional `_next`, each with a short comment. Refer to the redirect field as `_next` everywhere.
- Plain HTML only: no JavaScript unless the form type needs it (e.g. multi-step), no external dependencies, minimal inline styles.
- Accessible markup: every input has a `<label>`, use HTML5 validation (`required`, `type="email"`, …).
- Update the snippet table in `README.md` and the list in `llms.txt` in the same commit.
- Language: English for code, comments and docs.

## Links

- Website: https://form.taxi
- Documentation: https://docs.form.taxi
- Support: support@form.taxi
