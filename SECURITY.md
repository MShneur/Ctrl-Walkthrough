# Security and Public-Safety Policy

Control Walkthrough is public. The safest assumption is that anything committed, pasted into an issue, attached to a PR, or shown in a screenshot can be copied and retained indefinitely.

## Never publish secrets

Do **not** put any real secret or private account value in this repository, including:

- passwords or passphrases;
- API keys, access tokens, bearer tokens, PATs, OAuth client secrets;
- session cookies, SSH/private keys, recovery codes;
- private hostnames/IP addresses or internal-only endpoints;
- account IDs, billing information, personal information, or private project data;
- screenshots that expose private account state;
- secret values hidden in JSON, Base64, CWZ2/CW2, comments, test fixtures, example files, commit messages, issues, or PRs.

**Encoding is not encryption.** A CWZ2/CW2 handoff containing a secret is still a leaked secret.

**Deletion is not a privacy strategy.** Public Git history, forks, caches, notifications, and clones may retain content after you delete or rewrite it.

## How walkthroughs should handle credentials

A public walkthrough may contain a **secret name** such as `API_TOKEN` or instructions describing where the user should create/store it. It must never contain the real value.

Preferred storage order:

1. provider/server secret store for long-lived credentials;
2. temporary Tampermonkey-local walkthrough memory for short setup handoffs;
3. never GitHub, chat, a URL, a screenshot, or a public handoff code.

Temporary local memory is convenience storage, not an encrypted password manager. It is browser/profile-local and is cleared when the walkthrough ends.

## Automation safety

Control Walkthrough may automate reversible navigation and safe field filling when the exact target can be verified.

It should stop for user confirmation at consequential boundaries such as:

- login/MFA/CAPTCHA;
- OAuth/permission/legal consent;
- billing, purchase, subscription, or plan changes;
- account/security settings;
- final deployment, publication, deletion, or irreversible resource changes;
- creating, rotating, regenerating, revoking, or exposing credentials.

If the UI is ambiguous, the engine should fail closed rather than guess.

## Reporting a security issue

Do not demonstrate a vulnerability by posting a real credential or private account data in a public issue.

Open a public issue only when the report can be fully sanitized. If it cannot, use GitHub's private vulnerability reporting feature if/when it is enabled for this repository, or contact the repository owner through a private channel you already trust.
