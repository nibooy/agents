# PostgreSQL command-line tools agent prompts

Reference knowledge for the **PostgreSQL bundled CLI binaries** — `psql`, `pg_dump`/`pg_restore`/`pg_dumpall`, `pg_basebackup`/`pg_verifybackup`/`pg_combinebackup`, `pg_upgrade`, `pg_rewind`, `pg_resetwal`, `pg_archivecleanup`, `pg_waldump`, `pg_controldata`, `pg_amcheck`, `pgbench`, `pg_test_fsync`, `pg_test_timing`, `initdb`, `pg_ctl`, `pg_createsubscriber`, `pg_walsummary`, `vacuumdb`/`reindexdb`/`clusterdb`, plus all the smaller helpers (`createdb`, `createuser`, `dropdb`, `dropuser`, `pg_isready`, `pg_config`, `pg_receivewal`, `pg_recvlogical`, `postgres`).

Audited against PostgreSQL 18 (current at 2026-05-07) with notes on version-gated tools (incremental backup / `pg_combinebackup` / `pg_walsummary` PG 17+; `pg_createsubscriber` PG 17+; OAuth in `psql` PG 18+). Grounded in the official Part VI references at https://www.postgresql.org/docs/current/reference-client.html and https://www.postgresql.org/docs/current/reference-server.html, with inline `Full docs: <url>` links under every section so the agent can fetch the upstream page when it needs an exhaustive flag list.

This is the **executable-driven** companion to `postgres-admin-specialist`: that agent owns `postgresql.conf`/`pg_hba.conf`/GUCs/replication design; this one owns the CLI binaries you invoke against a cluster.

## Files

| File | Target tool | Format |
|------|-------------|--------|
| `claude.md` | Claude Code | Markdown with YAML frontmatter (`name`, `description`, `model`, `tools`) |
| `codex.md` | OpenAI Codex | Plain markdown (no frontmatter) |
| `copilot.md` | GitHub Copilot | Plain markdown (no frontmatter) |

The three files share the same body — only the frontmatter differs so each tool can parse it. They are **generated** from `knowledge.md` + `claude.frontmatter.yaml` by `scripts/sync.sh`; never edit them directly.

---

## Install

### Claude Code

Drop the file into your agents directory — user-level (available in every session) or project-level (this repo only):

```bash
# User-level (recommended — reusable across all projects)
mkdir -p ~/.claude/agents
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-tools/claude.md \
  -o ~/.claude/agents/postgres-tools-specialist.md

# Project-level
mkdir -p .claude/agents
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-tools/claude.md \
  -o .claude/agents/postgres-tools-specialist.md
```

The frontmatter registers it as a subagent named `postgres-tools-specialist`. Invoke it by asking Claude Code to "use the postgres-tools-specialist agent", or delegate programmatically via the `Agent` tool with `subagent_type: "postgres-tools-specialist"`.

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
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-tools/codex.md \
  -o ~/.codex/AGENTS.md

# Per-project — scoped to the current directory
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-tools/codex.md \
  -o AGENTS.md
```

Codex merges `AGENTS.md` files up the directory tree — either works. Run `codex` in your target directory; the startup banner lists the loaded path next to `Agents.md:` so you can confirm it's picked up.

---

### GitHub Copilot CLI

The standalone terminal tool. For VS Code or other IDEs, drop the same file at `.github/copilot-instructions.md` per repository and enable **GitHub > Copilot > Chat > Code Generation: Use Instruction Files** in settings.

**1. Install and authenticate:**

```bash
npm install -g @github/copilot
copilot       # first launch walks through GitHub sign-in
```

Requires a Copilot subscription. Run `copilot --help` if command names differ — this CLI moves fast.

**2. Install the prompt into a workspace.** Copilot CLI reads `.github/copilot-instructions.md` from the directory it's launched in:

```bash
mkdir -p .github
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-tools/copilot.md \
  -o .github/copilot-instructions.md
```

For an always-on personal setup (active in every directory), check `copilot --help` for the user-level custom-instructions path — last checked it lives under `~/.copilot/`.

---

## Updating

These files track the live PostgreSQL docs. To refresh, re-run the relevant `curl` above — nothing else is needed.

## Provenance and scope

- Built from the public Part VI references — Client Applications (https://www.postgresql.org/docs/current/reference-client.html) and Server Applications (https://www.postgresql.org/docs/current/reference-server.html) — plus the per-tool reference pages linked under each `Full docs:` line in `knowledge.md`.
- Covers PostgreSQL 18 with audit notes for PG 17/16; version-gated tools and flags are called out inline.
- Intended as a fast-retrieval reference for an LLM, not a substitute for the upstream docs. When in doubt, link the user to the canonical page.
- Claims not directly stated in the docs are explicitly hedged in-text.
- Companion to `postgres-admin-specialist` (server admin / config / replication design) and `postgres-sql-specialist` (SQL / query design). This agent owns the **executables**.
