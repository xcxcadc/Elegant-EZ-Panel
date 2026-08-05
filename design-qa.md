**Comparison target**

- Source visual truth: `C:\Users\AAA\AppData\Local\Temp\codex-clipboard-7debc5a3-c735-43d2-958f-549cbc3aacbb.png` (dashboard) and `C:\Users\AAA\AppData\Local\Temp\codex-clipboard-ea0a702c-5f97-4d11-b574-6a4cf5772f1f.png` (subscription import).
- Intended implementation route: `http://localhost:4173/?preview=1#/dashboard`.
- Intended state: authenticated dashboard with subscription link present; click “订阅导入”, then select the Windows tab.

**Evidence status**

- Source images were opened at their native desktop dimensions.
- The implementation compiled successfully with `npm.cmd run build`.
- A browser-rendered implementation screenshot, console inspection, and interaction capture are unavailable in this thread because the in-app Browser control is not exposed to the agent tool registry. The user can access the local preview in the in-app browser, but the agent cannot capture that tab.

**Static review findings and fixes**

- [P1 fixed] Subscription import was missing from the visible dashboard because the original EZ import card lived inside the hidden legacy dashboard container.
  - Fix: added a visible import panel in `src/views/dashboard/Dashboard.vue` with the current subscription URL, copy action, QR dialog, platform tabs, and 30 configured client definitions routed through the original `importToClient` protocol handler.
- [P1 fixed] Primary buttons used opaque black fills that competed with the reference’s lightweight cards and pastel utility accents.
  - Fix: replaced black primary controls with the `#6677e8` / `#5264d4` blue-violet token family and applied the token to dashboard controls plus shared ticket and purchase actions.
- [P2 fixed] The dashboard import affordance did not explain the import workflow or expose platform selection.
  - Fix: added clear “订阅与客户端” hierarchy, responsive platform tabs, app cards, current traffic progress, and a QR fallback.

**Required fidelity surfaces**

- Fonts and typography: code uses the existing Inter / PingFang SC stack; visual verification pending browser capture.
- Spacing and layout rhythm: the import panel uses the same 21px rounded-card and 10px grid rhythm as the dashboard reference; visual verification pending browser capture.
- Colors and visual tokens: changed from black to blue-violet primary controls with low-contrast white cards and pastel borders; visual verification pending browser capture.
- Image quality and asset fidelity: existing client application image assets and the existing mountain hero asset are reused; no placeholder artwork was introduced.
- Copy and content: import copy, QR action, and client labels are present; live subscription data depends on the connected account API.

**Open questions**

- Browser-side verification remains required for the final visual comparison at the user’s active viewport and with their authenticated account data.

**Implementation checklist**

- [x] Restore subscription import into the visible dashboard.
- [x] Keep original EZ client import protocols and app asset mappings.
- [x] Replace visible black primary buttons with blue-violet controls.
- [x] Build production assets successfully.
- [ ] Capture the running UI and compare it visually to the reference at the same viewport.

final result: blocked
