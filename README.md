# LinkClipper Data-Analytics

What we measure, what we never collect, and how that choice is enforced. This is a *policy* repo for Phase A/B and an *implementation* repo only when hosted features exist (Phase C+).

## The zero-telemetry law

- **The extension sends no usage data, ever** — no install beacons, no crash reporters, no error telemetry, no favicon checks. Not even "anonymous" counters. This is a product feature and shows up in the CWS listing and privacy page.
- Enforcement is structural: there is no network transport in the extension except (a) thumbnails/CDN data the user saves links point to, (b) the Reader fetch the user triggered, (c) sync to the user's own configured project. CI can keep it that way — see below.

## What we use instead (all owner-visible, all passive)

| Metric | Source | Used for |
|---|---|---|
| Store visibility stats (impressions, installs) | CWS dashboard | listing conversion |
| Uninstall signal | CWS account refund/failure logs + store reviews | regression alerts |
| GitHub issues/stars | product repo | feature demand ranking |
| Release adoption | GitHub release download counts | packaging questions only |

No unique-per-user anything, ever, at any phase.

## Phase C metrics design (pre-commitments)

When hosted discovery exists:

1. **Opt-in analytics only** — the extension never phones home; the *site* and hosted surfaces may count page events, obviously (they're public).
2. Events measured on the server, not the client: shares created, remixes, follows, moderations handled.
3. Establish before launch: retention policy, deletion request path (right-to-forget aware), no third-party mirror of event data (self-hosted plausible Piwik/Umami only).
4. Success metrics declared in advance (public board saves/minute of real usage; time-in-reader). No engagement-pumping levers exist by design — nothing to optimize darkly.

## CI guard (planned, small)

A `check` step greps the store zip for transport calls to unknown hosts: allow-listed hosts = manifest-declared. Fails on any other `fetch(`…`)/XHR` domain constant.
