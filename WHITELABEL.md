# K&L Counsel — White-Labeled AnythingLLM Fork

**Fork of:** `Mintplex-Labs/anything-llm` (MIT License)
**GitHub:** `Apope56/anything-llm-kl` (branch `whitelabel/keith-lorfing`)
**Client:** Keith & Lorfing Law Firm (West Texas)
**Consultant:** Austin Pope / Apope56
**Date:** 2026-05-04

---

## Overview

This is a fully white-labeled fork of AnythingLLM (Docker/server version) for Keith & Lorfing. All user-facing branding ("AnythingLLM", "Mintplex Labs") has been replaced with K&L Counsel branding. The backend (`/server`) is unmodified and binary-compatible with upstream.

---

## Brand Tokens

Extracted from live DOM analysis of `https://lorfinglaw.com/` on 2026-05-04.

| Token | Value | Usage |
|-------|-------|-------|
| `#0B0B0B` | Primariy background | All core surfaces |
| `#111111` | Surface lift | Cards, panels |
| `#141414` | Card bg | Home/dashboard cards |
| `#18181B` | Input bg | Text inputs, popups, menus |
| `#0A0A0A` | Sidebar | Deepest UI element |
| `#2E2E2E` | Borders | Subtle dividers |
| `#E66C0A` | **K&L Orange** | CTA buttons, links, active states, accents |
| `#C02B0A` | Rust/deep press | Button hover, active-press on orange |
| `rgba(255,255,255,0.92)` | Primary text | Strong body text on dark |
| `rgba(255,255,255,0.65)` | Secondary text | Muted labels, descriptions |
| `rgba(255,255,255,0.35)` | Placeholder | Input placeholders |

**Typography:**
- UI/Body: `Figtree, sans-serif` (Google Fonts CDN)
- Headings: `Playfair Display, Georgia, serif` (.font-display utility)

---

## Files Modified (24 files, +327/−74 lines vs upstream)

### New Files
- `frontend/src/styles/brand-keith-lorfing.css` — Theme override CSS (245 lines, ~80 variables)
- `frontend/src/media/logo/kl-logo.png` — Scaled K&L wordmark (732×270)
- `frontend/src/media/logo/kl-logo-dark.png` — Inverted for light theme
- `frontend/src/media/logo/kl-icon.png` — 512×512 favicon icon
- `frontend/src/media/illustrations/kl-login-logo.svg` — Login page SVG
- `frontend/src/media/illustrations/kl-login-logo-light.svg` — Light theme variant
- `frontend/public/kl-favicon.png` — 256×256 browser favicon
- `frontend/public/kl-favicon.ico` — Multi-resolution Windows .ico

### Branding Changes
- `frontend/index.html` — Title, meta tags, favicon references, Google Fonts CDN
- `frontend/public/manifest.json` — PWA name → "K&L Counsel"
- `frontend/src/LogoContext.jsx` — All 4 logo imports + 2 fallback references → kl-* assets
- `frontend/src/main.jsx` — Import brand CSS after index.css
- `frontend/tailwind.config.js` — Added Figtree to sans, new `display` font family

### UI String Replacements
- `frontend/src/components/Footer/index.jsx` — Removed GitHub/Discord/Docs links; replaced with firm site + IT support contact
- `frontend/src/components/Modals/Password/SingleUserAuth.jsx` — "AnythingLLM" default → "K&L Counsel"
- `frontend/src/components/Modals/Password/MultiUserAuth.jsx` — Same as above
- `frontend/src/pages/OnboardingFlow/Steps/Home/index.jsx` — "AnythingLLM" → "K&L Counsel" + font-display
- `frontend/src/pages/OnboardingFlow/Steps/Survey/index.jsx` — mailToMintplex → firmContact
- `frontend/src/locales/en/common.js` — Survey title, mobile app, various labels
- `frontend/src/pages/Admin/Workspaces/WorkspaceRow/index.jsx` — Deletion confirmation text
- `frontend/src/utils/paths.js` — Added firmSite() and firmContact(); updated github() to fork URL

### Icon Replacement
- `frontend/src/components/ProviderPrivacy/index.jsx` — AnythingLLMIcon → KLIcon
- `frontend/src/components/ProviderPrivacy/constants.js` — Same
- `frontend/src/pages/WorkspaceSettings/ChatSettings/WorkspaceLLMSelection/index.jsx` — Same

---

## Build Commands

### Frontend (dev)
```bash
cd frontend
yarn install
yarn dev
```

### Frontend (production build)
```bash
cd frontend
yarn build        # Output in frontend/dist/
```

### Docker image
```bash
docker build --no-cache -t kl-counsel:latest -f docker/Dockerfile .
```

### Run container
```bash
mkdir -p /path/to/storage
docker run -d --name kl-counsel -p 3001:3001 \
  --cap-add SYS_ADMIN \
  -e STORAGE_DIR=/app/server/storage \
  -v /path/to/storage:/app/server/storage \
  kl-counsel:latest
```

---

## Upstream Merge Procedure

1. Fetch upstream: `git fetch upstream`
2. Checkout whitelabel branch: `git checkout whitelabel/keith-lorfing`
3. Merge: `git merge upstream/master`
4. Resolve conflicts (only in `frontend/` files — never in `server/`)
5. Build check: `cd frontend && yarn build`
6. Rebuild Docker: `docker build --no-cache -t kl-counsel:latest -f docker/Dockerfile .`
7. Smoke test: start container, curl port 3001, verify title contains "K&L Counsel"

---

## License

Upstream: MIT License (Mintplex Labs, Inc.)
White-label modifications: same MIT License
K&L logo assets: property of Keith & Lorfing Law Firm

---

## Brand Reusability

To re-skin for another law firm:
1. Replace the 7 kl-* asset files with new firm logo variants
2. Edit `frontend/src/styles/brand-keith-lorfing.css` — only change values in the `:root` block
3. Update `frontend/src/locales/en/common.js` — search/replace "K&L Counsel" with new name
4. Update `frontend/src/pages/OnboardingFlow/Steps/Home/index.jsx` — firm name text
5. Update `frontend/src/utils/paths.js` — firmSite() and firmContact() URLs
6. Rebuild Docker image
