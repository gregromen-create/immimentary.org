# Immimentary.org

Website for **Immimentary** (immigrant + documentary), an archive of immigrant stories produced in Brooklyn.
Live domain: **https://immimentary.org**

The site is a single static page. There is no build step.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site (styles, script and the studio video are embedded) |
| `episodes.json` | Extra episodes list read by the page (`[]` = none beyond the built-in ones) |
| `studio-walk.mp4` | Standalone copy of the studio video (already embedded in `index.html`) |
| `_headers` | Security headers for Netlify / Cloudflare Pages |
| `.htaccess` | Security headers + HTTPS redirect for Apache hosts (GoDaddy, Bluehost, Hostinger…) |
| `SECURITY.md` | What the page protects against and the account checklist |
| `docs/` | Notes (Higgsfield video prompt); not needed on the server |

## Preview locally

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy to immimentary.org

Pick **one** host.

### Option A: Cloudflare Pages (recommended, free)
1. Cloudflare dashboard → Workers & Pages → Create → Pages → **Connect to Git** → pick this repo.
2. Build command: *(empty)*. Output directory: `/`.
3. Deploy, then **Custom domains** → add `immimentary.org` and `www.immimentary.org`.
4. `_headers` is applied automatically. Every push to `main` redeploys.

### Option B: Netlify
1. Add new site → Import from Git → pick this repo.
2. Build command: *(empty)*. Publish directory: `.`.
3. Domain management → add `immimentary.org`, follow the DNS steps.
4. `_headers` is applied automatically.

### Option C: Apache shared hosting (GoDaddy, Bluehost, Hostinger…)
1. Open the host's File Manager (or SFTP) and go to `public_html/`.
2. Upload `index.html`, `episodes.json`, `studio-walk.mp4` and `.htaccess`.
3. Turn on the host's free SSL certificate for `immimentary.org`. `.htaccess` then forces HTTPS.

### DNS
Point the domain at the host you chose (the host shows the exact records). For Cloudflare Pages, moving the domain's nameservers to Cloudflare is simplest.

## After the first deploy
- Open the site and send a test message from the Story Box. FormSubmit emails a confirmation link to the inbox address once; click it.
- Replace the email in `index.html` (`formsubmit.co/ajax/…`) with the random alias FormSubmit gives you, so the address isn't in the page source.
- Turn on two-factor authentication for hosting, registrar, GitHub, YouTube, Patreon and email accounts. See `SECURITY.md`.

## Editing the page
`index.html` has a Content-Security-Policy that allows only its own script, matched by a SHA-256 hash in three places: the CSP `<meta>` tag in `index.html`, `_headers` and `.htaccess`. **If you change the `<script>` in `index.html`, update the hash** in all three, or the page's script will be blocked:

```sh
python3 - <<'EOF'
import re, hashlib, base64
s = open('index.html', encoding='utf-8').read()
for m in re.finditer(r'<script(?![^>]*\bsrc=)[^>]*>(.*?)</script>', s, re.S):
    print("'sha256-" + base64.b64encode(hashlib.sha256(m.group(1).encode()).digest()).decode() + "'")
EOF
```
