# Household Budget Book — setup guide

This folder is a complete iPhone-installable app (a PWA). It opens full screen from your Home Screen, works offline, and keeps one shared budget for the two of you.

- **GitHub Pages** hosts the app for free.
- **Firebase** (Google's free tier) stores your entries and handles sign-in, so both phones see the same numbers.

Setup takes about 20 minutes and is done once. You only need a computer for steps 1–5; a phone browser works but is fiddly.

---

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The app |
| `config.js` | Where you paste your Firebase settings (step 3) |
| `firestore.rules` | Who is allowed to see the budget (step 4) |
| `manifest.webmanifest`, `sw.js`, `icons/` | What makes it installable and work offline |

---

## Step 1 — Create the Firebase project

1. Go to **console.firebase.google.com** and sign in with a Google account.
2. Click **Create a project** (or **Add project**). Name it `household-budget`.
3. Turn **off** Google Analytics (not needed), then click **Create project**.

## Step 2 — Turn on sign-in and add your two accounts

1. In the left menu: **Build → Authentication → Get started**.
2. Under **Sign-in method**, choose **Email/Password**, switch on the first toggle, and click **Save**.
3. Open the **Users** tab → **Add user**. Enter your email and a password. Repeat for your partner.
   These are the logins you'll use in the app.

## Step 3 — Create the database and connect the app

1. Left menu: **Build → Firestore Database → Create database**.
2. Pick a location close to you (for Southeast Asia, `asia-southeast1` Singapore is a good choice). This can't be changed later.
3. Choose **Start in production mode** → **Create**.
4. Click the **gear icon** (top left) → **Project settings**. Under **Your apps**, click the **`</>`** (Web) icon.
5. Give it a nickname like `Budget`. Leave "Firebase Hosting" unticked. Click **Register app**.
6. You'll see a block of code containing `const firebaseConfig = { apiKey: ..., ... }`.
   Open **`config.js`** from this folder in any text editor and replace each `PASTE_...` value with the matching value from that block. Save.

> These settings are safe to publish. What protects your data is the rules in step 4.

## Step 4 — Lock the budget to just the two of you

1. Back in **Firestore Database**, open the **Rules** tab.
2. Delete what's there and paste the contents of **`firestore.rules`** from this folder.
3. Replace `your.email@example.com` and `partner.email@example.com` with the two emails from step 2, **in lowercase**.
4. Click **Publish**.

## Step 5 — Put the app on GitHub Pages

1. Sign in at **github.com** (create a free account if you don't have one).
2. Click **+** (top right) → **New repository**. Name it `budget`, set it to **Public**, and click **Create repository**.
   (Free GitHub Pages needs a public repository. People could see the app's code, but not your budget; that stays in Firebase behind your rules.)
3. On the new repository page, click **uploading an existing file**. Drag in **everything in this folder**, including the `icons` folder. Click **Commit changes**.
4. Go to the repository's **Settings → Pages**. Under **Build and deployment**, set Source to **Deploy from a branch**, Branch to **main** and folder **/ (root)**. Click **Save**.
5. Wait 1–2 minutes and refresh. The page shows your app's address, like:
   `https://YOUR-GITHUB-NAME.github.io/budget/`
6. **Let Firebase accept sign-ins from that address:** in Firebase go to **Authentication → Settings → Authorized domains → Add domain** and enter `YOUR-GITHUB-NAME.github.io` (no `https://`, no `/budget`).

## Step 6 — Install on each iPhone

1. Open **Safari** (it must be Safari) and go to your app address from step 5.
2. Tap the **Share** button (square with an arrow) → **Add to Home Screen** → **Add**.
3. Open **Budget** from the Home Screen and sign in with your account from step 2.

Do the same on your partner's iPhone with their login. Anything either of you adds shows up on the other phone within seconds.

---

## Everyday use

- **Offline:** you can add, edit and tick off entries with no signal. The dot next to your email turns orange and the changes upload once you're back online. The first sign-in on each phone needs internet.
- **New month:** tap **›** to move to the next month, then **Copy bills from last month** under Bills.
- **Forgot password:** type your email on the sign-in screen and tap **Forgot password?**

## Changing the app later

Edit a file on GitHub (open it → pencil icon → **Commit changes**). Then in `sw.js` change `budget-v1` to `budget-v2` (and so on each time) so the phones pick up the new version. Close and reopen the app on the phone; it may take two opens to update.

To change the first month shown, edit `START_MONTH` in `config.js`.

## If something goes wrong

| What you see | Fix |
|---|---|
| "This app isn't connected to Firebase yet" | `config.js` still has `PASTE_...` values (step 3.6), or the updated file wasn't uploaded to GitHub. |
| "That email and password don't match" | Check the login in Firebase → Authentication → Users. |
| Sign-in fails with `unauthorized-domain` | Add your `github.io` address in Authorized domains (step 5.6). |
| "This account can't see the budget" | The email isn't in the Firestore rules exactly, in lowercase (step 4). |
| A blank white screen | The phone had no internet on its very first open. Connect and reopen. |
| GitHub page shows 404 | Wait a few minutes after step 5.4, and check `index.html` is at the top level of the repository, not inside a folder. |

## Cost

Firebase's free plan (Spark) covers 50,000 reads and 20,000 writes a day, far more than a household budget uses. GitHub Pages is free. You never need to add a credit card.
