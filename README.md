# Ritual — site package

## Structure
- `index.html` — landing page. Hero section autoplays `assets/hero-moon.mp4` behind the logo and headline.
- `login.html` — sign-in page, same navy/gold/terracotta theme, email+password form with client-side validation, Google/Apple button stubs, show/hide password.
- `app.html` — the calendar app shell: sticky header, logout, and an auth guard that checks for a session before showing the calendar.
- `app-widget.html` — your original calendar (paper day-page + month view) wrapped as a standalone document, loaded into `app.html` via `<iframe>` so its internal script/CSS stay isolated.
- `css/theme.css` — shared design tokens (colors, type, buttons) used by `index.html`, `login.html`, `app.html`.
- `assets/logo.png`, `assets/hero-moon.mp4` — your uploaded logo and hero video.

## Auth (demo only — replace before shipping)
`login.html` currently writes a fake session to `sessionStorage` on submit and redirects to `app.html`, which checks for that session and shows a "you're signed out" gate if it's missing.

Before production:
1. Replace the `fetch`/`setTimeout` stand-in in `login.html`'s submit handler with a real call to your auth API (e.g. `POST /api/login`), and store a real token/cookie instead of the demo `sessionStorage` value.
2. Wire the Google/Apple buttons to your OAuth flow.
3. Do the real session check in `app.html` against your backend/token, not just "is something in sessionStorage."
4. Add a real sign-up page and point `#toSignup` at it.
5. Serve over HTTPS; set cookies `HttpOnly`/`Secure` if you switch from sessionStorage to cookies.

## Running locally
Any static server works, e.g.:
```
npx serve .
# or
python3 -m http.server 8080
```
Then open `http://localhost:8080/index.html`.

## Deploying
This is fully static — drop the folder on Netlify, Vercel, GitHub Pages, S3+CloudFront, or any static host. No build step required.
