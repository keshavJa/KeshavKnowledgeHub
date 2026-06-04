# Keshav Jain — Salesforce Knowledge Hub

A single‑file personal website + personal knowledge base for a Salesforce Architect. It holds four things, each with its own configurable topic tree:

- **Interview Q&A** — questions grouped by Topic → Sub‑topic, with difficulty tags and rich Markdown answers.
- **Blogs** — article cards with cover images (or auto‑generated gradient covers) and full Markdown bodies.
- **Notes** — a Confluence‑style notebook (callouts, tables, code blocks, banners).
- **Why Hire Me** — a designed resume page with a one‑click **PDF export** (the file name is configurable).

Everything is in one file — `index.html` — with no build step and no server. It works opened straight from your computer or deployed to any static host.

---

## 1. Quick start (open it locally)

Just double‑click **`index.html`** (or right‑click → Open With → your browser). That's it. The site loads with all of Keshav's seed content already in place.

> Your edits are saved automatically in **that browser's local storage**. They persist between visits on the same browser/computer, but they are not shared to other people or devices until you publish (see §4).

---

## 2. Admin vs Guest

The site has two modes:

| | Guest (default) | Admin |
|---|---|---|
| Read everything | ✅ | ✅ |
| Export resume to PDF | ✅ | ✅ |
| Add / edit / delete content | ❌ | ✅ |
| Manage menus & topic trees | ❌ | ✅ |
| Edit the resume & site settings | ❌ | ✅ |

Click **Admin Login** (top right) to switch to Admin.

**Default credentials**

```
User ID:   admin
Password:  Salesforce@123
```

⚠️ **Change these before you publish.** Log in, open **Site settings** (the gear icon), and set your own User ID and password under *Admin sign‑in*. Anyone who knows the demo password could otherwise edit a public copy of your site.

---

## 3. Editing content (Admin)

When signed in as Admin you'll see:

- A **＋ Add** floating button (bottom‑right) that adds an item to whatever section you're viewing.
- **＋** buttons on each section, plus edit/delete controls on every item.
- **Manage topics** at the bottom of the left sidebar — add, rename, reorder, or delete topics and sub‑topics for the current section.
- **＋ Menu** in the top navigation — add, rename, re‑icon, reorder, show/hide, or delete navigation menus.
- The gear icon → **Site settings** — branding, your admin login, and data backup.

Every editor accepts **Markdown**, with a live preview and a toolbar (headings, bold, lists, callouts, code blocks, tables, links). You can also **drop a PDF or DOCX** into the upload box and its text is extracted in your browser and dropped into the editor — nothing is uploaded anywhere.

---

## 4. Publishing & moving your content

Because the site stores data in your browser, "publishing" means moving that data to wherever the live copy lives.

1. On the browser where you've been editing, open **Site settings → Export data (.json)**. This downloads a single backup file — treat it as your master copy.
2. On the deployed site (or another computer), sign in as Admin, open **Site settings → Import data (.json)**, and select that file.

Importing **replaces** the live site's content with your backup. Export first if you might want the current version back.

---

## 5. Deploy it for free

The site is just static files, so any of these free options work. Put **`index.html`** at the root.

### Option A — GitHub Pages
1. Create a free GitHub repo and upload `index.html` (and this `README.md`).
2. Repo **Settings → Pages → Build and deployment**: Source = *Deploy from a branch*, Branch = `main`, folder = `/ (root)`.
3. Your site goes live at `https://<your-username>.github.io/<repo>/` within a minute or two.

### Option B — Netlify (drag‑and‑drop)
1. Sign in at app.netlify.com (free).
2. Drag the folder containing `index.html` onto the **"Deploy"** area.
3. You get an instant `https://<random-name>.netlify.app` URL (renameable in site settings).

### Option C — Vercel
1. Sign in at vercel.com (free), **Add New → Project**, import your GitHub repo (or use the Vercel CLI).
2. No framework / no build command needed — it's a static file. Deploy.

After deploying, open the live URL, log in as Admin, change your password, and **Import** your exported data (§4).

---

## 6. Good to know

- **Storage is per‑browser.** Editing in Chrome on your laptop won't automatically appear in Safari, on your phone, or for visitors. Use Export/Import to sync. Visitors always see whatever content was baked into the `index.html` they load (plus anything they personally change in their own browser, which only they see).
- **Resume PDF file name** is set in the resume editor (*Edit resume → Export file name*). The Export button on the resume uses it.
- **No tracking, no accounts, no backend, no cost.** All processing (including PDF/DOCX text extraction) happens locally in the browser.

### Libraries (loaded from public CDNs)
`marked` (Markdown), `DOMPurify` (sanitising), `pdf.js` (PDF text extraction), `mammoth` (DOCX text extraction), `html2pdf.js` (resume → PDF). An internet connection is needed for these to load.

---

## 7. Want shared, multi‑user content later? (optional upgrade)

This flat‑file approach is intentionally free and simple. If you later want edits made on one device to instantly appear for everyone (no Export/Import step), the natural upgrade is **Firebase** (Google) — its free *Spark* tier is generous:

- Use **Cloud Firestore** to store the same data object this app keeps in local storage.
- Use **Firebase Authentication** for the admin login instead of the built‑in user/password.
- On load, read the document from Firestore; on save, write back to it.

The data shape the app already uses (`site`, `menus`, `topics`, `questions`, `blogs`, `notes`, `resume`) maps cleanly onto a single Firestore document, so this is an incremental change rather than a rewrite.
