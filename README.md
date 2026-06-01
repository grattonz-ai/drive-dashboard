# Drive Dashboard

A web app to visualize and manage sharing permissions across your entire Google Drive — see every file, who has access, and manage it all in one place.

**[Live app →](https://grattonz-ai.github.io/drive-dashboard/)**

---

## Features

- Browse all your Drive files — docs, sheets, slides, folders, PDFs, images, and more
- See sharing permissions — who has access and whether it's Private, shared with specific people, or Anyone with the link
- Filter & search — by file type, owner, visibility, or search by name/email
- Manage sharing — add people, change roles (Editor / Viewer / Commenter), or remove access
- Bulk actions — select multiple files and share or remove access in one go
- Auto sync — returns to the tab and quietly picks up any changes from Drive

---

## Self-hosting setup

To run your own copy you need a Google Cloud project with a Client ID. Each deployment needs its own.

### 1. Create a Google Cloud project

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project → give it any name
3. Go to **APIs & Services → Library** → search **Google Drive API** → **Enable**

### 2. Configure the OAuth consent screen

1. Go to **APIs & Services → OAuth consent screen**
2. Choose **External** → **Create**
3. Fill in app name, support email, and developer contact email
4. On the Scopes step → **Add or Remove Scopes** → add `https://www.googleapis.com/auth/drive` → **Update**
5. On the Test Users step → add your Gmail address
6. Save and continue through to the end

### 3. Create credentials

1. Go to **APIs & Services → Credentials → + Create Credentials → OAuth 2.0 Client ID**
2. Application type: **Web application**
3. Under **Authorized JavaScript origins** add your site URL (e.g. `https://yourusername.github.io`)
4. Click **Create** → copy your **Client ID**

### 4. Add your Client ID to the code

Open `index.html` and find this near the top of the `<script>` block:

```js
const CLIENT_ID = window.location.hostname === 'rdesa020.github.io'
  ? '...'
  : '...';
```

Replace it with:

```js
const CLIENT_ID = 'YOUR_CLIENT_ID_HERE.apps.googleusercontent.com';
```

### 5. Deploy to GitHub Pages

1. Push `index.html` to a GitHub repo
2. Go to **Settings → Pages → Deploy from branch → main → / (root)**
3. Your app will be live at `https://yourusername.github.io/repo-name`

> Make sure your GitHub Pages URL is listed under **Authorized JavaScript origins** in your Google Cloud credentials.

---

## Privacy

This app runs entirely in your browser. Your files and Google credentials are never sent to any server — all Drive API calls go directly from your browser to Google.
