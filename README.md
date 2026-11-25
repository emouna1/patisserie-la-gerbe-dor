# Pâtisserie la Gerbe d'Or — static website

A self-contained static website (one HTML page + an `images/` folder). No build step, no
dependencies, no server-side code. Upload the folder as-is and it works.

```
patisserie-la-gerbe-dor/
├── index.html
├── images/
│   ├── hero.jpg
│   ├── maison.jpg
│   ├── patisseries.jpg
│   ├── croissants.jpg
│   ├── salon.jpg
│   ├── baker.jpg
│   └── map.png
└── README.md
```

All image references are **relative** (`images/hero.jpg`, `images/map.png`, …), so the site
works from any directory, any sub-path, and any host. The only external requests are the
Google Fonts stylesheet and the Google Maps / OpenStreetMap links — the page still renders
correctly if those are blocked (it falls back to system fonts).

---

## 1. Upload it to a host

### Netlify (easiest, free)

**Option A — drag & drop**
1. Go to <https://app.netlify.com/drop>.
2. Drag the **`patisserie-la-gerbe-dor` folder** (not the zip) onto the page.
3. Netlify gives you a URL like `https://random-name-123.netlify.app` — the site is live.
4. Rename it under *Site configuration → Change site name*.

**Option B — from a zip**
1. Log in to Netlify → *Sites → Add new site → Deploy manually*.
2. Drop the **zip file** itself. Netlify unzips it automatically.
   ⚠️ The zip must contain `index.html` at its **root**, not inside a sub-folder.

**Option C — Git**
Push the folder contents to a GitHub/GitLab repo, then *Add new site → Import an existing
project*. Build command: **leave empty**. Publish directory: **`/`** (or the folder holding
`index.html`).

### Vercel (free)

1. Install the CLI: `npm i -g vercel`
2. From inside the `patisserie-la-gerbe-dor` folder run `vercel`.
3. Accept the defaults — Vercel detects a static site. Framework preset: **Other**,
   build command: **empty**, output directory: **`.`**
4. `vercel --prod` to publish to your production URL.

Or via the dashboard: push to GitHub → *Add New → Project* → import the repo → Framework
preset **Other**, no build command, output directory `.`.

### GitHub Pages (free)

1. Create a repository, e.g. `gerbe-dor-site`.
2. Upload `index.html` and the `images/` folder to the **root** of the `main` branch
   (drag & drop on the GitHub web UI works).
3. *Settings → Pages → Source:* **Deploy from a branch**, Branch: **main**, Folder: **/ (root)**.
4. Save. After a minute the site is at `https://<username>.github.io/gerbe-dor-site/`.

> Relative paths mean it works fine from that `/gerbe-dor-site/` sub-path — no changes needed.

### Classic web host (cPanel, FTP, OVH, Hostinger, Infomaniak…)

1. Connect over FTP/SFTP or open *File Manager* in cPanel.
2. Upload the **contents** of `patisserie-la-gerbe-dor` into `public_html/`
   (or `www/`, or `htdocs/` — whatever your host uses as the web root).
   The result must be `public_html/index.html` and `public_html/images/…`.
3. Set permissions: files `644`, folders `755`.
4. Visit your domain — done.

> If you want the site in a sub-folder instead (e.g. `example.com/boutique/`), upload the
> folder there; the relative paths keep working.

---

## 2. Point a domain at it

### On Netlify
*Domain management → Add a domain* → enter `gerbe-dor.tn` (or your domain) → follow the
instructions. Two ways:

- **Netlify DNS (recommended):** change your registrar's nameservers to the four Netlify
  nameservers shown. Everything (apex + `www`) is configured for you, HTTPS included.
- **External DNS:** at your registrar create
  - `A` record, host `@` → the Netlify load-balancer IP shown in the panel
  - `CNAME` record, host `www` → `<your-site>.netlify.app`

HTTPS is issued automatically (Let's Encrypt) once DNS resolves.

### On Vercel
*Project → Settings → Domains → Add*. Then at your registrar:
- apex `@` → `A` record to `76.76.21.21`
- `www` → `CNAME` to `cname.vercel-dns.com`

### On GitHub Pages
*Settings → Pages → Custom domain* → enter the domain → Save. At your registrar:
- apex `@` → `A` records to `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
  `185.199.111.153`
- `www` → `CNAME` to `<username>.github.io`

Tick **Enforce HTTPS** once the certificate is issued.

### On a classic host
In your hosting panel (*Domains → Add domain* or *Parked domains*), attach the domain to the
account, then at the registrar point the domain's nameservers to the host's, or set an `A`
record to the server IP. Enable the free SSL certificate in the panel (AutoSSL / Let's Encrypt).

### DNS propagation
Changes usually apply in minutes but can take up to 24–48 h. Check with
`dig gerbe-dor.tn +short` or <https://dnschecker.org>.

---

## 3. Editing the content

Everything is in `index.html` — plain HTML with an inline `<style>` block and a small inline
`<script>` (mobile menu + scroll reveal). Open it in any text editor.

- **Prices** are in Tunisian dinars (DT) and are currently *indicative* — search for `DT` in
  the file and replace the amounts with the real ones.
- **Address** appears in the hero, the intro, the "Nous Trouver" section, the footer and the
  `<meta name="description">`. Search for `Béja` to find every occurrence.
- **Map** is a static image (`images/map.png`). To change the location, regenerate it from
  OpenStreetMap and replace the file, then update the coordinates in the two links
  (`openstreetmap.org` and `google.com/maps`) near the end of the file.
- **Hours and phone** are intentionally not invented — add them once confirmed.

---

## 4. Notes

- No tracking, no cookies, no analytics, no third-party badge or branding.
- The page is responsive (desktop / tablet / mobile) and has no JavaScript dependency for
  content — the menu and reveal animations degrade gracefully.
- Total size: under 1 MB, so it loads fast on mobile networks.
