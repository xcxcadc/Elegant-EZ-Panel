**Comparison target**

- Source visual truth (dashboard): `C:\Users\AAA\AppData\Local\Temp\codex-clipboard-55540f92-f2b2-4791-b7e8-a845a507d4e5.png` at 952 × 692 px, authenticated dashboard state.
- Source visual truth (account menu / logout): `C:\Users\AAA\AppData\Local\Temp\codex-clipboard-420ef33f-c042-4f53-99c0-85719ad7da46.png` at 954 × 563 px, account menu open.
- Source visual truth (shop): `C:\Users\AAA\AppData\Local\Temp\codex-clipboard-f7988184-5a77-40c4-8edf-983e20b76d05.png` at 1280 × 582 px, selected \"all\" plan filter.
- Reported pre-fix implementation evidence: `C:\Users\AAA\AppData\Local\Temp\codex-clipboard-da515b0a-0f03-4343-8a1d-77d8b8019e38.png` at 912 × 580 px.
- Intended implementation routes: `http://localhost:4173/?preview=1#/dashboard` and `http://localhost:4173/?preview=1#/shop`.
- Intended state: desktop, Simplified Chinese, authenticated review preview; dashboard has subscription information and shop has at least two purchasable plans.

**Evidence status**

- The four source images above were opened at native size. No density normalization was needed for source review.
- The local Vite server is listening on port 4173 and returned HTTP 200. Production build completed with `npm.cmd run build`.
- The agent does not have an in-app Browser control in this thread, so it cannot take a browser-rendered post-fix screenshot, inspect console output, exercise the account-menu click, or compose the required same-view comparison image. No implementation screenshot path is available. Visual QA is therefore blocked rather than inferred from the source code.

**Findings**

- [P0 fixed in code] Preview logout was visually inert.
  Location: `src/router/index.js`, `src/components/common/UserAvatar.vue`.
  Evidence: the visual-review guard recreated `elegant-preview` on every navigation after the avatar menu removed it, so the login route redirected back into the dashboard.
  Fix: the avatar now sets a session-scoped preview-logout sentinel before removing the fake token; the guard respects it for protected routes and preserves a subsequently issued real API token.

- [P1 fixed in code] The redesigned subscription card omitted the existing EZ online-client count.
  Location: `src/views/dashboard/Dashboard.vue`.
  Evidence: `fetchSubscribe` already stores `alive_ip` and `device_limit`, while the visible card only showed traffic and expiry data.
  Fix: the card now renders `在线客户端` as `alive_ip / device_limit`, with `不限` for an unlimited device cap.

- [P1 fixed in code] Shop filter and primary card hierarchy did not match the supplied Elegant reference.
  Location: `src/assets/styles/elegant-theme.scss`.
  Evidence: the reported implementation used a black selected filter and a disconnected lavender first-card background; the source uses a distinct featured-card surface and clear selected filter state.
  Fix: introduced a shared fresh palette: mint featured-card surface and primary actions, sky-blue selected filter and secondary purchase action, and amber for stock-warning emphasis. The first plan retains its elevated, coloured hierarchy without a dark fill.

- [P2 fixed in code] Blue-violet controls created a disconnected visual system across dashboard, shop, ticket, and import controls.
  Location: `src/assets/styles/elegant-theme.scss`.
  Fix: replaced shared primary tokens with mint (`#149982`), light mint surfaces, sky data accents (`#4f91d6`), and warm amber emphasis (`#d29b48`). Traffic bars now use flat data colours rather than decorative gradients.

**Required fidelity surfaces**

- Fonts and typography: retains Inter / PingFang SC system stack and existing card typography. Browser-side wrapping and optical weight still need capture.
- Spacing and layout rhythm: keeps the reference-inspired rounded-card grid. Adding one online-client row keeps the existing plan-card rhythm; browser-side card height and responsive wrapping still need capture.
- Colors and visual tokens: static review confirms all shared primary controls now resolve to mint, selected shop filters to sky, and stock warning to amber. Browser-side contrast and scoped-style precedence still need capture.
- Image quality and asset fidelity: retains the existing mountain hero and original configured client icons; no replacement image or placeholder artwork was introduced.
- Copy and content: adds `在线客户端` using the existing subscription API fields. Live value depends on the connected panel account.

**Comparison history**

1. Reported implementation review found inert preview logout, missing online-client visibility, black shop filter, and a lavender card that did not fit the desired fresh system.
2. Applied session-scoped preview logout, `alive_ip` / `device_limit` display, and mint / sky / amber shared tokens.
3. A post-fix browser screenshot at the matching route and viewport is unavailable to the agent; the visual comparison loop cannot be completed.

**Implementation checklist**

- [x] Ensure preview logout reaches the login screen instead of recreating the review token.
- [x] Reuse original subscription API fields for online-client statistics.
- [x] Restore shop featured-card hierarchy with a theme-aligned surface.
- [x] Replace black and blue-violet shared primary controls with fresh composite tokens.
- [x] Build production assets successfully.
- [ ] Capture dashboard and shop in the user-selected in-app Browser, test logout and shop filter, and compare at the matching viewports.

**Follow-up polish**

- [P3] If a card grid has a business-defined recommended plan rather than a first plan, bind the mint featured treatment to that API flag instead of visual order.

final result: blocked
