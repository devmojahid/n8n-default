# CLAUDE.md — n8n Automation Build System

> Drop this file at the **root of your n8n project** (the `n8n/` folder in your screenshots).
> Claude Code reads `CLAUDE.md` automatically on every session, so these rules and lessons load
> every time — you do **not** need to paste a big "role prompt" again.

---

## 1. Your job when I ask for an automation

You are a senior n8n automation engineer. When I describe a client need (in plain English or
Bengali), you:

1. **Understand the goal** — restate it in 1–2 lines so we agree before building.
2. **Search before guessing** — use the n8n-MCP tools + the API skills in `.agents/skills/` +
   `docs/latest-api-references/`. Never invent node names, parameters, or API fields.
3. **Build it correctly** using the protocol in section 2.
4. **Validate it** before showing me (section 2).
5. **Deploy to a COPY**, never to a live/active workflow.
6. **Tell me exactly what I must do by hand** (add credentials, switch webhook URL, etc.).

Do not re-output the whole workflow over and over when fixing one thing. Fix the specific node,
explain the one change, and move on.

---

## 2. n8n-MCP build protocol (MANDATORY ORDER)

This order is what makes workflows work on the first or second try instead of failing repeatedly.

1. **Templates first.** Call `search_templates` before building from scratch. If a good template
   exists, start from it.
2. **Get real node schemas.** Use `search_nodes` / `get_node` for every node you add. Use the
   actual property names from the schema, not memory.
3. **Never trust defaults.** Default parameter values are the #1 cause of runtime failures.
   Explicitly set every parameter that controls behavior.
4. **Validate in levels:**
   - `validate_node` (minimal) → fix → `validate_node` (full) → fix →
   - `validate_workflow` on the whole thing → fix until clean.
5. **Deploy to a copy.** Create as a new/inactive workflow. NEVER edit my production workflow
   directly — AI edits can be unpredictable. I will copy to production myself once it's tested.
6. **Test path, then report.** Tell me the test steps and what a correct output looks like.

If `validate_workflow` is not available when I run `/mcp`, STOP and tell me — it means n8n-MCP is
only giving node docs, not connected to my instance (see section 6).

---

## 3. The build loop for a client request

```
Client need (plain language)
        │
        ▼
Restate goal  ──►  search_templates  ──►  get_node schemas  ──►  read API skill / docs
        │                                                              │
        ▼                                                              ▼
Build workflow (env vars for secrets, correct expressions, no risky Code nodes)
        │
        ▼
validate_node (minimal → full)  ──►  validate_workflow  ──►  fix until clean
        │
        ▼
Deploy as INACTIVE copy  ──►  give me: credentials to add + webhook URL + test steps
```

---

## 4. PITFALL LIBRARY — never make these mistakes again

These are the exact bugs that wasted hours before. Apply the fix automatically; do not re-discover them.

### 4.1 `Module 'crypto' is disallowed` in a Code node
- **Cause:** n8n's task-runner sandbox blocks `require('crypto')`.
- **Default fix (preferred): do NOT use a Code node for hashing.** Use the built-in **Crypto node**:
  - Action: **HMAC**, Algorithm: **SHA256**, Encoding: **Hex**.
  - Value = the raw request body; Secret = `{{ $env.FB_APP_SECRET }}`.
  - Then an **IF** node compares `=sha256={{ $json.hmacHex }}` to the
    `x-hub-signature-256` header.
- **Alternative fix (if Code node is required):** set `NODE_FUNCTION_ALLOW_BUILTIN=crypto` on the
  task runner. In a single-container Docker n8n (internal runner) set it on the `n8n` service. With
  an **external** runner sidecar it must be set on the **runner** container (or in
  `n8n-task-runners.json` under `env-overrides`) — setting it only on the main container will NOT
  work.
- **Rule:** prefer native nodes (Crypto, HTTP Request, Set, IF, Switch) over Code nodes. Code nodes
  are the most fragile part of n8n.

### 4.2 `Node '⚙️ CONFIG - Edit Me' hasn't been executed`
- **Cause:** referencing `$node['CONFIG'].json` only works if that node ran in the current path.
  After a Switch/IF, the branch that didn't run can't see it → this error.
- **Fix:** stop using a Set node as config. Put values in **n8n environment variables** and read them:
  - `{{ $env.FB_PAGE_TOKEN }}`, `{{ $env.FB_APP_SECRET }}`, `{{ $env.FB_PAGE_ID }}`,
    `{{ $env.GRAPH_API_VERSION }}`
- **Benefits:** never throws "hasn't executed", works in every branch, secrets stay out of the
  workflow JSON (safe to export/share).
- Requires `N8N_BLOCK_ENV_ACCESS_IN_NODE=false` (this is the default unless explicitly blocked).

### 4.3 Merge node "combineAll" stalls the workflow
- **Cause:** a Switch fires only ONE branch, but the Merge node (mode: combine, combineAll, 4 inputs)
  waits for **all** inputs. The empty branches never arrive → workflow looks "successful" but stops.
- **Fix:** after a Switch, connect each branch **directly** to the next node (e.g. all branches →
  `Load Memory`). n8n allows several inputs into one node; only the active branch carries data. Do
  not put a "combineAll" Merge after a Switch.

### 4.4 `continueOnFail` hides real errors
- **Cause:** with Continue On Fail = ON, a node (like `Send Reply` calling Graph API) can be rejected
  by Facebook while n8n still shows "executed successfully."
- **Fix:** during development, **turn Continue On Fail OFF** so real errors surface. Turn it on only
  in production, and only with a proper error branch.

### 4.5 Memory node session key with non-Chat triggers
- **Cause:** Simple Memory expects `sessionId` from the Chat Trigger. Telegram/Facebook triggers
  don't provide it.
- **Fix:** set the session key manually:
  - Telegram: `={{ $('Telegram Trigger').item.json.message.chat.id }}`
  - Messenger: `={{ $json.psid }}` (the sender PSID)
- **Production note:** Simple Memory is per-instance and breaks in queue/multi-worker mode. For
  production use **Redis Chat Memory** or **Postgres Chat Memory**.

### 4.6 "Workflow runs but nothing happens" — test vs production webhook
- **Cause:** `/webhook-test/...` only listens while you click **Listen for test event** /
  **Execute workflow**. It stops the moment execution ends.
- **Fix:** the production URL `/webhook/...` only works after the workflow is **Active**. Flow:
  1. Test with the `-test` URL while building.
  2. When it works, **Activate** the workflow.
  3. In Meta's callback settings, switch the URL from `/webhook-test/...` to `/webhook/...`.

### 4.7 Send API field mistakes (Messenger/WhatsApp)
- **Never** put reply text in the Chat ID / recipient field. That causes "chat not found" /
  "invalid recipient ID."
  - Recipient/Chat ID = the user's ID (PSID or chat id), as a **string**.
  - Message text = the AI output.
- **HTTP Request JSON body:** send a real JSON **object**, not `JSON.stringify(...)`. Example body:
  ```
  ={{ { recipient: { id: String($json.psid || '') }, messaging_type: 'RESPONSE',
        message: { text: String($json.replyText || 'Hi! How can I help you?') } } }}
  ```
- Messenger Send API endpoint: `https://graph.facebook.com/{{ $env.GRAPH_API_VERSION }}/{{ $env.FB_PAGE_ID }}/messages`
  with `?access_token={{ $env.FB_PAGE_TOKEN }}`.

---

## 5. Token / permission reality (Meta-specific)

When a Graph API call fails, it is almost always one of these — check in this order:
- **Error 190** → expired/invalid token. Regenerate the Page Access Token and update `$env.FB_PAGE_TOKEN`.
- **Permission / "user not allowed"** → app is in Development mode. Only Admin/Developer/Tester
  Facebook accounts work before App Review. Add the test account as a Tester and accept the invite.
- **Error 100 "invalid recipient"** → the upstream node didn't produce a valid PSID. Check the node
  before Send Reply.
- **Error 80001 / rate limit** → back off and retry.

---

## 6. Environment requirements (one-time n8n setup)

Set these in your `docker-compose` for the `n8n` service so the sandbox and env vars behave:

```yaml
services:
  n8n:
    image: docker.n8n.io/n8nio/n8n   # pin a version, 2.18.4+ recommended for MCP build features
    environment:
      - NODE_FUNCTION_ALLOW_BUILTIN=crypto
      - NODE_FUNCTION_ALLOW_EXTERNAL=axios,luxon,uuid
      - N8N_BLOCK_ENV_ACCESS_IN_NODE=false   # allow {{ $env.* }} in expressions
      # Meta secrets — read in workflows as {{ $env.FB_PAGE_TOKEN }} etc.
      - FB_APP_SECRET=__set_me__
      - FB_PAGE_TOKEN=__set_me__
      - FB_PAGE_ID=__set_me__
      - GRAPH_API_VERSION=v25.0
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
```

> If you run task runners in **external** mode (a separate `n8nio/runners` sidecar), set the
> `NODE_FUNCTION_ALLOW_*` vars on the **runner** container too (or in `n8n-task-runners.json`).

**Connect n8n-MCP to your instance (so it can validate + deploy):**
1. n8n → **Settings → n8n API → Create API Key**.
2. Give n8n-MCP your `N8N_API_URL` (`http://localhost:5678`) + the API key (see n8n-mcp README for
   exact env var names).
3. In Claude Code, run `/mcp` and confirm you now see `n8n_create_workflow` and
   `n8n_validate_workflow`, not just `search_nodes`.

---

## 7. Repo map

```
n8n/
├── CLAUDE.md                      # this file — auto-loaded by Claude Code
├── .mcp.json                      # n8n-docs MCP + n8n-mcp MCP
├── skills-lock.json
├── .agents/skills/
│   └── whatsapp-cloud-api/        # bellopushon skill (official Meta WhatsApp docs)
│       ├── SKILL.md
│       └── references/
├── docs/
│   └── latest-api-references/     # backup reference only — prefer skills + Postman
│       ├── facebook/
│       ├── messenger/             # (rename from "messneger")
│       └── whatsapp/
└── workflows/                     # exported workflow JSON, version-controlled
```

---

## 8. API reference priority (where to look first)

1. **n8n-MCP** → for any n8n node config (the schema is the source of truth).
2. **`.agents/skills/whatsapp-cloud-api/`** → WhatsApp Cloud API specifics.
3. **Meta's official Postman collections** (postman.com/meta) → structured, maintained request/payload
   examples for WhatsApp, Messenger, Instagram, Graph API. Export these into `docs/` as your primary
   API reference; add Messenger/Instagram/Graph skills the same way the WhatsApp skill works.
4. **`docs/latest-api-references/`** → last resort / offline backup.

Always prefer the **latest** Graph API version (currently `v25.0`) — do not copy old version numbers
from blog posts.

---

## 9. Adding more API skills (instead of building a custom MCP)

You do NOT need a custom MCP for API knowledge. Add references as skills:

```bash
# example pattern (review any skill before use — they run with full agent permissions)
npx skills add https://github.com/<author>/<repo> --skill <skill-name>
```

Build a custom MCP **only** if you later want Claude to *execute* live API calls (with your tokens)
outside of n8n — and even then, check for an existing Meta MCP server first.
