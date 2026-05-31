# 🌐 Favicon — What It Is & How to Add It

**Date:** 2026-05-31  
**Topic:** Favicon creation and integration in a Vite + React project

---

## 📌 What is a Favicon?

A **favicon** is the small icon that appears on the browser tab next to the page title.

```
🟠 V.  Vivek | Portfolio   ← that small icon is the favicon
```

Every website has one — GitHub, Claude, LinkedIn all have their own.

---

## 🛠️ How to Create a Favicon

**Website:** [https://favicon.io](https://favicon.io)

### Steps:
1. Go to favicon.io → click **"Generate From Text"**
2. Configure:
   - **Text** → `V.`
   - **Font Color** → `#FFFFFF`
   - **Background Color** → `#FF4500`
   - **Background Shape** → `Circle`
   - **Font Family** → `Inter` or `Syne`
   - **Font Size** → `90`
3. Click **Download Package**
4. Extract the ZIP — you get these files:
```
favicon.ico
favicon-16x16.png
favicon-32x32.png
apple-touch-icon.png
android-chrome-192x192.png
android-chrome-512x512.png
site.webmanifest
```

---

## 📁 Where to Add the Files

In a **Vite + React** project, place all favicon files inside the `public/` folder:

```
vivek-portfolio/
├── public/
│   ├── favicon.ico
│   ├── favicon-16x16.png
│   ├── favicon-32x32.png
│   ├── apple-touch-icon.png
│   ├── android-chrome-192x192.png
│   ├── android-chrome-512x512.png
│   └── site.webmanifest
├── src/
└── index.html
```

> ✅ Vite automatically serves everything inside `public/` at the root `/` — no extra config needed.

---

## 🔗 How to Link in `index.html`

Add these lines inside the `<head>` tag:

```html
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
<link rel="icon" type="image/x-icon" href="/favicon.ico">
<link rel="manifest" href="/site.webmanifest">
```

---

## ✅ Key Takeaways

- Favicon = small icon on browser tab
- Create for free at **favicon.io**
- In Vite projects → files go in `public/` folder
- Link them in `index.html` inside `<head>`
- Netlify auto-picks them up on next deploy
