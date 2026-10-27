# Evaluation Prep Sheet

## 1. How the project works
PriceHistory shows products with current/highest/lowest price, a price-history chart, search, compare, a wishlist, and a travel booking flow. All data is local (`src/data`) - no backend, no API keys. `data/api.js` wraps it in Promises with a short delay so it behaves like a real API.

## 2. React Router / SPA
- `main.jsx` wraps the app in `<BrowserRouter>`; `App.jsx` lists every `<Route>`.
- `Layout.jsx` renders Header + `<Outlet />` + Footer once; the Outlet is replaced by the matching page. No page reload = Single Page App.
- `<Link>` / `<NavLink>` navigate without reloading. `useParams` reads `:id` on `/product/:id`. `useSearchParams` reads `?a=` on `/compare`. `useNavigate` redirects after booking or URL lookup.
- `path="*"` shows `NotFound` (404).

## 3. Local state vs Context API
- **Local (`useState`)**: data one component needs. Example: form fields and search status in `Travel.jsx`, selected products in `Compare.jsx`, menu open/closed in `Header.jsx`.
- **Global (Context)**: data many components share. Wishlist: heart button (ProductCard), Wishlist page and Header count all read the same list. Bookings: Travel adds, My Trips lists, Header counts.
- How: `createContext` -> `Provider` wraps the app in `main.jsx` -> `useContext` (inside `useWishlist` / `useBooking`) reads it. This removes "prop drilling".

## 4. Forms and reusable components
- `FormInput.jsx` = label + input/select + error text; used in Travel, Compare, TrackUrlForm.
- Validation functions in `utils/validators.js` return an error message or "". On submit: `preventDefault`, validate, show errors, `return` if invalid.
- Rules: search 2-50 chars; URL must be valid Amazon.in/Flipkart.com; travel needs from/to (different), future date, 1-6 travellers; passenger needs name, email format, 10-digit mobile.

## 5. Loading and error handling
Pattern: `status` state = `idle | loading | success | empty | error`, set inside `try/catch` around an `await`.
- Home search: loading text, error box with "Try again", empty message. Type `error` to demo.
- ProductPage: loading, then not-found message for a bad id (try `/product/xyz`).
- Travel: loading, empty (Goa <-> Kolkata), error.
- `ProductPage` uses an `ignore` flag in `useEffect` cleanup so old requests don't overwrite new ones.

## 6. Where each feature lives
| Feature | Files |
|---|---|
| Routing / layout | `App.jsx`, `Layout.jsx`, `Header.jsx`, `ScrollToTop.jsx`, `NotFound.jsx` |
| Price history | `ProductPage.jsx`, `PriceChart.jsx` (canvas, useRef), `products.js` |
| Search / URL tracking | `Home.jsx`, `TrackUrlForm.jsx`, `api.js` |
| Comparison | `Compare.jsx` |
| Travel booking | `Travel.jsx`, `Bookings.jsx`, `travel.js`, `BookingContext.jsx` |
| Wishlist | `WishlistContext.jsx`, `WishlistButton.jsx`, `Wishlist.jsx` |
| Validation | `validators.js`, `FormInput.jsx` |

## 7. Likely viva questions
- **What is a SPA?** One HTML page; React swaps components by URL without reloading.
- **Link vs `<a>`?** Link changes the route client-side; `<a href>` reloads the page.
- **What is `<Outlet>`?** Placeholder in a layout where the matched child route renders.
- **Why Context?** To share state across distant components without passing props through each level.
- **Why not Context for everything?** Local-only data (form fields) is simpler and re-renders less with `useState`.
- **Controlled component?** Input value comes from state (`value` + `onChange`).
- **Why `preventDefault`?** Stops the browser's default form submit/page reload.
- **What does `useEffect` with `[id]` do?** Re-runs when `id` changes, here to reload the product.
- **Why `key` in lists?** Helps React track items efficiently on re-render.
- **Is data real?** Product prices are sample data; there is no paid API. Travel results are generated mock data.
- **How is wishlist saved?** `localStorage`, written in a `useEffect` whenever the list changes.

## 8. Two small bugs worth mentioning (shows you tested edge cases)
- **Stale form (Travel.jsx):** the booking used the live form fields, so editing the route after searching saved the wrong route. Fix: `searched` state keeps a copy of the form at search time.
- **Race condition (Home.jsx):** an old search reply could overwrite newer results. Fix: a `useRef` counter (`latestRequest`); replies with an older number are ignored.

## 9. Git: commit your work
```
git status
git add -A
git commit -m "Add layout routing, Context API, forms, async states and evaluation notes"
git log --oneline -3
```
Optional (only if you already have a remote called origin): `git push`.
