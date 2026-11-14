# Immimentary website security

## Built into the page
- **No code injection:** everything that comes from visitors, the story inbox or the episode database is shown as plain text. Links to YouTube are rebuilt from a checked video ID, never taken from stored data.
- **Strict Content Security Policy** (hosted copy): the browser runs only the page's own script, matched by a fingerprint. It loads images only from YouTube thumbnails, fonts only from Google Fonts, and sends form data only to FormSubmit. Plugins, other scripts and redirecting the form are blocked.
- **No inline event handlers:** images fall back through one checked listener instead.
- **Story Box spam protection:** a hidden trap field for bots, a block on forms sent within 4 seconds of opening the page, one message per minute, length limits on every field, and a check that the email or phone looks real.
- **Episode Manager:** only the producer can add or remove episodes, enforced by the database rules on the Claude-hosted version. Visitors can read episodes. Only the producer can read the story inbox.
- **Alice:** she has read-only tools, ignores instructions hidden in data, never reveals inbox contact details, is limited to 6 helpers per chat, and runs one request at a time.
- **External links** open with `rel="noopener"`, so other sites can't control this tab.

## When you host it on immimentary.org
1. Upload `index.html` plus **one** of these, depending on your host:
   - `_headers` for Netlify or Cloudflare Pages
   - `.htaccess` for Apache hosts (GoDaddy, Bluehost, Hostinger and similar)
2. These add HTTPS-only (HSTS), clickjacking protection, no MIME sniffing, a strict referrer policy, and camera, microphone and location turned off.
3. Turn on **two-factor authentication** for your hosting, domain registrar, YouTube, Patreon, Higgsfield, Gmail and Claude accounts. Most site break-ins start with a stolen password, not the code.
4. In FormSubmit, replace `immiland@gmail.com` with the random alias FormSubmit gives you after the first confirmation email. That keeps your address out of the page source.
5. Optional: put the domain behind Cloudflare's free plan for DDoS protection and bot filtering.

## What no website can promise
No site is "hacker-proof". These measures close the common attacks: cross-site scripting, clickjacking, form spam, injected scripts and data tampering. Keeping your accounts secure (step 3) matters as much as the code.
