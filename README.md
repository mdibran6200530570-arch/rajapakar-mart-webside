# Rajapakar Mart — Amazon-style E-Commerce Frontend

A complete shopping UI built with **plain HTML, CSS and vanilla JavaScript**.
No frameworks, no build step, no Node/npm required — open `index.html` and it runs.

---

## Quick start

**Easiest:** just double-click `index.html`.

**Or run a local server** (recommended — `localStorage` is restricted on some
`file://` origins). PowerShell is all this machine needs:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File tools\serve.ps1 -Port 8000
```

Then open <http://localhost:8000>.

If you have Python or Node installed, `python -m http.server 8000` or `npx serve .`
work too.

---

## Pages

| File | What it does |
|---|---|
| `index.html` | Homepage: hero carousel, department grid, deal countdown, product rails, top picks, value props |
| `products.html` | Listing: department / brand / price / rating / discount / delivery / colour filters, 6 sort modes, grid + list view, pagination |
| `product.html` | Detail: image gallery with hover zoom, colour variants, quantity, offers, buy box, tabs (description / specs / reviews with histogram / Q&A), related products |
| `cart.html` | Cart: quantity controls, save-for-later, remove, working coupons, live totals |
| `checkout.html` | Address form with validation, 5 payment methods, place-order flow |
| `order-confirmed.html` | Success screen, delivery timeline, invoice summary, printable |
| `orders.html` | Order history with per-order tracking timeline, rebuy, advance status |
| `wishlist.html` | Wishlist with bulk add-to-cart and clear |
| `admin.html` | Seller Central: password-protected dashboard (orders, product price/stock editor, settings). Demo login: `mdibran6200530570@gmail.com` / `mdibran@123` |
| | **Add Product:** Products tab → "＋ Add New Product" — title, brand, department, price, MRP, stock, photo. Naya product turant homepage, search, listing aur detail page par live ho jata hai (ID `CZ…`, `NEW` badge). Admin-added products delete bhi ho sakte hain. |

---

## Project structure

```
├── index.html            Homepage
├── products.html         Listing / search results
├── product.html          Product detail
├── cart.html             Shopping cart
├── checkout.html         Address + payment
├── order-confirmed.html  Order success
├── orders.html           Order history
├── wishlist.html         Wishlist
├── assets/
│   ├── css/style.css     Single stylesheet, CSS custom properties, no framework
│   ├── js/
│   │   ├── data.js       Departments + 36 sample products  ← edit this first
│   │   ├── store.js      State layer (localStorage) + money/format helpers
│   │   ├── ui.js         Header, mega-menu, footer, toasts, auth modal, product card
│   │   ├── home.js       Homepage rendering
│   │   ├── products.js   Filters, sorting, pagination (state synced to URL)
│   │   ├── product-detail.js
│   │   ├── cart.js  checkout.js  order-confirmed.js  orders.js  wishlist.js
│   ├── img/*.svg         33 generated product illustrations
└── tools/
    ├── serve.ps1         PowerShell static server (no Node/Python needed)
    └── gen-images.ps1    Regenerates the SVG images
```

---

## How state works

Everything persists in `localStorage` under one key, `shopzone.v1`:

```json
{
  "cart":     [{ "id": "SZ1001", "qty": 2, "color": "Black" }],
  "wishlist": ["SZ2002"],
  "orders":   [ /* order history */ ],
  "user":     { "name": "...", "email": "..." },
  "addresses":[ /* saved addresses */ ]
}
```

Any change fires a `store:change` DOM event, so the header cart badge updates
everywhere automatically. Clear everything from the browser console with:

```js
Store.reset()
```

---

## Coupon codes (working in the demo)

| Code | Effect |
|---|---|
| `SAVE200` | Flat ₹200 off orders over ₹1,000 |
| `NEW10` | 10% off, capped at ₹800, orders over ₹999 |
| `FREESHIP` | Free shipping on any order |

---

## Making it real

This is a front-end demo. To connect a backend:

1. **Products** — replace the `PRODUCTS` array in `assets/js/data.js` with a
   `fetch()` against your API. Keep the same object shape and every page keeps working.
2. **Cart / wishlist / auth** — replace the `localStorage` writes in `store.js`
   with API calls. The store already exposes a clean CRUD surface.
3. **Payments** — wire the checkout `payment` branch in `checkout.js` to
   Razorpay or Stripe Checkout instead of the fake `setTimeout`.
4. **Search** — `Store.search()` is a client-side scorer; swap it for an
   Elasticsearch or Typesense endpoint for real relevance at scale.

## Notes

- No real payments are processed and nothing leaves the browser.
- Product images are copyright-free photos (Unsplash License: free for commercial
  use, no attribution required, no copyright strike) stored locally in
  `assets/img/photos/`, so the site works fully offline and deploys to Firebase
  as-is. Refresh them any time with:
  `powershell -NoProfile -ExecutionPolicy Bypass -File tools\fetch-images.ps1`
- Department icons in `assets/img/*.svg` are original illustrations made for this
  project — also 100% copyright-safe. Replace the files in `assets/img/photos/`
  with real photos when you have them — the filenames are referenced from `data.js`.
- "Sign in" is a demo modal that only stores a name and email locally.