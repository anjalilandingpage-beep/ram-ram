# Anjali Thakur Landing Page — Deployment Guide

Single-file HTML landing page ready for **Vercel + Meta Pixel**.

---

## Files for deployment

Push only these 3 files to GitHub:

| File | Purpose |
|---|---|
| `index.html` | The entire site (HTML + CSS + JS in one file) |
| `vercel.json` | Caching + security headers |
| `DEPLOYMENT.md` | This guide (optional) |

> Don't push `frontend/`, `backend/`, `node_modules/` — those are dev-only. Only the 3 files above.

---

## Deploy on Vercel — 3 steps

### 1. Push to GitHub
```bash
# Create a new GitHub repo (e.g., anjali-landing), then:
git init
git add index.html vercel.json DEPLOYMENT.md
git commit -m "Initial landing page"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/anjali-landing.git
git push -u origin main
```
> Or use Emergent's **"Save to Github"** button in the chat input.

### 2. Import to Vercel
- Go to [vercel.com/new](https://vercel.com/new) → **Import** your GitHub repo.
- Framework Preset: **Other** (auto-detected as static HTML).
- Click **Deploy**.

### 3. Done
- Vercel gives you a live URL: `https://your-project.vercel.app`.
- Add a custom domain later from the Vercel dashboard.

---

## Meta Pixel setup

The Pixel base code is already wired in `index.html`. You just need to drop your Pixel ID.

### Step 1 — Get your Pixel ID
- Go to [Meta Events Manager](https://business.facebook.com/events_manager).
- **Connect Data Sources** → **Web** → Create or pick existing Pixel.
- Copy the **15-digit Pixel ID** (e.g. `1234567890123456`).

### Step 2 — Replace in `index.html`
Open `index.html`, search & replace **both** occurrences:

```
YOUR_PIXEL_ID  →  1234567890123456
```

Locations:
- Line `fbq('init', 'YOUR_PIXEL_ID');`
- Line `src="https://www.facebook.com/tr?id=YOUR_PIXEL_ID&ev=PageView&noscript=1"`

Commit & push. Vercel auto-redeploys in ~30 seconds.

### Step 3 — Verify Pixel
1. Install [Meta Pixel Helper](https://chrome.google.com/webstore/detail/meta-pixel-helper/fdgfkebogiimcoedlicjlajpkdmockpc) (Chrome extension).
2. Visit your live Vercel URL.
3. Pixel Helper icon should turn **blue** with a green checkmark — meaning Pixel is firing correctly.
4. In Events Manager → **Test Events** tab, paste your URL → fire test events to confirm.

---

## Events that fire automatically

| Event | When it fires | Use this for |
|---|---|---|
| `PageView` | Every page load | Build "All Visitors" custom audience |
| `ViewContent` | Page fully loaded | Retarget engaged viewers |
| `InitiateCheckout` | User clicks "Get Started" / opens form | Warm leads audience |
| `Lead` | **Form submitted** (right before WhatsApp redirect) | **Primary conversion event for ads** |
| `Contact` | Same moment as Lead | Backup signal |

### How to use in Meta Ads Manager

1. Create a new campaign → Objective: **Leads**
2. Ad Set → Conversion Event: **Lead** (the one we fire on form submit)
3. Audience: Use **Lookalike** based on `Lead` event after you collect 50+ leads
4. Budget: Start with ₹500–1000/day, scale once Pixel learns
5. Creative: Use one of Anjali's success-story videos

---

## Quick edits later (find & change in `index.html`)

| Want to change | Search for |
|---|---|
| WhatsApp number | `WHATSAPP_NUMBER` |
| Coach photo | `Anjali Thakur` near `<img src=` |
| Feedback videos | `feedbacks = [` |
| Achievement videos | `achievements = [` |
| Stats numbers (500+, 20L+) | `stat-val` |
| Page title / SEO | `<title>` and `<meta name="description">` |
| Pixel ID | `YOUR_PIXEL_ID` (2 places) |

---

## Troubleshooting

**Videos don't play after deploy?**
- The videos are hosted on Emergent CDN. They'll keep working. If they ever break, replace the URLs in the `feedbacks` and `achievements` arrays with new video URLs (e.g. upload to Cloudinary, S3, or your own host).

**Pixel Helper says "No Pixel found"?**
- You forgot to replace `YOUR_PIXEL_ID`. Search for it in `index.html` — must be replaced in **2 places**.

**Form doesn't redirect to WhatsApp?**
- On desktop browsers without WhatsApp Web logged in, it opens `wa.me` (works). On mobile with WhatsApp installed, it opens the app directly. Both are correct.

**Want leads saved to a database too (not just WhatsApp)?**
- Ask me to add a backend (FastAPI + MongoDB or Google Sheets integration). Form will save the lead AND redirect to WhatsApp.

---

## Conversion funnel summary

```
Visitor → PageView
   ↓
Watches video → ViewContent
   ↓
Clicks "Get Started" → InitiateCheckout (modal opens)
   ↓
Fills form & submits → Lead + Contact (PIXEL CONVERSION)
   ↓
WhatsApp opens with pre-filled message → +91 8881733366
   ↓
You reply on WhatsApp → close the deal
```

That's it. Push, deploy, drop Pixel ID, run ads.
