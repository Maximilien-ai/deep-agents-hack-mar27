# OpenClaw Architecture: How It Works Under the Hood

**Type:** Reference | **Area:** OpenClaw | **Writer:** Writer 1
**Status:** 📋 Outline (pending editor review)
**Target date:** 2026-04-24

---

## Outline

### Introduction (2-3 sentences)
You've installed OpenClaw and sent your first message. Now you want to know what's actually happening behind the scenes. This article breaks down the Gateway architecture — the components, the protocol, the session model, and how everything connects.

### Target Audience
Developers who've completed the "Getting Started" tutorial (or equivalent) and want to understand OpenClaw's internals. They can read code, understand client-server architecture, and want to know how the pieces fit together before building on top of it.

### Scope
**In scope:** Gateway as the central hub, WebSocket protocol, client types (operators vs. nodes), connection lifecycle, session management, channel routing, pairing/trust model, configuration structure, and remote access patterns.

**Out of scope:** Multi-agent routing details (gets its own section but links to full docs), building custom skills/workflows, mobile node internals, deployment guides (Docker, Kubernetes, cloud). These get "learn more" links.

### Key Sections

#### 1. The Big Picture
- OpenClaw is a single long-lived Gateway process that owns all messaging surfaces
- High-level diagram: channels → Gateway → agent, with clients and nodes connecting via WebSocket
- One Gateway per host — it's the single source of truth for sessions, routing, and connections
- Compare to familiar patterns: reverse proxy for AI (messages in, responses out)

#### 2. Components
Walk through each component with its role:

- **Gateway (daemon)** — the core process. Maintains channel connections, exposes the WebSocket API, validates frames, emits events. This is the hub everything connects to.
- **Channels** — WhatsApp (Baileys), Telegram (grammY), Discord, iMessage, Signal, Slack, etc. Each is a provider connection the Gateway maintains. Messages flow in from channels, responses flow back out.
- **Agent (Pi)** — the AI runtime. Receives routed messages, processes them with tools, returns responses. Runs in RPC mode with tool streaming.
- **Clients (operator role)** — macOS app, CLI, web Control UI, automations. Connect via WebSocket, send requests, subscribe to events.
- **Nodes (node role)** — iOS, Android, macOS, headless devices. Connect via WebSocket with explicit capabilities and commands (camera, screen recording, location, Canvas).
- **WebChat / Control UI** — static browser UI that uses the Gateway WebSocket API. No separate server needed.

#### 3. The WebSocket Protocol
- Transport: WebSocket, text frames with JSON payloads
- Handshake: first frame must be `connect` with role, scopes, capabilities, and auth
- Request/response pattern: `{type:"req"} → {type:"res"}`
- Server-push events: `{type:"event"}` for streaming, presence, health, etc.
- Idempotency keys for side-effecting methods (send, agent) — safe retries
- Protocol versioning: `minProtocol`/`maxProtocol` negotiation
- Mermaid sequence diagram: connect → hello-ok → agent request → streaming events → final response

#### 4. Connection Lifecycle
- Device identity on connect (all clients and nodes)
- Challenge-response: Gateway sends nonce, client signs it
- Pairing approval for new devices
- Local trust: loopback/tailnet connections can auto-approve
- Device token issued after first approval — used for subsequent connects
- Signature v3 binds platform + deviceFamily; metadata changes require re-pairing

#### 5. Sessions: How Conversations Work
- Direct chats collapse to a single main session per agent (`agent:<agentId>:main`)
- Group chats get isolated session keys
- `dmScope` controls DM isolation: `main`, `per-peer`, `per-channel-peer`, `per-account-channel-peer`
- Security implications: why `per-channel-peer` matters for multi-user setups
- Session state is owned by the Gateway (clients query, not read local files)
- Identity links: mapping the same person across channels to one session

#### 6. Channel Routing
- How inbound messages get from a channel to the right agent
- Channel-specific handlers (Baileys for WhatsApp, grammY for Telegram, etc.)
- Group chat activation: mention patterns, require-mention settings
- Access control: allowlists, pairing policies (`dmPolicy`), group policies
- Multi-account support: multiple WhatsApp numbers, multiple Telegram bots, all in one Gateway

#### 7. The Agent Runtime
- Pi in RPC mode with tool streaming
- Workspace contract: AGENTS.md, SOUL.md, USER.md, TOOLS.md, BOOTSTRAP.md, IDENTITY.md
- Bootstrap file injection on session start
- Built-in tools (read, write, exec, edit) plus skills
- Tool policy and sandboxing

#### 8. Configuration Architecture
- Config file: `~/.openclaw/openclaw.json` (JSON5)
- Safe defaults if missing
- Four ways to edit: wizard, CLI one-liners, Control UI, direct edit
- Hot reload: Gateway watches the file and applies changes
- Environment variable overrides: `OPENCLAW_HOME`, `OPENCLAW_STATE_DIR`, `OPENCLAW_CONFIG_PATH`
- Key config sections: channels, agents, session, messages, gateway, tools

#### 9. Security Model
- Personal assistant trust model: one trusted operator boundary per gateway
- Gateway auth: tokens protect the WebSocket API
- Device pairing: challenge-response + approval
- Channel access control: allowlists, pairing, group policies
- Not a multi-tenant security boundary — separate gateways for separate trust domains
- `openclaw security audit` for checking your setup

#### 10. Remote Access
- Preferred: Tailscale or VPN
- Alternative: SSH tunnel (`ssh -N -L 18789:...`)
- Same handshake + auth over the tunnel
- TLS + optional certificate pinning for production setups

#### 11. Multi-Agent Routing (Overview)
- Brief overview: multiple isolated agents in one Gateway
- Each agent has its own workspace, state directory, session store, and auth profiles
- Bindings route inbound messages to the right agent
- `openclaw agents add <name>` to create new agents
- Link to full multi-agent routing docs for details

#### 12. What's Next
- Link to multi-agent routing docs
- Link to Gateway protocol reference
- Link to configuration reference
- Tease "Design Patterns for Reliable AI Agents" (Sprint 3)

---

## Sources
- Architecture docs: `/opt/homebrew/lib/node_modules/openclaw/docs/concepts/architecture.md`
- Protocol docs: `/opt/homebrew/lib/node_modules/openclaw/docs/gateway/protocol.md`
- Session docs: `/opt/homebrew/lib/node_modules/openclaw/docs/concepts/session.md`
- Multi-agent docs: `/opt/homebrew/lib/node_modules/openclaw/docs/concepts/multi-agent.md`
- Security docs: `/opt/homebrew/lib/node_modules/openclaw/docs/gateway/security/index.md`
- Agent runtime docs: `/opt/homebrew/lib/node_modules/openclaw/docs/concepts/agent.md`
- Configuration docs: `/opt/homebrew/lib/node_modules/openclaw/docs/gateway/configuration.md`

## Notes for Editor
- This will be the longest article in the series — estimated 4,000-5,000 words
- Heavy on diagrams: architecture overview, sequence diagrams, component map
- Should include the mermaid connection lifecycle diagram from the protocol docs
- Pairs directly with the "Getting Started" tutorial — readers are expected to have a running Gateway
- The multi-agent section is intentionally brief — it's an overview that links to the full docs
- May need to split "Configuration Architecture" into its own article if this runs too long
