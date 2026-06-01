# Drive Dashboard

A single-page web app to visualize and manage sharing permissions across your entire Google Drive.

![HTML](https://img.shields.io/badge/HTML-single%20file-blue) ![Google Drive API](https://img.shields.io/badge/Google%20Drive-API%20v3-green)

---

## Features

- **Browse all your Drive files** — files, folders, docs, sheets, slides, PDFs, images, and more
- **See sharing permissions** — who has access, their role (Owner / Editor / Viewer / Commenter), and visibility (Private / Shared with people / Anyone with link)
- **Filter & search** — filter by file type, owner, visibility, or shared-with; full-text search across names and emails
- **Clickable stat cards** — click Total Files, Private, Publicly Shared, or Shared with People to instantly filter the table
- **Manage sharing** — add people, change roles, or remove access directly from the dashboard
- **Bulk operations** — select multiple files and share or remove access in one action
- **Full folder paths** — see the complete breadcrumb path (My Drive › folder › subfolder) with a clickable link to open the folder
- **Auto sync** — when you return to the tab after 30+ seconds, it silently fetches only what changed in Drive (no full reload)
- **Fast two-phase loading** — file metadata appears immediately; permissions load in the background

---

## Setup

### 1. Create a Google Cloud project

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project (or select an existing one)
3. Go to **APIs & Services → Library** and enable the **Google Drive API**
4. Go to **APIs & Services → OAuth consent screen**
   - Choose **External**
   - Fill in app name, support email, and developer email
   - Add the scope: `https://www.googleapis.com/auth/drive`
   - Add your Google account as a **test user**
5. Go to **APIs & Services → Credentials → Create Credentials → OAuth 2.0 Client ID**
   - Application type: **Web application**
   - Add your site URL to **Authorized JavaScript origins** (e.g. `https://yourusername.github.io`)
   - Copy the **Client ID**

### 2. Add your Client ID

Open `index.html` and replace the Client ID near the top of the `<script>` block:

```js
const CLIENT_ID = "YOUR_CLIENT_ID_HERE.apps.googleusercontent.com";
```

### 3. Deploy to GitHub Pages

1. Push `index.html` to a GitHub repo
2. Go to **Settings → Pages**
3. Set source to **Deploy from branch → main → / (root)**
4. Your dashboard will be live at `https://yourusername.github.io/repo-name`

> Make sure to add your GitHub Pages URL to the **Authorized JavaScript origins** in Google Cloud Console.

---

## Local development

No build step needed. Just open `index.html` directly in a browser — but note that Google OAuth won't work over `file://`. Use a simple local server instead:

```bash
npx serve .
# or
python3 -m http.server 8080
```

Then add `http://localhost:8080` to your Authorized JavaScript origins in Google Cloud Console.

---

## Tech stack

| Layer | Tech |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (no frameworks) |
| Auth | Google Identity Services (GIS) OAuth 2.0 |
| Data | Google Drive API v3 |
| Sync | Drive Changes API (incremental, token-based) |
| Fonts | Geist + DM Serif Display (Google Fonts) |
| Hosting | GitHub Pages |

---

## How it works

1. **Sign in** with your Google account — grants read/write access to Drive metadata and permissions
2. **Phase 1** — fetches all file metadata in batches of 1000, renders immediately as files arrive
3. **Phase 2** — fetches permissions for each file in background batches of 50
4. **Auto sync** — saves a Changes API token after load; on tab focus after 30s, fetches only changed files and patches them in place
