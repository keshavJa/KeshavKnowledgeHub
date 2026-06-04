# Keshav Jain — Salesforce Knowledge Hub

A single‑file personal website + personal knowledge base for a Salesforce Architect. It holds four things, each with its own configurable topic tree:

- **Interview Q&A** — questions grouped by Topic → Sub‑topic, with difficulty tags and rich Markdown answers.
- **Blogs** — article cards with cover images (or auto‑generated gradient covers) and full Markdown bodies.
- **Notes** — a Confluence‑style notebook (callouts, tables, code blocks, banners).
- **Why Hire Me** — a designed resume page with a one‑click **PDF export** (the file name is configurable).

Everything lives in one file — `index.html` — no build step. It runs two ways:

- **Local mode (zero setup):** open the file and start using it. Edits save to that browser only.
- **Cloud mode (recommended, ~5‑min one‑time setup, free):** every admin **Save** is published **live to all visitors on every device** — no export/import, ever. See §3.

---

## 1. Quick start

Double‑click **`index.html`** (or right‑click → Open With → your browser). It loads with all of Keshav's seed content already in place. Out of the box it's in **local mode**.

---

## 2. Admin vs Guest

| | Guest (default) | Admin |
|---|---|---|
| Read everything | yes | yes |
| Export resume to PDF | yes | yes |
| Add / edit / delete content | no | yes |
| Manage menus & topic trees | no | yes |
| Edit the resume & site settings | no | yes |

Click **Admin Login** (top right) to switch to Admin.

In **local mode**, the default login is:

```
User ID:   admin
Password:  Salesforce@123
```

WARNING: change it before publishing a local‑mode site (Site settings → the gear icon). In **cloud mode**, login is your Firebase email/password instead (see §3) and this in‑app password is ignored.

---

## 3. Make every save sync everywhere (Firebase — free, one‑time)

This is the part that removes export/import completely. You connect the site to **Firebase** (Google's free backend). After this, you log in, edit, hit **Save**, and the change is instantly live for everyone, on every device.

> **Why a backend is required:** "available to everyone, on every device" means the data has to live on a shared server — a browser's local storage is private to that one browser. Firebase's free **Spark** tier is the no‑server, no‑cost way to do this (no credit card needed).

### One‑time setup (~5 minutes)

1. **Create a project.** Go to https://console.firebase.google.com → **Add project**. Give it any name. (You can skip Google Analytics.)

2. **Register a web app.** On the project overview, click the **`</>`** (Web) icon → **Register app** (nickname can be anything; you don't need Hosting). Firebase shows you a `firebaseConfig` object — keep that tab open.

3. **Paste the config into the file.** Open `index.html` in any text editor. At the **very top of the `<script>`** there's a block called `CLOUD`. Set `enabled: true` and copy your values in:

   ```js
   const CLOUD = {
     enabled: true,                       // turn it on
     config: {
       apiKey:            "AIza...",       // from your firebaseConfig
       authDomain:        "your-app.firebaseapp.com",
       projectId:         "your-app",
       storageBucket:     "your-app.appspot.com",
       messagingSenderId: "1234567890",
       appId:             "1:1234567890:web:abc123"
     },
     docPath: "knowledgeHub/site"          // leave as-is (must match the rule below)
   };
   ```

4. **Create the database.** Firebase console → **Build → Firestore Database → Create database** → **Start in production mode** → pick a location → Enable.

5. **Set the security rules.** In Firestore → **Rules** tab, replace everything with this and click **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /knowledgeHub/site {
         allow read: if true;                    // anyone can view your published content
         allow write: if request.auth != null;   // only your signed-in admin can change it
       }
     }
   }
   ```

6. **Turn on email login.** Console → **Build → Authentication → Get started → Sign‑in method →** enable **Email/Password →** Save.

7. **Create your admin user.** Authentication → **Users** tab → **Add user** → enter the email + password you'll log in with. *This is now your admin login.*

8. **Done.** Save `index.html`, deploy it (§4) or just open it, click **Admin Login**, and sign in with that email/password. Edit anything, hit **Save** — it's live for every visitor immediately. The first save automatically creates and seeds your cloud content.

### Good to know about cloud mode
- The `config` values (including `apiKey`) are **safe to publish** in the file — they're identifiers, not secrets. Your content is protected by the rules: only your authenticated admin account can write; everyone else is read‑only.
- Free Spark limits (about 50k reads / 20k writes per day) are far beyond what a personal site needs.
- Backups still work: Site settings → **Download backup** saves a JSON snapshot; **Restore backup** overwrites the live cloud copy (handy for rollbacks or migrating).
- **Troubleshooting:** "Cloud is on but not configured" → you left a `PASTE_...` placeholder. "Could not reach the cloud" → check internet / the config values. A save that says *"cloud write failed — are you signed in?"* → re‑check the Rules (step 5) and that you're logged in with the Firebase user (step 7).

---

## 4. Deploy it for free

Static files, so any free host works. Put **`index.html`** at the root.

- **GitHub Pages:** upload `index.html` to a repo → Settings → Pages → deploy from `main` / root. Live at `https://<user>.github.io/<repo>/`.
- **Netlify:** app.netlify.com → drag the folder onto the deploy area → instant `*.netlify.app` URL.
- **Vercel:** vercel.com → Add New → Project → import the repo → deploy (no build settings needed).

After deploying, open the live URL and sign in. In cloud mode your content is already there; in local mode, import your backup once.

---

## 5. Editing content (Admin)

When signed in you'll see:

- A **+ Add** floating button (bottom‑right) for the section you're viewing.
- **add / edit / delete** controls on each section and item.
- **Manage topics** (bottom of the sidebar) — add, rename, reorder, delete topics & sub‑topics.
- **+ Menu** (top nav) — add, rename, re‑icon, reorder, show/hide, delete menus.
- The gear icon → **Site settings** — branding, login/backup.

Editors take **Markdown** with a live preview and a toolbar (headings, bold, lists, callouts, code, tables, links). You can also **drop a PDF or DOCX** in and its text is extracted in your browser and dropped into the editor — nothing is uploaded.

The resume's PDF **file name** is set in *Edit resume → Export file name*.

---

## 6. Libraries (loaded from public CDNs)

`marked` (Markdown), `DOMPurify` (sanitising), `pdf.js` (PDF text extraction), `mammoth` (DOCX text extraction), `html2pdf.js` (resume → PDF), and — only in cloud mode — the **Firebase** App / Firestore / Auth SDKs (loaded on demand from gstatic). An internet connection is needed for these to load. No tracking, no ads, no cost.
