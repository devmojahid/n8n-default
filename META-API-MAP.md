# META-API-MAP.md — The Meta API landscape + how to stay current

> Reference doc. Goal: know what every Meta API is, and keep the system on the **latest version**
> without freezing a stale scrape. Pairs with `CLAUDE.md` (build rules).
>
> **Last verified: June 2026 — Graph API & Marketing API are at v25.0 (released Feb 18, 2026).**
> Re-verify against Meta's changelog every quarter.

---

## 1. The one thing to remember

Everything is built on the **Graph API** (`https://graph.facebook.com/vXX.0/...`, OAuth tokens,
objects + edges). Every other "API" below is either a set of Graph API endpoints or a product
layered on top. Learn the **5 shared mechanics** (section 4) and every Meta API behaves the same.

**No AI knows the latest version on its own.** The version is a *variable you control*
(`{{ $env.GRAPH_API_VERSION }}`), and the live source of truth is Meta's **Changelog**, not the
model's memory or a one-time scrape.

---

## 2. The full API map (grouped by use case)

### Foundation
| API | What it does |
|---|---|
| **Graph API** | Core HTTP interface to the whole social graph. Read/write Pages, posts, comments, media, etc. Everything else rides on it. |

### Messaging (CRM / chatbot / support)
| API | What it does |
|---|---|
| **WhatsApp Business Platform (Cloud API)** | Send/receive WhatsApp messages, templates, media, interactive buttons. Conversation-based pricing. |
| **WhatsApp Flows** | Structured in-chat forms/flows on WhatsApp. |
| **Messenger Platform** | Facebook Page DMs — Send API, webhooks, handover protocol, persistent menu. |
| **Instagram Messaging** | Instagram DMs (runs through the Messenger platform). 24-hour reply window rule. |

### Content & social
| API | What it does |
|---|---|
| **Instagram Graph API** | Publish posts/reels/stories, read insights, manage comments. **Business/Creator accounts only.** |
| **Threads API** | Publish/manage owned Threads content. Newer, narrower endpoints, no public search. |
| **Pages / Stories / Live Video / Sharing / Social Plugins** | Page content, story publishing, live streams, share dialogs, embeds. |
| **Creator Discovery API** | Discover and analyze creator content. |

### Ads & commerce (Marketing family)
| API | What it does |
|---|---|
| **Marketing API** | Campaigns, ad sets, ads, audiences, insights. A collection of Graph API endpoints for ads. |
| **Conversions API** (+ **Gateway**, **Signals Gateway**) | Server-side event tracking for ad optimization & measurement. |
| **Catalog API / Commerce Platform** | Product catalogs, shops, Marketplace selling. |
| **App Ads / Automotive Ads / Audience Network** | Specialized ad formats & monetization. |

### Business & auth (shared plumbing)
| API | What it does |
|---|---|
| **Business Management API** | Manage business assets (pages, accounts, users) across FB/IG/Messenger/WhatsApp. |
| **Meta Business SDK** | Official SDK wrapping Graph + Marketing APIs. |
| **Facebook Login** | OAuth 2.0 — how you get user/Page access tokens. |
| **Webhooks** | Event delivery (incoming messages, status updates). Used by every product. |
| **Wit.ai / Data Portability** | NLP; user data export. |

---

## 3. Versioning reality (why you can't freeze a scrape)

- Meta releases a **new API version about every quarter**, and supports each version for
  **~2 years** before sunset.
- **Current: v25.0** (Graph + Marketing, released Feb 18, 2026). **v26.0 ~September 2026.**
- **Deprecation schedule (examples — verify in changelog):**
  - v19 → deprecated May 21, 2026
  - v20 → deprecated September 24, 2026
  - Legacy reach metric → replaced by **Page Viewer Metric / Media Views by June 2026**
  - **Webhooks mTLS cert** → must trust new Meta CA by **March 31, 2026** (or webhook delivery stops)
  - Instagram **Basic Display API** → already end-of-life (Dec 4, 2024); Business/Creator only now
  - Personal-profile publishing via API → gone, not returning (Pages only)

**Source of truth (check before any production release):**
`https://developers.facebook.com/docs/graph-api/changelog`

---

## 4. The 5 mechanics every Meta API shares

Master these once — they cause far more breakage than "which endpoint."

1. **OAuth & tokens** — short-lived → long-lived (e.g. 60 days for many). Tokens expire → **error 190**.
   Implement refresh; store tokens in env/secrets, never in workflow JSON or git.
2. **Permissions & App Review** — most scopes need Meta approval. In **Development mode**, only
   Admin/Developer/Tester accounts work. Public users need App Review + Live mode.
3. **Webhooks** — verify the challenge on setup, **ack within seconds**, dedupe by event ID (events
   can arrive more than once), treat payloads as untrusted.
4. **Rate limits** — Business Use Case (BUC) tiers scale with app history; new apps are throttled
   hard. Read `X-App-Usage` / `X-Business-Use-Case-Usage` headers. **Error 80001** = too many calls.
5. **Versioning** — the `vXX.0` in the URL. Keep it in one variable; bump it deliberately.

---

## 5. How to keep the system on the latest version (the actual maintenance plan)

### A. Version = one variable
- `GRAPH_API_VERSION=v25.0` in env. Every node/URL uses `{{ $env.GRAPH_API_VERSION }}`.
- To upgrade: change one line, re-test, done. The AI does not need to "know" the number.

### B. Maintained references (not frozen scrapes)
- **WhatsApp skill** (`.agents/skills/whatsapp-cloud-api/`) — community-maintained from official docs.
- **Meta's official Postman collections** (`postman.com/meta`) — Meta maintains these; closest thing
  to a live structured spec. Export WhatsApp / Messenger / Instagram / Graph and keep in `docs/`.
- Add Messenger / Instagram / Graph **skills** the same way you added WhatsApp (don't build a custom
  docs MCP — it just duplicates these and rots).

### C. Web-search-at-build-time
- When Claude Code is unsure about a field, version, or deprecation → **check the changelog, don't
  guess.** This is what keeps you correct *between* refreshes.

### D. Quarterly refresh (≈15 min, 4×/year — aligned to Meta's release cadence)
1. Bump `GRAPH_API_VERSION` to the new version.
2. Re-pull Postman collections / update skills.
3. Skim the changelog for deprecations that hit your workflows (metrics, certs, removed fields).
4. Re-run `validate_workflow` on critical workflows after the bump.

### E. Same pattern for non-Meta APIs ("and others")
- **n8n** → n8n-MCP already tracks node versions for you.
- **Stripe / others** → version-as-variable + maintained reference (skill or official OpenAPI/Postman)
  + changelog-check. Never freeze "the latest"; just make updating cheap.

---

## 6. Quick decision guide (which API for a client request)

- "Reply to WhatsApp messages / send templates" → **WhatsApp Cloud API**
- "Auto-reply Facebook Page DMs" → **Messenger Platform**
- "Auto-reply / manage Instagram DMs" → **Instagram Messaging**
- "Publish to Instagram, read insights, moderate comments" → **Instagram Graph API**
- "Post text to Threads" → **Threads API**
- "Create/manage ad campaigns, pull ad insights" → **Marketing API**
- "Server-side conversion tracking" → **Conversions API**
- "Product catalog / shop" → **Catalog / Commerce**
- "Manage business assets/users across all of the above" → **Business Management API**

If unsure: it's almost always Graph API underneath — confirm the exact endpoint + version in the
changelog before building.
