# Headlamp agent prompts

Reference knowledge for [Headlamp](https://headlamp.dev/) — the extensible Kubernetes web UI from kubernetes-sigs. Covers architecture, installation (desktop/in-cluster/Docker), the plugin SDK (`@kinvolk/headlamp-plugin`), UI extension APIs, OIDC/auth setup, multi-cluster support, and troubleshooting.

Grounded in the official docs at https://headlamp.dev/docs/latest/, with inline `Full docs: <url>` links under every section so the agent can fetch upstream when it needs more detail.

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

```bash
# User-level (recommended — reusable across all projects)
mkdir -p ~/.claude/agents
curl -fsSL https://raw.githubusercontent.com/ytimocin/agents/main/headlamp/claude.md \
  -o ~/.claude/agents/headlamp-specialist.md

# Project-level
mkdir -p .claude/agents
curl -fsSL https://raw.githubusercontent.com/ytimocin/agents/main/headlamp/claude.md \
  -o .claude/agents/headlamp-specialist.md
```

### OpenAI Codex

Copy `codex.md` into `AGENTS.md` or append it to an existing one.

### GitHub Copilot

Copy `copilot.md` into `.github/copilot-instructions.md` or append it.
