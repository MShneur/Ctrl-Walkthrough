# Control Walkthrough — AI Handoff Protocol

Purpose: let an AI hand a user a temporary, private or reusable walkthrough without exposing credentials or forcing a private-repository token into the public Control Walkthrough userscript.

## Decision rule

Use the smallest safe transport that fits the walkthrough.

| Walkthrough type | Preferred transport | Persistence |
| --- | --- | --- |
| Generic, reusable, public-safe setup | Publish as a canonical module under `modules/` and add it to `manifest.json` | Public/stable |
| Personalized or project-specific, no secrets, normal size | `CWZ2:` gzip + Base64URL paste code | Temporary local handoff |
| Same as above, browser cannot decode gzip | `CW2:` Base64URL UTF-8 JSON | Temporary local handoff |
| Large personalized/project-specific walkthrough, no secrets | Local `.walkthrough.json` file/attachment imported from Control Walkthrough | Local until user removes it |
| Private GitHub walkthrough | `CWG1:` / GitHub blob pointer only as a best-effort convenience when that browser is already proven to support signed-in private import | Temporary if import succeeds |
| Anything requiring passwords, API keys, bearer tokens, SSH keys, recovery codes or other credentials | **Never put the secret in the walkthrough, code, URL or repository.** Generate/capture locally only when needed and store long-lived values in the provider/server secret store | Secret stays outside Git |

## Default for AI-generated private fixes

When a user asks an AI for a Control Walkthrough to fix something, the AI should:

1. determine whether the instructions are generic/public-safe or private/project-specific;
2. create and validate a schema-v2 walkthrough;
3. remove credentials and secret values;
4. if generic and reusable, publish a canonical public module only when publication is authorized;
5. otherwise prefer a self-contained **`CWZ2`** handoff;
6. if the code is impractically large, provide a local `.walkthrough.json` attachment/file instead;
7. automate stable deep links, verified safe clicks and safe fills rather than adding manual acknowledgment steps;
8. tell the user only the minimum import action needed.

The user should not need to create a GitHub Personal Access Token merely to load a private walkthrough.

## Why `CWZ2` is the default

`CWZ2` is:

```text
CWZ2:<Base64URL(gzip(UTF-8 schema-v2 JSON))>
```

Gzip keeps normal walkthroughs compact. Base64URL makes the binary result safer to copy/paste in a normal text box without `+`, `/` or padding problems.

`CW2` is the fallback:

```text
CW2:<Base64URL(UTF-8 schema-v2 JSON)>
```

It is larger but does not require browser gzip decompression.

### Important: compression is not encryption

`CWZ2` and `CW2` are transport encodings. Anyone who possesses the code can decode it. They may carry private/project-specific **instructions** through an already-private conversation, but they must never contain credentials or secret values.

## Expiry / temporary memory

AI-generated paste handoffs should normally include a top-level:

```json
"handoffExpiresAt": "<UTC ISO-8601 timestamp>"
```

Recommended default: **24 hours** unless the user explicitly needs longer.

Temporary walkthroughs can be removed when completed/ended and may be purged after expiry. Temporary secret values held by the engine are cleared when the walkthrough ends.

This is Tampermonkey/browser storage, **not AI long-term memory and not an encrypted password manager**.

## Never use a temporary public Git commit for private data

A timer that deletes a file from a public GitHub branch does **not** make the content private or ephemeral. Git history, forks, caches, clones and notifications can retain it.

Therefore:

- public GitHub is only for material safe to remain public permanently;
- private/project-specific instructions belong in CWZ2/CW2 or a local file;
- credentials belong in neither.

## Private GitHub pointer (`CWG1`)

Format:

```text
CWG1:OWNER/REPO@REF:path/to/walkthrough.json
```

This remains a convenience transport, not the primary private transport. Browser privacy/session/CORS behavior can prevent a userscript from fetching a private GitHub file even while the normal GitHub page is visible. If a `CWG1` import fails, do not ask the user for a PAT as the first fix; generate a `CWZ2` handoff or local file instead.

## Generator

Use the public helper:

```text
make_handoff.py
```

Example:

```bash
python3 make_handoff.py my.walkthrough.json
```

Default output is a 24-hour `CWZ2` code. Use `--format cw2` for the uncompressed fallback or `--ttl-hours 0` only when a non-expiring paste handoff is intentionally required.

The generator performs basic schema validation and refuses several common credential/token patterns. It is a guardrail, not a substitute for privacy review.

## Public/private publication boundary

Before publishing anything here:

- no personal information;
- no private repository names/paths or private infrastructure;
- no internal-only hosts/IPs/endpoints;
- no account-specific billing/entitlement state;
- no private source lists/research terms;
- no credentials, secret values, cookies or recovery material;
- no screenshots that expose private account state.

When in doubt, keep the walkthrough out of the public repository and hand it off locally.
