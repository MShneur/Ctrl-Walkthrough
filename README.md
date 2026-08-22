# 🧭 Control Walkthrough

**Setup guides are usually very good at telling you to “click Settings.” Control Walkthrough would rather click Settings for you.**

Control Walkthrough is a public, AI-neutral userscript for turning setup and repair instructions into **small, verified, increasingly autonomous walkthroughs** across real websites.

Instead of dumping a 40-step checklist on somebody, it tries to behave like GPS:

> **Here’s where you are. Here’s the one thing that matters now. I’ll do the safe parts. I’ll stop when you need to decide.**

Current engine: **v0.5.1**

---

## What it does

A walkthrough can:

- open an exact HTTPS page instead of making you hunt through menus;
- wait for slow/mobile/SPA controls to appear;
- safely click reversible navigation controls;
- fill ordinary fields without submitting them;
- temporarily remember a secret **locally in Tampermonkey** so you do not have to memorize it;
- move its side rail away from the field or button you need;
- verify that an action actually reached the expected page/state before continuing;
- observe a user-confirmed consequential action and continue after the result is verified;
- stop instead of guessing when a locator is ambiguous or the site changed.

The default experience is **novice mode**: assume the person may be on a phone, tired, distracted, young, older, or simply unfamiliar with the product. Expert mode can be more compressed, but safety and verification do not disappear.

## Install

Control Walkthrough runs as a userscript. Install a compatible userscript manager such as Tampermonkey, then open the raw script:

[`ctrl-walkthrough.user.js`](ctrl-walkthrough.user.js)

The userscript’s update URL points back to this repository, so future releases can update from here.

## The rule that matters most

**Stable deep link → verified safe click → safe fill → highlight → manual fallback.**

If Control Walkthrough can reliably do something harmless for the user, it should do it. If it cannot prove what it is about to touch, it should stop.

Consequential controls remain human-gated: login/MFA/CAPTCHA, OAuth or permission consent, billing, purchases, account/security changes, final deploy/publish/delete actions, token rotation, and similar irreversible or externally meaningful actions.

## 🔐 Public means public. Forever-ish.

This repository is intentionally public. Treat every commit, issue, PR, screenshot, example, walkthrough, and handoff as something that may be copied or retained indefinitely.

**Never put any of these here:**

- passwords or passphrases;
- API keys, bearer tokens, PATs, OAuth secrets, cookies, SSH/private keys, recovery codes;
- private hostnames, private IPs, internal-only URLs, account identifiers, billing details;
- personal information or private project data;
- screenshots that expose credentials or private account state;
- secrets hidden inside JSON, Base64, CWZ2/CW2, comments, “temporary” files, deleted commits, or examples.

Encoding is not encryption. Deleting a public Git commit later is not a privacy mechanism.

When a walkthrough needs a secret, the **walkthrough source contains only the secret’s name/instructions**. The value belongs in the provider’s secret store or, for short-lived setup convenience, temporary local Tampermonkey storage. See [`SECURITY.md`](SECURITY.md).

## How walkthroughs work

The engine is deliberately separated from the instructions:

```text
ctrl-walkthrough.user.js   reviewed engine
manifest.json              public guide catalog
modules/*.json             declarative walkthroughs
AUTHORING_RULES.md         how humans/AIs should write guides
AI_HANDOFF_PROTOCOL.md     private/public handoff rules
make_handoff.py            CWZ2/CW2 handoff generator
```

A walkthrough is data, not arbitrary remote JavaScript. That means ChatGPT, Claude, Gemini, Copilot, Perplexity, a local model, or a human can generate the same schema without needing a proprietary integration.

## For AIs writing a walkthrough

Read [`AUTHORING_RULES.md`](AUTHORING_RULES.md) first.

The short version:

1. Explain unfamiliar product nouns the first time they appear.
2. Research a stable direct URL before teaching menu archaeology.
3. Treat mobile and desktop as different UI breakpoints.
4. Make each step one task and one obvious action.
5. Automate reversible navigation/filling when the exact target is known.
6. Give every automated action a real success predicate when one exists.
7. Never ask the user to press “Next” merely to acknowledge something Control can already verify.
8. Keep consequential actions human-gated.
9. Never put a real secret in a walkthrough or handoff.
10. Preserve the true end-to-end verification condition at the finish.

## Private and custom guides

Public reusable guides can live in `modules/`.

Private/project-specific guides should **not** be published here. They can be imported locally as a `.walkthrough.json` file or handed off through the supported temporary formats described in [`AI_HANDOFF_PROTOCOL.md`](AI_HANDOFF_PROTOCOL.md).

Temporary local secret memory is convenience storage, not a password manager and not cross-browser cloud sync.

## Browser model

The engine uses ordinary provider pages plus Tampermonkey state. It does not depend on iframes because auth/admin sites frequently block or alter iframe behavior.

The compact rail can live on either edge of the viewport and automatically move away from a highlighted target. State is local to the browser/profile by design.

## Contributing

Useful contributions include:

- a current mobile/desktop locator correction;
- a stable deep link that replaces three menu clicks;
- a better verification predicate;
- a public-safe walkthrough for a commonly used tool;
- an accessibility or cognitive-load improvement;
- a failure report that includes what the site actually showed.

Before submitting anything, run the privacy test: **could this commit safely remain public forever?** If not, do not submit it.

## History

Control Walkthrough started inside the [`Agents-of-AI`](https://github.com/MShneur/Agents-of-AI) tools shelf while the format was being proven. It became its own repository once the engine grew into a reusable project rather than one supporting script. See [`CHANGELOG.md`](CHANGELOG.md).

---

## The AI Duct Tape Collection

The future arrived early, forgot the manual, and left six tabs asking for an API key. These projects are different strips of duct tape for that situation:

- **[CTRL-AI](https://github.com/MShneur/CTRL-AI)** — Governance: evidence, dissent, uncertainty, and rules for how the AI should behave.
- **[Agents of AI](https://github.com/MShneur/Agents-of-AI)** — The cast: reusable personas, agents, workflows, teams, techniques, and failure modes.
- **[R-Duck](https://github.com/MShneur/R-Duck)** — The operator: turns fuzzy intent into structured autonomous work.
- **[Ghost in the Loop](https://github.com/MShneur/ghost-in-the-loop)** — The handoff lane: move/export work between AI sessions without dropping the context down the stairs.
- **[CTRL-FORGE](https://github.com/MShneur/ctrl-forge)** — The memory/control layer: durable project state when the chat window is not enough.
- **[Control Walkthrough](https://github.com/MShneur/Ctrl-Walkthrough)** *(this repo)* — The hands: guide, click, fill, verify, and get out of the way.

Use one. Use several. Please do not use a public repository as a password manager.
