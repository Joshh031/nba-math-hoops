# 🛒 Our Shopping List

A **real-time shared shopping list** built for two people. Add things by
**typing or voice**, everything is **auto-categorized** (Produce, Dairy, Household…),
and every item gets a **priority**:

| | Priority | Meaning |
|---|---|---|
| 🔴 | **Need today** | Grab it on the next trip out |
| 🟡 | **1–2 days** | Needed soon |
| 🟢 | **Not urgent** | Whenever it's convenient |

With live sync turned on, when one of you adds "milk", it appears on the
other's phone **instantly** — no refresh, no app store, no accounts to create.

## ✨ Features

- 🎤 **Voice input** — tap the mic and say *"milk, eggs and bananas"* → three items added at once. Say *"diapers, urgent"* and it lands in 🔴 Need today.
- 🗂 **Smart categories** — "cheddar" files itself under 🥛 Dairy & Eggs automatically. Tap any item's category to change it.
- 🚦 **Priorities** — pick one when adding; tap an item's priority badge to change it later.
- 👤 **Who added it** — each item shows your name, so you know who wants what.
- ✅ **Check-off & undo** — tap an item to check it off; deleted something by accident? There's an Undo button.
- 📱 **Feels like an app** — add it to your phone's home screen and it opens full-screen with its own icon. Works in light and dark mode.
- 🔌 **Works offline** — changes sync as soon as you're back online.

## 🚀 Get it on your phones (2 minutes)

The app deploys itself to GitHub Pages automatically (see the **Actions** tab).

1. Go to your repo's **Settings → Pages** and make sure the source is **GitHub Actions** (the workflow usually sets this up on its first run — if the first run failed, set this and re-run it).
2. Your list lives at: `https://joshh031.github.io/nba-math-hoops/`
3. Open that link on both phones. In your browser menu choose **Add to Home Screen** — now it's an app icon.

> 💡 Until you set up live sync below, the list is saved per-device (great for trying it out).

## 🔄 Set up live sync (10 minutes)

Live sync uses **Firebase** (Google, free tier — a family shopping list won't come close to the limits).

1. Go to <https://console.firebase.google.com> and sign in with a Google account.
2. **Create a project** — call it anything, e.g. `our-shopping-list`. (Google Analytics: off is fine.)
3. In the left menu: **Build → Realtime Database → Create Database**. Pick the location closest to you and start in **locked mode**.
4. Open the **Rules** tab and replace the rules with:
   ```json
   {
     "rules": {
       "lists": {
         ".read": "auth != null",
         ".write": "auth != null"
       }
     }
   }
   ```
   Click **Publish**.
5. In the left menu: **Build → Authentication → Get started → Sign-in method → Anonymous → Enable**. (The app signs both of you in silently — no passwords, no emails.)
6. Click the ⚙️ **Project settings** (top-left) → scroll to **Your apps** → click the web icon `</>` → register the app (any nickname, no hosting needed). Firebase shows you a `firebaseConfig = { ... }` code block.
7. Edit [`firebase-config.js`](firebase-config.js) in this repo (the ✏️ button on GitHub works fine): replace `window.FIREBASE_CONFIG = null;` with
   ```js
   window.FIREBASE_CONFIG = { /* paste the config values here */ };
   ```
   and commit.
8. Wait ~1 minute for the site to redeploy, then refresh the app on both phones. The pill in the top-right turns **🟢 Live** — you're sharing one list!

> 🔐 The config values are safe to commit — they're identifiers, not secrets. Access is controlled by the database rules from step 4.

## 🧪 Try it locally

It's a static site — no build step:

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

(Voice input needs HTTPS or localhost, so opening the file directly won't enable the mic.)

## 🏷 Renaming this repo

This repo can be renamed (e.g. to `shared-shopping-list`) in **Settings → General → Repository name** — GitHub redirects the old URLs automatically. Your Pages URL becomes `https://joshh031.github.io/<new-name>/`; the in-app README links point at the old name but will follow GitHub's redirect.

## 🧰 How it's built

No frameworks, no build step, no dependencies to maintain: one `index.html`
with vanilla JS, the Web Speech API for voice, and Firebase Realtime Database
for sync (loaded only when configured). A tiny service worker makes it
installable and fast to open.
