# VIP Telecom — Virtual Catalogue

A single-file product catalogue with Firebase Firestore backend and GitHub Pages hosting.

---

## 🚀 Hosting on GitHub Pages

### Step 1 — Create a GitHub repository

1. Go to [github.com](https://github.com) → **New repository**
2. Name it something like `vip-telecom` (or any name you like)
3. Set it to **Public**
4. Click **Create repository**

### Step 2 — Upload your files

**Option A — GitHub website (easiest)**
1. In your new repo, click **Add file → Upload files**
2. Drag and drop `index.html` (and this `README.md` if you want)
3. Click **Commit changes**

**Option B — Git command line**
```bash
git init
git add index.html README.md
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

### Step 3 — Enable GitHub Pages

1. In your repo, go to **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Choose branch: `main`, folder: `/ (root)`
4. Click **Save**
5. After ~1 minute, your site will be live at:
   `https://YOUR_USERNAME.github.io/YOUR_REPO/`

---

## 🔥 Firebase Setup (required for products to save)

### Step 1 — Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → Name it `vip-telecom` → Continue
3. Disable Google Analytics → **Create project**

### Step 2 — Enable Firestore

1. Left sidebar → **Build → Firestore Database → Create database**
2. Choose **Start in test mode** → Select a region (e.g. `asia-south1`) → **Enable**

### Step 3 — Register your web app

1. Project Overview → click the **`</>`** (Web) icon
2. Register with nickname `vip-telecom` → skip Hosting → **Continue to console**
3. You'll see your `firebaseConfig` — copy it

### Step 4 — Paste config into index.html

Open `index.html` in a text editor and find these lines (around line 360):

```js
const firebaseConfig = {
  apiKey:            "AIzaSy...",
  authDomain:        "...",
  projectId:         "...",
  storageBucket:     "...",
  messagingSenderId: "...",
  appId:             "..."
};
```

Replace the values with your own config. Save the file, then re-upload to GitHub.

### Step 5 — Set Firestore security rules (important!)

Test mode expires after 30 days. Set proper rules:

1. Firestore → **Rules** tab → replace with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Anyone can read products (public catalogue)
    match /products/{id} {
      allow read: if true;
      // Only allow writes from your domain (update YOUR_DOMAIN)
      allow write: if request.auth == null; // or restrict by origin
    }
  }
}
```

For a simple public catalogue with no auth, you can use:
```
allow read, write: if true;
```
(Fine for personal use, but anyone could modify your data.)

---

## ⚠️ Image Hosting Note

Firebase Firestore **cannot store image files** (base64). Use image URLs instead:

- Upload images to [imgbb.com](https://imgbb.com), [cloudinary.com](https://cloudinary.com), or [imgur.com](https://imgur.com)
- Copy the direct image URL (ending in `.jpg`, `.png`, etc.)
- Paste the URL into the **Product Images** field in the Admin panel

---

## 🔐 Admin Access

Default password: `admin123`

**Change it** — open `index.html`, find this line and update it:
```js
const ADMIN_PASS = "admin123"; // ← Change this!
```

---

## 📦 Features

- Real-time sync with Firebase Firestore
- Add / edit / delete products from Admin panel
- Toggle stock status (In Stock / Out of Stock) instantly
- Filter by category and stock status
- Search by name, ID, or category
- Multi-image gallery with lightbox
- YouTube video embed per product
- Copy Product ID to order via Facebook
- Responsive mobile layout
- Falls back to localStorage if Firebase is not configured
