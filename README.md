# PriceHistory (React)

A small **React** front-end project for **Front End Engineering-II
(25CSE0203)**. It shows product cards with their current, highest and
lowest recorded price, lets you search/filter products, view a
price-history chart for any product, and see recommendations for
similar products.

There is **no backend and no third-party API** anywhere in this
project — no `.env` file, no API keys, no `fetch()` calls to any
external service. All product data lives in one plain JavaScript file:
`src/data/products.js`.

## About the product data

`src/data/products.js` ships with **25 real products** you can actually
find on Amazon.in and Flipkart.com (Nike deodorant combo, boAt Rockerz/
Airdopes, Noise/Fire-Boltt smartwatches, Studds helmet, Wildcraft
backpack, Milton bottle, Prestige induction cooktop, etc.) across 6
categories. There's still no backend and no paid pricing API (none
exist for free), so the `history` arrays are realistic sample data
built around each product's real, current market price.

**Links:** `getProductUrl()` builds a live **Amazon.in / Flipkart.com
search link** for each product's exact name instead of a single
hard-coded `/dp/ASIN` or `/p/...?pid=...` URL. A frozen permalink
breaks the moment that exact listing is delisted or its ID changes; a
search link always loads and always shows the real product.

## Getting started

You need [Node.js](https://nodejs.org) installed (v18 or newer).

```bash
npm install
npm run dev
```

Then open the URL shown in the terminal (usually `http://localhost:5173`).

To build a production-ready static bundle:

```bash
npm run build
npm run preview
```

## Project structure

```
pricehistory-react/
├── index.html                 Vite entry HTML
├── package.json
├── vite.config.js
├── EVALUATION_NOTES.md        Viva preparation sheet
└── src/
    ├── main.jsx                 Entry: BrowserRouter + Context providers
    ├── App.jsx                   All routes (inside Layout) + 404
    ├── index.css                  All styling
    ├── context/
    │   ├── WishlistContext.jsx      Global wishlist state
    │   └── BookingContext.jsx       Global travel bookings state
    ├── utils/
    │   └── validators.js            Form validation functions
    ├── data/
    │   ├── products.js              Product "database" + helpers
    │   ├── api.js                   Mock async API (delay, errors)
    │   ├── travel.js                Mock travel options
    │   └── stores.js                Store list
    ├── components/
    │   ├── Layout.jsx                Header + <Outlet /> + Footer
    │   ├── Header.jsx                 NavLink menu, live counts, mobile menu
    │   ├── ScrollToTop.jsx            Scroll on route change / #section
    │   ├── Footer.jsx
    │   ├── FormInput.jsx              Reusable label + input + error
    │   ├── TrackUrlForm.jsx           Product link form with validation
    │   ├── ProductCard.jsx / ProductGrid.jsx / WishlistButton.jsx
    │   ├── CategoryChips.jsx
    │   └── PriceChart.jsx             Canvas line chart (useRef + useEffect)
    └── pages/
        ├── Home.jsx / PriceHistory.jsx / ProductPage.jsx / Compare.jsx
        ├── Travel.jsx / Bookings.jsx / Wishlist.jsx / Stores.jsx
        └── NotFound.jsx                404 page
```

## Where the rubric items show up

- **JSX / Functional Components** — every file in `components/` and
  `pages/` is a functional component written in JSX (no class
  components).
- **Props** — `ProductCard`, `ProductGrid`, `CategoryChips` and
  `PriceChart` all receive data through props instead of hard-coding
  anything.
- **State (`useState`)** — `Home.jsx` uses `useState` for the search
  box text, the active filter and the search results.
  `ProductPage.jsx` uses `useState` to hold the currently loaded
  product.
- **`useEffect`** — `ProductPage.jsx` reloads the product whenever the
  URL's `:id` changes; `PriceChart.jsx` redraws the canvas whenever the
  `history` prop or the window size changes; `Home.jsx` has a small
  effect that runs once on mount.
- **Reusable components** — `ProductCard` is written once and reused
  on the Home page (Trending / Deals and Drops / Search Results) and
  on the Product page (Recommended for you).
- **DOM manipulation** — `PriceChart.jsx` uses a `ref` to get the real
  `<canvas>` DOM node and draws directly on it with the Canvas API.

## Adding a new product

Open `src/data/products.js` and add a new object to the `PRODUCTS`
array with a unique `id`, `title`, `category`, `store`, `url`,
`currentPrice` and a `history` array of past prices. Every page picks
it up automatically — no other file needs to change.

## GitHub

Commit this folder as your project repository (the `.gitignore`
already excludes `node_modules`). Push regularly so the evaluator can
see your commit history, as required by the rubric.

## Evaluation additions

- **Routes** (`App.jsx`): `/`, `/price-history`, `/compare`, `/travel`, `/bookings`, `/wishlist`, `/stores`, `/product/:id`, `*` (404).
- **Context API**: `context/WishlistContext.jsx` and `context/BookingContext.jsx` (providers in `main.jsx`, read with `useWishlist()` / `useBooking()`).
- **Async states**: `data/api.js` and `data/travel.js` are mock Promise-based APIs; pages show loading / success / empty / error.
- **Forms**: `components/FormInput.jsx` is the reusable field; rules live in `utils/validators.js`.
- Demo the error state by searching the word `error` on the Home page.
