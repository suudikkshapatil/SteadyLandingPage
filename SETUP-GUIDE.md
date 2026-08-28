# Steady Landing Page — Deployment Guide (Updated)

This walks you through exactly what to upload, what to name your images, whether you need Firebase, and how to get it all live on Vercel.

---

## 📌 First — answering your Firebase question

**No, you don't need Firebase for this.** Here's the reasoning: Firebase Authentication (the Google sign-in) is only needed by the **Steady app itself** — the tool with breathing exercises, mood tracking, streaks — because that app needs to know *who* is logging data, so it can save it and sync it back to that same person later.

This landing page is different. It's a **sales page + checkout flow**. Nobody needs to "sign in" to read about Steady, see the price, or pay for it. The only account creation that matters happens *after* payment, when they open the actual Steady app from the thank-you page — and that's where Firebase already lives, in the app itself, completely separately from this landing page.

So: **landing page = no login, no Firebase, no database.** It's just static HTML, CSS, and JavaScript files that Vercel serves directly. This is exactly why it's free and simple to host.

---

## 📁 STEP 1 — Build your folder exactly like this

On your computer, make a folder called `steady-landing`. Inside it, you need:

```
steady-landing/
├── index.html          ← your sales page
├── thankyou.html        ← the after-payment page
├── refund.html          ← refund policy page
├── images/
│   ├── founder.jpg
│   ├── screen-symptoms.jpg
│   ├── screen-toolkit.jpg
│   ├── screen-patterns.jpg
│   └── screen-export.jpg
```

### Important renaming step

I've been working on a file called `index-v2.html` so you could compare it against your older `index.html` side by side. Before you upload anything, **rename `index-v2.html` to `index.html`** (replacing the old one) on your own computer. This matters because when a website loads, the server automatically looks for a file literally named `index.html` as the homepage — if it's still called `index-v2.html`, visitors would need to type the full filename into the URL to see your new version, which obviously won't happen from an ad click.

---

## 🖼️ STEP 2 — Your image files: exact names, exact folder

You said you'll rotate and crop the founder photo yourself — perfect, here's exactly what the code is looking for. **These filenames are case-sensitive and must match exactly:**

| What it is | Exact filename | Goes in |
|---|---|---|
| Your founder photo | `founder.jpg` | `images/` folder |
| Symptom selection screenshot | `screen-symptoms.jpg` | `images/` folder |
| Toolkit list screenshot | `screen-toolkit.jpg` | `images/` folder |
| Patterns/insights screenshot | `screen-patterns.jpg` | `images/` folder |
| PDF export screenshot | `screen-export.jpg` | `images/` folder |

**Why they're not showing up right now:** the code has a line like `<img src="images/founder.jpg">` — this tells the browser "go look inside a folder called `images`, sitting right next to this HTML file, for something named exactly `founder.jpg`." Since that folder and those exact files don't exist in your project yet (you're only seeing my placeholder versions, which live inside this chat, not in a real folder), the browser can't find them and shows a broken image icon instead.

**What to do:**
1. Edit your founder photo — rotate it upright, crop however you like.
2. Save it as `founder.jpg` (lowercase, exactly that, no spaces, no "IMG_2858" or anything else).
3. Do the same for your 4 screenshots — rename them to match the table above exactly.
4. Put all 5 files inside a folder named `images` (lowercase) sitting directly inside your `steady-landing` folder, next to `index.html`.

If you'd rather use `.png` instead of `.jpg`, that's fine — just also change the filename ending inside the HTML code to match (e.g., `founder.png`), otherwise the browser will keep looking for a `.jpg` that doesn't exist. Let me know if you'd like me to make that change instead.

---

## ⚠️ Before you go further — a gap I want to flag honestly

Your footer links to 6 pages: Privacy Policy, Terms & Conditions, Refund Policy, Delivery Policy, Contact Us, and About. Right now, **only Refund Policy actually exists** as a real page (`refund.html`). The other 5 links will lead to a "page not found" error if someone clicks them today.

This matters for two reasons: it looks unfinished to a buyer who clicks "Privacy Policy" out of curiosity, and **Razorpay specifically checks for these pages during KYC approval** — without them, your payment activation could get delayed. I'm happy to build the remaining 5 pages now, in the same style as `refund.html`, if you'd like — just say the word.

---

## 🐙 STEP 3 — Upload to GitHub

1. Go to **github.com** and sign in (or create a free account if you haven't)
2. Click the **+** icon in the top-right corner → **New repository**
3. Name it `steady-landing`. Keep visibility set to **Public**. Don't tick "Add a README" — we don't need one.
4. Click **Create repository**
5. On the next screen, click the link that says **uploading an existing file**
6. Drag in everything from inside your `steady-landing` folder: `index.html`, `thankyou.html`, `refund.html`, and the whole `images` folder (with all 5 files inside it)
7. Scroll down and click **Commit changes**

That's it — your code now lives on GitHub. Think of GitHub here purely as file storage with version history; it's not doing anything active yet. Vercel is what will actually turn these files into a live website.

---

## ▲ STEP 4 — Host it on Vercel (free)

1. Go to **vercel.com** → click **Sign up** → choose **Continue with GitHub** (this links the two accounts so Vercel can see your repository)
2. Once logged in, click **Add New…** → **Project**
3. Find `steady-landing` in the list of repositories → click **Import**
4. On the settings screen, leave everything as default (Framework Preset: "Other") — we don't need any build steps since this is plain HTML
5. Click **Deploy**

Vercel will spend about 20–30 seconds copying your files onto its servers. When it's done, you'll get a live link that looks like:

```
https://steady-landing.vercel.app
```

Open it and check: does your photo show up? Do the 4 screenshots appear? If any are still broken, it almost always means a filename typo somewhere — double check the exact spelling against the table in Step 2.

Your thank-you page will automatically be reachable at:
```
https://steady-landing.vercel.app/thankyou.html
```

**Keep both links handy — you'll need them for Razorpay next.**

---

## 🔁 Updating the site later

Any time you want to change something — swap a screenshot, fix a typo, add the missing policy pages — you don't need to redo any of this setup. Just:
1. Go to your repository on GitHub
2. Click on the file you want to change (or "Add file" to upload a new one)
3. Edit or replace it, then commit the change

Vercel is watching your GitHub repository automatically, so within about 20 seconds of your commit, the live site updates itself. This connection between GitHub and Vercel is the one "wiring" step you already did in Step 4 — you won't need to reconnect anything.

---

## 💳 Razorpay (unchanged from before)

Once your site is live, the Razorpay setup is the same as we discussed earlier: complete KYC using your Vercel URL as your business website, create a Payment Button for ₹149, set its redirect URL to your `thankyou.html` link, and paste the embed code into the marked spot inside `index.html`. Let me know when you're at this stage and I can walk through it step by step again with your actual live links.

---

## ✅ Quick checklist for today

- [ ] Rename `index-v2.html` → `index.html` on your computer
- [ ] Rotate/crop your founder photo, save as `founder.jpg`
- [ ] Rename your 4 screenshots to match the exact table above
- [ ] Put all 5 images inside an `images` folder next to `index.html`
- [ ] Decide: want me to build the 5 missing policy pages now?
- [ ] Upload everything to a new GitHub repo
- [ ] Import that repo into Vercel and deploy
- [ ] Open the live link and confirm every image actually loads
