# 🚀 Noble Funded — Developer Handover Guide

> **Read this whole thing before you touch anything.** It will save you hours of confusion.

---

## 🧠 What Is This Project?

This is the **Noble Funded website** — a static HTML/CSS/JS site hosted on **Vercel**, connected to a **GitHub repo** called `Damcheck/Noble-Frontend`.

Think of it like a printed book that someone already designed. The pages are ready. You just need to swap out certain parts — like filling in your name on a form or changing a phone number in a flyer.

**There is NO live coding server running.** You don't need `npm install` or `npm run build`. Just edit the HTML files and push to GitHub. Vercel will auto-deploy.

---

## 📁 The Files That Matter — What To Touch vs What To Leave Alone

### ✅ SAFE TO EDIT — These are YOUR files:

| File | What It Is |
|---|---|
| `index.html` | The home page |
| `contact-us.html` | The Contact Us page *(already redesigned — see below)* |
| `faqs.html` | The FAQ page |
| `rules.html` | The Trading Rules page |
| `affiliate.html` | The Referral/Affiliate page |
| `images/` folder | All images used across the site |
| `documents/` folder | PDF documents (Terms, Privacy Policy, etc.) |

### 🚫 DO NOT TOUCH — Leave these exactly as they are:

| File / Folder | Why |
|---|---|
| `_next/static/` folder | This is the compiled React JavaScript. It's minified and very fragile. One wrong character breaks the whole site. |
| `*__rsc_*.html` files | These are React server component caches. Ignore them. |
| `global_v*.html` files | Old working drafts. Leave them as backup. |

---

## ✅ What Has Already Been Done — Don't Redo These

The owner has already taken care of the following. **You do NOT need to do any of this:**

### 🎨 Design & Layout
- [x] Complete color rebrand to Noble Funded colors (`#002B36`, `#14655B`, `#A7FFEB`)
- [x] Logo replaced everywhere (header + footer)
- [x] Hero section redesigned with background video, tickers, and stats
- [x] Pricing/Challenge cards with Naira (₦) pricing
- [x] Feature cards with custom icons (user-provided images)
- [x] Testimonials section
- [x] Calculator for profit estimates
- [x] Payout certificate images (using owner's own certificate image)
- [x] Footer with social links (Instagram, Discord, X/Twitter, YouTube, WhatsApp)

### 📄 Pages Completed
- [x] **Home page** (`index.html`) — Fully redesigned
- [x] **FAQ page** (`faqs.html`) — Updated with Noble Funded Q&As
- [x] **Trading Rules page** (`rules.html`) — Updated
- [x] **Affiliate/Referral page** (`affiliate.html`) — Fully redesigned with tier cards
- [x] **Contact Us page** (`contact-us.html`) — **Fully redesigned** (see details below)

### 🛒 Checkout
- [x] A separate React checkout app was built and deployed at `checkout.noblefunded.com`
- [x] "Get Funded" buttons on the home page link directly to the checkout app with the correct currency (`?currency=ngn`) and account size pre-selected

### 🔒 Next.js Hydration Fix
- [x] A special script was added to `contact-us.html` to prevent the old Next.js JavaScript from overwriting the new Contact Us design. **Don't remove it.** It's at the very top of the `<body>` tag and looks like this:
```html
<script>
  // Prevent Next.js from hydrating and overwriting static HTML
  (function() { ... })();
</script>
```

---

## 📞 The Contact Us Page — What Was Built & What Still Needs Work

### ✅ What's Done:
- Beautiful split layout: contact info on the left, message form on the right
- Contact details showing: Email, Discord, WhatsApp, Telegram, Twitter
- Form fields: Full Name, Email Address, Subject, Message
- **Form validation** — if someone leaves a field empty and clicks Send, they get a red shaking error message under that field
- **Success toast** — after filling everything in and clicking Send, a green "Message Sent! 🎉" pop-up appears at the bottom of the screen

### ❌ What Still Needs to Be Done:

#### 1. Wire up the form to actually send emails

Right now the form shows a success message and opens the user's email app as a fallback (`mailto:`), but it does **not actually send an email to your inbox automatically**. You need to connect a form backend.

**The easiest free solution: [Formspree](https://formspree.io)**

Here's how (explain to a 10-year-old):
> Imagine the form is a letter. Right now, the letter is written but there's no postman. Formspree is the postman. You sign up, they give you a special address (a form ID), and every time someone fills out your form, Formspree delivers the letter to your email.

**Steps:**
1. Go to [https://formspree.io](https://formspree.io) and create a free account
2. Click "New Form" and enter the email you want messages sent to (e.g. `support@noblefunded.com`)
3. They'll give you a **Form ID** that looks like: `https://formspree.io/f/abcdefgh`
4. Open `contact-us.html`
5. Find this line:
```javascript
var mailto = 'mailto:support@noblefunded.com'
```
6. Replace the whole `setTimeout` block that builds the mailto link with a `fetch` call to Formspree like this:

```javascript
// Replace the setTimeout block with this:
fetch('https://formspree.io/f/YOUR-FORM-ID', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: nameInput.value,
    email: emailInput.value,
    subject: subjectInput.value,
    message: msgInput.value
  })
})
.then(function(res) {
  btn.classList.remove('loading');
  btn.textContent = origText;
  form.reset();
  var toast = document.getElementById('contact-success-toast');
  if (toast) {
    toast.classList.add('show');
    setTimeout(function() { toast.classList.remove('show'); }, 4500);
  }
})
.catch(function() {
  btn.classList.remove('loading');
  btn.textContent = origText;
  alert('Something went wrong. Please email us directly at support@noblefunded.com');
});
```

---

## 🔐 Login & Sign Up — What Needs To Be Done

Right now the "Log In" and "Sign Up" buttons in the header link to:
- **Log In** → `https://dashboard.noblefunded.com`
- **Sign Up** → Same URL

These are external links to whatever dashboard system Noble Funded uses (likely a third-party prop trading dashboard or a custom backend).

### If you are building your own dashboard:
1. Build your login/register pages (separate app or same site)
2. Open ALL these HTML files: `index.html`, `contact-us.html`, `faqs.html`, `rules.html`, `affiliate.html`
3. In each file, search for `dashboard.noblefunded.com` and replace it with your login URL
4. Search for the "Sign Up" button — currently it has no separate URL — and add your registration URL

**Quick command to update all files at once:**
```bash
# Replace login URL in all HTML files at once:
find . -name "*.html" -exec sed -i '' 's|dashboard.noblefunded.com|YOUR-LOGIN-URL.com|g' {} +
```

### If you are using a third-party dashboard (e.g. Match-Trader, TradeLocker, MyFXbook):
- Just update the URL above to point to wherever traders log in
- You don't need to build anything — just update the link

---

## 🖼️ How To Update Images

> Think of it like swapping a picture in a picture frame. The frame stays the same. You just need to put in a new picture with the same size.

All images live in the `images/` folder. To replace one:
1. Get your new image ready
2. Name it **exactly the same** as the old one
3. Drop it into the `images/` folder, overwriting the old file
4. That's it — the site will automatically use the new image

**Key images:**

| File | Where It Appears |
|---|---|
| `images/logo.svg` | Top left of every page (header) + footer |
| `images/NB bg.webm` | The background video on the home page hero |
| `images/fast.png` | Feature card icon |
| `images/spilt.png` | Feature card icon |
| `images/step.png` | Feature card icon |
| `images/news.png` | Feature card icon |
| `images/target.png` | Feature card icon |
| `images/payouts/*.jpg` | The scrolling payout certificates |
| `images/lastsectiob3.png` | "Ready to Elevate Your Trading?" background |

---

## 💬 How To Change Text Content

Most text is directly in the `.html` files. Open the file in a code editor, use **Ctrl+F** (or Cmd+F on Mac) to search for the text you want to change, then type the new text.

**Examples:**

| What to change | Search for |
|---|---|
| Support email | `support@noblefunded.com` |
| WhatsApp number | `2349070552755` |
| Promo code in banner | `NOBLE25` |
| Company name in footer | `Noble Funded Ltd` |
| Discord link | `discord.gg/ScpkyxPec9` |
| Telegram handle | `@noblefunded` |
| Twitter/X handle | `x.com/noblefunded` |

---

## 📦 How To Deploy Changes

This site auto-deploys when you push to GitHub. The process is simple:

```bash
# Step 1: Add your changed files
git add .

# Step 2: Write a short message describing what you changed
git commit -m "describe what you changed here"

# Step 3: Push to GitHub (Vercel will pick it up automatically)
git push
```

Wait about 1-2 minutes after pushing, then check your Vercel dashboard or visit the live site to confirm the changes went through.

---

## 🔴 Things That Are NOT Done Yet (Owner Hasn't Asked For These)

These are things that may be needed in the future but have not been requested or built yet:

- [ ] **Real form email delivery** — The contact form needs Formspree or similar (see above)
- [ ] **Login/Signup pages** — Depends on dashboard choice (see above)
- [ ] **Blog / Journal page** — Not built
- [ ] **Google Analytics** — No analytics is set up. See the existing README for how to add it.
- [ ] **Facebook Pixel** — Not installed. See existing README.
- [ ] **Intercom / Live Chat** — Not installed. See existing README.
- [ ] **Giveaway popup** — There's a popup in the home page JS that used to collect emails. It no longer works. The owner hasn't asked to fix it yet.
- [ ] **Payout certificate images** — The images in `/images/payouts/` may still show old branding. Check them and replace if needed.
- [ ] **Documents folder** — The footer links to Terms & Conditions, Privacy Policy, Trading Rules, and KYC Policy PDFs. Make sure these PDFs exist in the `/documents/` folder.

---

## ⚠️ Very Important Rules — Do Not Break These

1. **Never edit anything inside `_next/static/`** unless you really know what you're doing. This is compiled code and is extremely fragile.

2. **The `contact-us.html` has a hydration blocker** at the top of the `<body>`. Do NOT delete it or the new contact page design will disappear and the old one will come back.

3. **Always test locally before pushing.** Run a local server like this:
```bash
python3 -m http.server 3000
# Then open http://localhost:3000/index.html in your browser
```

4. **Commit your work before making big changes.** That way if something breaks, you can go back:
```bash
git checkout -- contact-us.html   # undo changes to one file
git checkout -- .                  # undo ALL uncommitted changes
```

5. **The site uses Tailwind CSS classes** (like `flex`, `text-white`, `bg-[#002B36]`). These styles come from a precompiled CSS file. You can add custom inline styles (e.g. `style="color:red"`) but don't try to add new Tailwind classes that aren't already in the CSS — they won't work.

---

## 🌐 Useful Links

| Link | What It Is |
|---|---|
| [https://noblefunded.com](https://noblefunded.com) | Live website |
| [https://dashboard.noblefunded.com](https://dashboard.noblefunded.com) | Trader dashboard (external) |
| [https://checkout.noblefunded.com](https://checkout.noblefunded.com) | Checkout app (separate Vercel project) |
| [https://github.com/Damcheck/Noble-Frontend](https://github.com/Damcheck/Noble-Frontend) | GitHub repo for this site |
| [https://formspree.io](https://formspree.io) | Recommended contact form backend |

---

## 🆘 If Something Breaks

1. Check what you last changed: `git diff`
2. Undo the last commit: `git revert HEAD`
3. Or restore a specific file: `git checkout -- filename.html`
4. Open the file in a browser locally first **before pushing** to make sure it looks right

---

*Last updated: March 2026 | Built with ❤️ for Noble Funded*
