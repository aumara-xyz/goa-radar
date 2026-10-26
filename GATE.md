# Client proposal password gate (26 Oct 2026)

## What we did
Pure static GitHub Pages cannot set HttpOnly cookies or enforce server auth. For Duane Healey’s blocking request we shipped:

1. **Client-side password gate** on every client proposal page (`site/p/*/index.html` and `site/p/tXX.html`).
2. **Password:** `partner` — same as everyspot.media/goa.
3. **Payload wrapping:** the full named proposal HTML (including Disney / QPAC titles) is stored as **base64** and only decoded into the DOM after a correct password. Ungated View Source shows a generic “Client proposal (gated)” shell, not “Prepared for Disney Theatrical / QPAC”.
4. **Session:** `sessionStorage` key `goa-radar-gate-v1` so reloads in the same tab stay unlocked.
5. **Rep briefs** (`*-brief.html` / `*/brief/`) stay ungated for the goa sales demo walkthrough; they already send `noindex,nofollow`.
6. **Radar index / hot list** still name public events (including Lion King) because that feed is the sales tool demo for goa, not a client-facing proposal.

## Limits (honest)
This is **not** server-side auth. A determined person can decode the base64 payload. It is stronger than open HTML naming Disney on a public URL, and matches what static GH Pages allows without Cloudflare Access / Pages Functions.

## Production recommendation
Put `/p/*` client proposals behind Cloudflare Access (or move the demo off public GH Pages) with the same shared partner password or goa SSO.
