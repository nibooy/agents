# AKS Desktop agent prompts

Reference knowledge for [AKS Desktop](https://github.com/Azure/aks-desktop) — Azure's application-focused desktop app for deploying and managing workloads on AKS, built on a Headlamp fork. Covers repo layout, hard cluster requirements (Entra ID, Azure RBAC, Cilium), local dev (`install:all` / `plugin:setup` / `dev`), the `runAzCommand` pattern for Azure CLI calls, plugin registration with project-tab gating, and the `aksd:` / `upstreamable:` commit conventions.

Grounded in the repo README, `AGENTS.md`, and `docs/cluster-requirements.md`, with inline `Full docs: <url>` links under every section.

## Files

| File | Target tool | Format |
|------|-------------|--------|
| `claude.md` | Claude Code | Markdown with YAML frontmatter (`name`, `description`, `model`, `tools`) |
| `codex.md` | OpenAI Codex | Plain markdown (no frontmatter) |
| `copilot.md` | GitHub Copilot | Plain markdown (no frontmatter) |

The three files share the same body — only the frontmatter differs.
