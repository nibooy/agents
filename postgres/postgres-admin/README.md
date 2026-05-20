# PostgreSQL admin agent prompts

Reference knowledge for [PostgreSQL](https://www.postgresql.org/) **server administration** — installation, `initdb` and data directory layout, `postgresql.conf` / `pg_hba.conf`, roles and authentication (SCRAM, GSSAPI, LDAP, cert, peer), databases and tablespaces, localization, VACUUM / autovacuum, WAL and checkpoints, physical and logical replication, PITR with `pg_basebackup`, monitoring via `pg_stat_*` views, JIT, regression tests.

Covers PostgreSQL 18 (the current major as of 2026-05-07; behaviour generally also applies to 17 and 16). Grounded in the official docs at https://www.postgresql.org/docs/current/admin.html, with inline `Full docs: <url>` links under every section so the agent can WebFetch upstream when it needs more detail.

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
curl -fsSL https://raw.githubusercontent.com/yoobinshin/agents/main/postgres/postgres-admin/claude.md \
  -o ~/.claude/agents/postgres-admin-specialist.md

# Project-level
mkdir -p .claude/agents
curl -fsSL https://raw.githubusercontent.com/yoobinshin/agents/main/postgres/postgres-admin/claude.md \
  -o .claude/agents/postgres-admin-specialist.md
```

The frontmatter registers it as a subagent named `postgres-admin-specialist`. Invoke it by asking Claude Code to "use the postgres-admin-specialist agent", or delegate programmatically via the `Agent` tool with `subagent_type: "postgres-admin-specialist"`.

The preamble directs the agent to WebFetch the linked upstream page on questions the summary doesn't fully cover, so it stays accurate as PostgreSQL evolves.

---

### OpenAI Codex

**1. Install Codex CLI** and authenticate (`codex login` or set `OPENAI_API_KEY`):

```bash
brew install codex            # or: npm install -g @openai/codex
codex --version
```

**2. Drop the prompt into Codex's instruction path.** Two options — pick based on scope:

```bash
# Global — active in every Codex session
mkdir -p ~/.codex
curl -fsSL https://raw.githubusercontent.com/yoobinshin/agents/main/postgres/postgres-admin/codex.md \
  -o ~/.codex/AGENTS.md

# Per-project — scoped to the current directory
curl -fsSL https://raw.githubusercontent.com/yoobinshin/agents/main/postgres/postgres-admin/codex.md \
  -o AGENTS.md
```

Codex merges `AGENTS.md` files up the directory tree — either works. Run `codex` in your target directory; the startup banner lists the loaded path next to `Agents.md:` so you can confirm it's picked up.

---

### GitHub Copilot CLI

The standalone terminal tool. For VS Code or other IDEs, drop the same file at `.github/copilot-instructions.md` per repository and enable **GitHub › Copilot › Chat › Code Generation: Use Instruction Files** in settings.

**1. Install and authenticate:**

```bash
npm install -g @github/copilot
copilot       # first launch walks through GitHub sign-in
```

Requires a Copilot subscription. Run `copilot --help` if command names differ — this CLI moves fast.

**2. Install the prompt into a workspace.** Copilot CLI reads `.github/copilot-instructions.md` from the directory it's launched in:

```bash
mkdir -p .github
curl -fsSL https://raw.githubusercontent.com/yoobinshin/agents/main/postgres/postgres-admin/copilot.md \
  -o .github/copilot-instructions.md
```

For an always-on personal setup (active in every directory), check `copilot --help` for the user-level custom-instructions path — last checked it lives under `~/.copilot/`.

---

## Updating

These files track the live PostgreSQL docs at https://www.postgresql.org/docs/current/. To refresh, re-run the relevant `curl` above — nothing else is needed.

## Provenance and scope

- Built from the public Part III "Server Administration" docs at https://www.postgresql.org/docs/current/admin.html, plus the canonical chapter pages:
  - Installation: https://www.postgresql.org/docs/current/install-binaries.html, https://www.postgresql.org/docs/current/installation.html
  - Server setup: https://www.postgresql.org/docs/current/runtime.html
  - Server configuration: https://www.postgresql.org/docs/current/runtime-config.html
  - Client authentication: https://www.postgresql.org/docs/current/client-authentication.html
  - Database roles: https://www.postgresql.org/docs/current/user-manag.html
  - Managing databases: https://www.postgresql.org/docs/current/managing-databases.html
  - Localization: https://www.postgresql.org/docs/current/charset.html
  - Routine maintenance: https://www.postgresql.org/docs/current/maintenance.html
  - Backup and restore: https://www.postgresql.org/docs/current/backup.html
  - High availability: https://www.postgresql.org/docs/current/high-availability.html
  - Monitoring: https://www.postgresql.org/docs/current/monitoring.html
  - Reliability and WAL: https://www.postgresql.org/docs/current/wal.html
  - Logical replication: https://www.postgresql.org/docs/current/logical-replication.html
  - JIT: https://www.postgresql.org/docs/current/jit.html
  - Regression tests: https://www.postgresql.org/docs/current/regress.html
- Audited against PostgreSQL 18 (current release as of 2026-05-07). Most material applies unchanged to PG 17 and 16 — version-gated changes are noted inline.
- Intended as a fast-retrieval reference for an LLM, not a substitute for the upstream docs. When in doubt, link the user to the canonical page.
- Claims not directly stated in the docs are explicitly hedged in-text.
