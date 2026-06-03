# Gateway API agent prompts

Reference knowledge for the [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/) — the standard for L4/L7 service networking in Kubernetes, covering ingress, load balancing, traffic management, and service mesh (GAMMA).

Covers GatewayClass, Gateway, HTTPRoute, GRPCRoute, TLSRoute, TCPRoute/UDPRoute, ReferenceGrant, route attachment, traffic splitting, TLS termination, cross-namespace references, release channels, conformance, and troubleshooting. Grounded in the official docs at https://gateway-api.sigs.k8s.io/docs/, with inline `Full docs: <url>` links under every section so the agent can fetch upstream when it needs more detail.

## Files

| File | Target tool | Format |
|------|-------------|--------|
| `claude.md` | Claude Code | Markdown with YAML frontmatter (`name`, `description`, `model`, `tools`) |
| `codex.md` | OpenAI Codex | Plain markdown (no frontmatter) |
| `copilot.md` | GitHub Copilot | Plain markdown (no frontmatter) |

The three files share the same body — only the frontmatter differs so each tool can parse it.

---

## Install

### Claude Code

Drop the file into your agents directory — user-level (available in every session) or project-level (this repo only):

```bash
# User-level (recommended — reusable across all projects)
mkdir -p ~/.claude/agents
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/gateway-api/claude.md \
  -o ~/.claude/agents/gateway-api-specialist.md

# Project-level
mkdir -p .claude/agents
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/gateway-api/claude.md \
  -o .claude/agents/gateway-api-specialist.md
```

The frontmatter registers it as a subagent named `gateway-api-specialist`. Invoke it by asking Claude Code to "use the gateway-api-specialist agent", or delegate programmatically via the `Agent` tool with `subagent_type: "gateway-api-specialist"`.

---

### OpenAI Codex

```bash
# Global
mkdir -p ~/.codex
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/gateway-api/codex.md \
  -o ~/.codex/AGENTS.md

# Per-project
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/gateway-api/codex.md \
  -o AGENTS.md
```

---

### GitHub Copilot CLI

```bash
mkdir -p .github
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/gateway-api/copilot.md \
  -o .github/copilot-instructions.md
```

---

## Updating

These files track the live Gateway API docs. To refresh, re-run the relevant `curl` above.

## Provenance and scope

- Built from the public docs at https://gateway-api.sigs.k8s.io/docs/ and the repo at https://github.com/kubernetes-sigs/gateway-api.
- Intended as a fast-retrieval reference for an LLM, not a substitute for the upstream docs. When in doubt, link the user to the canonical page.
- Claims not directly stated in the docs are explicitly hedged in-text.
