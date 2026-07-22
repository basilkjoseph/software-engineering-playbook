# Cookies & Cache: A Quick Guide

## What is a Cookie?

A cookie is a small piece of data that a website sends to your browser, which your browser stores and sends back on every future request to that same site. It's essentially a sticky note the server hands you that says "remember this," and you hand it back each time you interact with that server again.

### Why Cookies Exist
HTTP is stateless — every request is treated as brand new, with no memory of previous ones. Cookies solve this by giving servers a way to recognize you across multiple requests or visits.

### Common Uses
- **Session management** — staying logged in, keeping items in a shopping cart, multi-step checkout flows
- **Authentication** — storing a token that proves you've already verified your identity
- **Preferences** — remembering language, theme, currency, etc.
- **Tracking/analytics** — monitoring behavior across visits or across different sites

### How It Works
1. Server sends a `Set-Cookie` header with a key-value pair (plus metadata like expiration and security flags)
2. Browser stores it
3. Browser automatically attaches the cookie to future requests via a `Cookie` header
4. Server reads it to identify you or restore session state

---

## Cookies vs. Cache

Both live in the browser, but they serve completely different purposes.

| | Cookies | Cache |
|---|---|---|
| **Purpose** | Identity & state (who are you?) | Performance (avoid re-downloading files) |
| **What's stored** | Small key-value data (session IDs, tokens, preferences) | Actual files/responses (images, CSS, JS, API responses) |
| **Visibility** | Sent to the server on every matching request | Stays on your machine; never sent to the server |
| **Size** | Small (a few KB) | Can be large (MBs) |
| **Expiration logic** | Set by server rules (time/session-based) | Based on freshness headers (`Cache-Control`, `ETag`) |

**Analogy:** A cookie is like showing your ID badge every time you enter a building so security remembers you. Cache is like keeping a photocopy of a document in your desk drawer so you don't have to walk to the records room and request it again.

---

## How Cookies Relate to Privacy

The privacy concern mainly comes from **third-party cookies**, not cookies in general.

- **First-party cookies** — set by the site you're actually visiting (e.g., a shopping cart on an e-commerce site). Mostly benign.
- **Third-party cookies** — set by a different domain embedded in the page you're visiting (e.g., an ad network's script). If that same network is embedded across thousands of unrelated sites, it can recognize you on each one and build a profile of your browsing habits across the entire web.

### How Tracking Works
If an ad network's script sits on a news site, a shopping site, and a recipe blog, your browser sends it the same cookie ID each time. The network can stitch together your activity across all three — even though you never directly interacted with the ad company. This is why browsing for shoes once can lead to shoe ads following you for weeks.

### Why It's Regulated
Cross-site profiling without clear consent led to laws like GDPR (EU) and CCPA (California), requiring sites to disclose cookie use and obtain consent for non-essential tracking — hence the "Accept Cookies" banners.

### Where Things Are Headed
Browsers have been cracking down: Safari and Firefox block third-party cookies by default, and Chrome has been moving (gradually) toward similar restrictions. This has pushed companies toward alternatives like fingerprinting and first-party data strategies, which raise their own privacy debates.
