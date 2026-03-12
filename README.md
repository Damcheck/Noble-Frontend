# Noble Funded — Frontend Website

> **⚠️ IMPORTANT: READ THIS ENTIRE DOCUMENT BEFORE MAKING ANY CHANGES**
>
> This is a **pre-compiled static website** (not a standard Next.js project). Editing the wrong files will break the site. Follow the instructions below carefully.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Project Structure](#project-structure)
3. [How to Run Locally](#how-to-run-locally)
4. [How to Deploy](#how-to-deploy)
5. [⚠️ Rules — What NOT to Touch](#%EF%B8%8F-rules--what-not-to-touch)
6. [Checkout Links (API Integration)](#checkout-links-api-integration)
7. [Dashboard Login Link](#dashboard-login-link)
8. [Contact Form](#contact-form)
9. [Pricing Configuration](#pricing-configuration)
10. [Changing Text Content](#changing-text-content)
11. [Changing Images](#changing-images)
12. [Adding Your Own Tracking/Analytics](#adding-your-own-trackinganalytics)
13. [Color Palette Reference](#color-palette-reference)
14. [Known Limitations](#known-limitations)

---

## Project Overview

This is the **Noble Funded** prop trading firm frontend website. It was originally a Next.js application that has been exported as **static HTML/CSS/JS files**. It does NOT require Node.js, npm, or any build step to run.

**Tech Stack:**

- Pure HTML5 + CSS3 + JavaScript (pre-compiled from React/Next.js)
- No server-side rendering needed
- Can be hosted on any static hosting platform (Vercel, Netlify, AWS S3, etc.)

---

## Project Structure

```
Noble-Frontend/
├── index.html              ← Homepage (MAIN PAGE)
├── affiliate.html          ← Affiliate program page
├── contact-us.html         ← Contact page
├── faqs.html               ← FAQ page
├── rules.html              ← Trading rules page
│
├── *__rsc_*.html           ← React Server Component payloads (DO NOT DELETE)
│
├── _next/
│   ├── static/
│   │   ├── chunks/         ← Compiled JavaScript (contains pricing, checkout links, FAQ content)
│   │   │   ├── 652-*.js    ← ⭐ MAIN APP LOGIC (checkout links, pricing, FAQ text, nav)
│   │   │   ├── app/        ← Page-specific JS bundles
│   │   │   └── *.js        ← Framework JS (DO NOT EDIT)
│   │   ├── css/
│   │   │   └── *.css       ← Compiled CSS styles (DO NOT EDIT)
│   │   └── media/          ← Font files
│   └── image_url_*.html    ← Image proxy wrappers (DO NOT DELETE)
│
├── images/                 ← All website images and assets
│   ├── logo.svg            ← Noble Funded logo
│   ├── NB bg.webm          ← Hero section background video
│   ├── hero/               ← Hero section assets (payment icons, crypto icons)
│   ├── about/              ← Feature section SVG icons
│   ├── top-rated/          ← "Why #1" section card backgrounds
│   ├── payouts/            ← Payout certificate images
│   ├── affiliate/          ← Affiliate page assets
│   └── *.png/*.svg/*.webp  ← Other images
│
├── favicon/                ← Browser tab icons
├── documents/              ← PDF documents (terms, privacy, AML policy)
└── README.md               ← This file
```

---

## How to Run Locally

No build step required. Just serve the files with any static server:

```bash
# Option 1: Using npx (recommended)
npx -y serve -l 3000 .

# Option 2: Using Python
python3 -m http.server 3000

# Option 3: Using PHP
php -S localhost:3000
```

Then open `http://localhost:3000` in your browser.

---

## How to Deploy

### Vercel (Current)

Push to GitHub and import into Vercel. It auto-detects static files.

**Important Vercel Setting:** Set the "Framework Preset" to **"Other"** (not Next.js) since this is pre-compiled.

### Netlify

1. Push to GitHub
2. Import repo in Netlify
3. Set "Publish Directory" to `.` (root)
4. No build command needed

### Any Web Server

Simply upload all files to your web server's root directory. No build step required. Works with nginx, Apache, or any CDN.

---

## ⚠️ Rules — What NOT to Touch

> **CRITICAL: Breaking these rules WILL break the website.**

### ❌ NEVER modify these files

| File/Directory | Why |
|---|---|
| `_next/static/css/*.css` | Compiled CSS — editing breaks all styling |
| `_next/static/chunks/webpack-*.js` | Framework bootstrapper |
| `_next/static/chunks/main-app-*.js` | React framework core |
| `_next/static/chunks/polyfills-*.js` | Browser compatibility |
| `_next/static/chunks/app/layout-*.js` | Layout component |
| `_next/static/media/*` | Font files |
| `*__rsc_*.html` files | React hydration payloads — deleting breaks page rendering |
| `_next/image_url_*.html` files | Image proxy wrappers — deleting breaks images |

### ✅ Safe to modify

| File | What You Can Change |
|---|---|
| `_next/static/chunks/652-*.js` | Checkout URLs, pricing, FAQ text, promo codes |
| `index.html` | Homepage text, meta tags, hero section text |
| `affiliate.html` | Affiliate page text, meta tags |
| `contact-us.html` | Contact page text, meta tags |
| `faqs.html` | FAQ page text, meta tags |
| `rules.html` | Rules page text, meta tags |
| `images/*` | Replace images (keep same filename and format) |

---

## Checkout Links (API Integration)

All checkout/purchase links are located in **one file**:

**File:** `_next/static/chunks/652-8c1b25fb4aeaa8fd.js`

Search for `noblefundedcheckout.com` in this file. You will find 4 arrays of URLs — one for each account type:

```
Array 1: Spartan Instant accounts
  https://noblefundedcheckout.com/product/spartan-instant-1k
  https://noblefundedcheckout.com/product/spartan-instant-3k
  https://noblefundedcheckout.com/product/spartan-instant-6k
  https://noblefundedcheckout.com/product/spartan-instant-15k
  https://noblefundedcheckout.com/product/spartan-instant-25k
  https://noblefundedcheckout.com/product/spartan-instant-50k
  https://noblefundedcheckout.com/product/spartan-instant-100k

Array 2: Instant accounts
  https://noblefundedcheckout.com/product/instant-5k
  https://noblefundedcheckout.com/product/instant-10k
  ... etc.

Array 3: 1-Step Challenge accounts
  https://noblefundedcheckout.com/product/1-step-challenge-5k
  ... etc.

Array 4: 2-Step Challenge accounts
  https://noblefundedcheckout.com/product/2-step-challenge-5k
  ... etc.
```

**To change checkout URLs:** Open the file, find-and-replace each URL with your actual checkout system URL. The order matches the account sizes listed in the pricing table on the website.

---

## Dashboard Login Link

The "Log In" button in the navigation header links to:

```
https://dashboard.noblefunded.com
```

**Locations:** This link appears in multiple places:

- **`index.html`** line 37 (inside the nav bar HTML) — search for `dashboard.noblefunded.com`
- **`_next/static/chunks/652-*.js`** — search for `dashboard.noblefunded.com`
- Also appears in `affiliate.html`, `contact-us.html`, `faqs.html`, `rules.html`

**To change:** Find-and-replace `dashboard.noblefunded.com` with your actual dashboard URL in ALL files:

```bash
# Quick command to replace across all files:
find . -type f \( -name "*.html" -o -name "*.js" \) -exec sed -i '' 's|dashboard.noblefunded.com|YOUR-DASHBOARD-URL.com|g' {} +
```

---

## Contact Form

The contact form on `contact-us.html` currently uses a **mailto:** link:

```
mailto:support@noblefunded.com
```

**Location:** `_next/static/chunks/652-*.js` — search for `mailto:support@noblefunded.com`

The form constructs a mailto: link with the subject and body fields. To connect it to a real backend API:

### Option A: Keep mailto (simplest)

Just update the email address:

```bash
find . -type f \( -name "*.html" -o -name "*.js" \) -exec sed -i '' 's|support@noblefunded.com|YOUR-EMAIL@yourdomain.com|g' {} +
```

### Option B: Connect to Backend API

To replace the mailto with an actual form submission, find this section in `652-*.js`:

```javascript
// Search for: "mailto:support@noblefunded.com"
// You'll find a form onSubmit handler that creates a mailto link
```

Replace the `window.location.href=a` (mailto redirect) with a `fetch()` call to your backend:

```javascript
fetch('https://your-api.com/contact', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: e.name, email: e.email, subject: e.subject, message: e.message })
})
```

---

## Pricing Configuration

All pricing data is in **one file**: `_next/static/chunks/652-8c1b25fb4aeaa8fd.js`

### Account Sizes

Search for the arrays that contain `"$1K"`, `"$3K"`, etc:

```javascript
// Spartan sizes:     ["$1K","$3K","$6K","$15K","$25K","$50K","$100K"]
// Instant sizes:     ["$5K","$10K","$15K","$25K","$50K","$100K"]
// 1-Step sizes:      ["$5K","$10k","$25K","$50K","$100K","$200K"]
// 2-Step sizes:      ["$5K","$10k","$25K","$50K","$100K","$200K"]
```

### Prices (Original / Discounted)

Search for the price arrays. There are two sets:

**Original prices (shown with strikethrough):**

```javascript
["$17","$39","$75","$112","$159","$225","$419"]   // Spartan
["$119","$178","$204","$319","$497","$897"]        // Instant
["$67","$119","$219","$340","$619","$847"]          // 1-Step
["$54","$99","$197","$319","$599","$793"]           // 2-Step
```

**Discounted prices (shown as main price):**

```javascript
["$9","$21","$49","$86","$99","$159","$209"]       // Spartan
["$119","$178","$204","$319","$497","$897"]         // Instant
["$67","$119","$219","$340","$619","$847"]          // 1-Step
["$54","$99","$197","$319","$599","$793"]           // 2-Step
```

### Profit Targets (Spartan)

Search for the array:

```javascript
["$100","$240","$480","$960","$2000","$4500","$8500"]
```

### Trading Rules Values

The challenge rules (drawdown %, profit targets, etc.) are rendered via conditional logic in the same JS file. Search for strings like `"6%"`, `"5%"`, `"3%"`, `"12%"`, `"80%"`, `"90%"` near the pricing section.

### Promo Code

The header banner shows `Code: NOBLE40`. To change it, search for `NOBLE40` in ALL HTML files:

```bash
find . -name "*.html" -exec sed -i '' 's|NOBLE40|YOUR-NEW-CODE|g' {} +
```

---

## Changing Text Content

### In HTML files (header/footer text, meta tags)

Open the relevant `.html` file and edit the text directly. For example:

- **Page title:** Search for `<title>` tag
- **Meta description:** Search for `<meta name="description"`
- **Footer text:** Search for `Noble Markets Ltd`
- **Support email:** Search for `support@noblefunded.com`

### In JS chunks (dynamic content)

FAQ answers, testimonials, feature descriptions, etc. are in `_next/static/chunks/652-*.js`. Open this file and search for the text you want to change.

**⚠️ When editing JS files:**

- Do NOT add line breaks inside strings
- Do NOT change quotes style (keep `\"` escaped quotes exactly as-is)
- Do NOT delete commas, brackets, or parentheses
- Test the site immediately after any change

---

## Changing Images

### Simple Replacement

To replace an image, simply overwrite the file in `/images/` with a new file using the **exact same filename**:

```bash
# Example: Replace the logo
cp /path/to/new-logo.svg images/logo.svg

# Example: Replace hero video
cp /path/to/new-video.webm "images/NB bg.webm"
```

### Image Reference

| Image | Location | Used For |
|---|---|---|
| `images/logo.svg` | Header & Footer | Site logo |
| `images/NB bg.webm` | Hero section | Background video |
| `images/section.webp` | — | Old section background (replaced) |
| `images/sectiob3.png` | "Join thousands" section | Background image |
| `images/platform.webp` | — | Old platform image (replaced) |
| `images/Stars.png` | "How it Works" section | Step illustration |
| `images/top-rated/BG Sec.png` | "#1 Fastest Growing" section | Card backgrounds |
| `images/payouts/*.jpg` | Payout ticker | Payout certificates |
| `images/hero/*.png` | Hero section | Payment method icons |
| `images/about/*.svg` | Feature cards | Feature icons |
| `images/gift.png` | Popup | Promotional popup image |

### ⚠️ Payout Certificate Images

The payout certificate images in `images/payouts/` still display **the old** branding because they are raster images with baked-in text. These need to be replaced with Noble Funded branded certificates using Photoshop or similar.

---

## Adding Your Own Tracking/Analytics

All previous third-party tracking (Facebook Pixel, Intercom, TrackDesk, Google Ads) has been removed. To add your own:

### Google Analytics / Tag Manager

Add this just before `</head>` in ALL 5 HTML files:

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Facebook Pixel

Add this just before `</head>` in ALL 5 HTML files:

```html
<script>
  !function(f,b,e,v,n,t,s)
  {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
  n.callMethod.apply(n,arguments):n.queue.push(arguments)};
  if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
  n.queue=[];t=b.createElement(e);t.async=!0;
  t.src=v;s=b.getElementsByTagName(e)[0];
  s.parentNode.insertBefore(t,s)}(window, document,'script',
  'https://connect.facebook.net/en_US/fbevents.js');
  fbq('init', 'YOUR-PIXEL-ID');
  fbq('track', 'PageView');
</script>
```

### Intercom / Live Chat

Add this just before `</body>` in ALL 5 HTML files:

```html
<script>
  window.intercomSettings = {
    api_base: "https://api-iam.intercom.io",
    app_id: "YOUR-APP-ID"
  };
</script>
<script>(function(){var w=window;var ic=w.Intercom;if(typeof ic==="function"){ic('reattach_activator');ic('update',w.intercomSettings);}else{var d=document;var i=function(){i.c(arguments);};i.q=[];i.c=function(args){i.q.push(args);};w.Intercom=i;var l=function(){var s=d.createElement('script');s.type='text/javascript';s.async=true;s.src='https://widget.intercom.io/widget/YOUR-APP-ID';var x=d.getElementsByTagName('script')[0];x.parentNode.insertBefore(s,x);};if(document.readyState==='complete'){l();}else if(w.attachEvent){w.attachEvent('onload',l);}else{w.addEventListener('load',l,false);}}})();</script>
```

---

## Color Palette Reference

| Color | Hex Code | Usage |
|---|---|---|
| Dark Teal (Primary) | `#002B36` | Backgrounds, cards, footer |
| Mid Teal (Secondary) | `#14655B` | Borders, secondary elements |
| Mint (Accent) | `#A7FFEB` | Buttons, highlights, gradients |
| White | `#FFFFFF` | Primary text |
| White/50 | `rgba(255,255,255,0.5)` | Secondary text |
| Muted Teal | `#5E8A84` | Muted text in pricing cards |

---

## Known Limitations

1. **Not a standard Next.js project** — There is no `package.json` or React source code. This is pre-compiled static output. You cannot run `npm install` or `npm run build`.

2. **Minified JavaScript** — The JS in `_next/static/chunks/` is minified and bundled. Editing requires care. Always test after changes.

3. **React hydration** — The site uses React hydration (JS takes over the HTML after page load). If the HTML and JS content don't match, React may "flash" old content. Always match changes in BOTH the HTML and the JS file.

4. **Payout certificates** — The images in `/images/payouts/` still show the old text. These are raster JPG images and need manual replacement with new Noble Funded branded certificates.

5. **Documents folder** — The `/documents/` folder should contain `tnc.pdf`, `privacy.pdf`, `aml.pdf`, `prohibited.pdf`. If these are missing, create and add them.

6. **Giveaway popup** — A promotional popup appears 10 seconds after page load (GET 10% OFF). It sends an email to a server action that no longer works. To disable it, search for `setTimeout` and `10e3` (10000ms) in `652-*.js`.

---

## Quick Reference Commands

```bash
# Replace checkout domain across all files
find . -type f \( -name "*.html" -o -name "*.js" \) -exec sed -i '' 's|noblefundedcheckout.com|YOUR-CHECKOUT-DOMAIN.com|g' {} +

# Replace dashboard domain across all files
find . -type f \( -name "*.html" -o -name "*.js" \) -exec sed -i '' 's|dashboard.noblefunded.com|YOUR-DASHBOARD.com|g' {} +

# Replace support email across all files
find . -type f \( -name "*.html" -o -name "*.js" \) -exec sed -i '' 's|support@noblefunded.com|YOUR-EMAIL@domain.com|g' {} +

# Replace promo code across all files
find . -name "*.html" -exec sed -i '' 's|NOBLE40|YOUR-CODE|g' {} +

# Run local preview server
npx -y serve -l 3000 .
```

---

## Need Help?

If something breaks after editing, use `git diff` to see what changed and `git checkout -- <file>` to undo the change. Always commit your work before making new edits.

```bash
# See what changed
git diff

# Undo all uncommitted changes
git checkout -- .

# Undo a specific file
git checkout -- index.html
```
