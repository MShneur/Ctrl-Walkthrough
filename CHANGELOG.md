# Changelog

Material changes to Control Walkthrough are recorded here.

## Docs — 2026-10-20 (no engine release)

- Authoring rules now require a preparatory **inspect uploaded HTML/screenshot evidence before asking for repeat navigation** step, exact ready-to-paste values only when grounded, and explicit DNS-record versus authoritative-nameserver separation. Links to the independent offline, read-only AoA source-role extractor; no userscript/runtime version changed.

## 0.5.1 — 2026-08-22

### Standalone repository

- Moved Control Walkthrough out of the `Agents-of-AI/tools/ctrl-walkthrough/` incubator into its own public repository.
- Updated the canonical manifest and userscript update/download URLs to this repository.
- Kept the old Agents-of-AI location as a compatibility/migration path so existing installs can move forward without starting over.
- Added a repository-level public-safety boundary: no credentials, private account state, personal data, private infrastructure, or secret-bearing handoffs in public GitHub history.
- Added ecosystem links so Control Walkthrough sits alongside CTRL-AI, Agents of AI, R-Duck, Ghost in the Loop, and CTRL-FORGE rather than pretending to be the only strip of duct tape in the drawer.

## 0.5.0 — 2026-08-22

### Autonomous verified navigation

- Renamed the visible surface to **Control Walkthrough**.
- Replaced the mobile bottom sheet with a compact left/right edge rail that can move away from the target control.
- Added wait-for-target behavior for slow mobile and SPA interfaces.
- Added visible-element locator scoring and ambiguity fail-closed behavior.
- Added persistent pending actions across full page navigation.
- Added verify-before-advance semantics.
- Added `fill` and `fillSaved` actions; safe field filling never submits a form.
- Added `advanceOn` so a user-confirmed consequential click can be observed and verified without requiring a second redundant “Next” press.
- Added autonomous execution for safe deep links, exact safe clicks, safe fills, and selected local-memory actions.
- Preserved human gates for login/MFA/CAPTCHA, permission consent, billing/purchases, account/security changes, final create/deploy/publish/delete actions, and token rotation.

## 0.4.0 — 2026-08-21

### Novice-first navigation HUD

- Reworked the original panel into a smaller current-step navigator.
- Added `Why:` and `Next:` context while hiding the full route behind a separate route view.
- Added safe exact-target click actions.
- Added local `download`, `generate`, `capture`, and `copySaved` actions.
- Added temporary Tampermonkey-local private-value storage, cleared when a walkthrough ends.
- Hardened CWZ2/CW2 import against whitespace, Markdown fences, zero-width characters, soft hyphens, and common mobile copy/paste damage.
- Added expiry cleanup for temporary handoffs.

## 0.3.x

- Added custom/private walkthrough handoffs and best-effort private GitHub pointers.
- Established CWZ2/CW2 portable handoff formats.

## 0.2.x

- Moved public-safe walkthroughs to a public manifest and added local custom import.

## 0.1

- Early private-token prototype. Retired.
