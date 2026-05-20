# PostgreSQL extensions agent prompts

Reference knowledge for **PostgreSQL bundled extensions** — Appendix F "Additional Supplied Modules and Extensions" of the official manual. Covers `pg_stat_statements`, `auto_explain`, `pg_trgm`, `pgcrypto`, `postgres_fdw`, `btree_gin`/`btree_gist`, `hstore`, `citext`, `ltree`, `intarray`, `cube`, `bloom`, `amcheck`, `pg_prewarm`, `pg_buffercache`, `pg_visibility`, `pg_walinspect`, `pageinspect`, `pgstattuple`, `tablefunc`, `unaccent`, `file_fdw`, `dblink`, `fuzzystrmatch`, `isn`, `lo`, `passwordcheck`, `pgrowlocks`, `pg_freespacemap`, `pg_surgery`, `seg`, `sslinfo`, `tcn`, `test_decoding`, `uuid-ossp`, `xml2`, `auth_delay`, and the rest of the contrib tree — plus the `CREATE EXTENSION` lifecycle and the `shared_preload_libraries` / `session_preload_libraries` distinction.

PostgreSQL 18 audited 2026-05-07, with notes on functionality that moved into core in earlier versions (e.g., `gen_random_uuid` graduated from `pgcrypto` to core in PG 13). Out of scope: third-party extensions (PostGIS, TimescaleDB, pgvector, Citus, pg_partman, pg_cron, pg_repack, plv8) — they ship outside the contrib tree. Grounded in the official docs at https://www.postgresql.org/docs/current/contrib.html, with inline `Full docs: <url>` links under every section so the agent can WebFetch upstream when it needs deeper detail.

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
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-extensions/claude.md \
  -o ~/.claude/agents/postgres-extensions-specialist.md

# Project-level
mkdir -p .claude/agents
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-extensions/claude.md \
  -o .claude/agents/postgres-extensions-specialist.md
```

The frontmatter registers it as a subagent named `postgres-extensions-specialist`. Invoke it by asking Claude Code to "use the postgres-extensions-specialist agent", or delegate programmatically via the `Agent` tool with `subagent_type: "postgres-extensions-specialist"`.

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
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-extensions/codex.md \
  -o ~/.codex/AGENTS.md

# Per-project — scoped to the current directory
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-extensions/codex.md \
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
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-extensions/copilot.md \
  -o .github/copilot-instructions.md
```

For an always-on personal setup (active in every directory), check `copilot --help` for the user-level custom-instructions path — last checked it lives under `~/.copilot/`.

---

## Updating

These files track the live PostgreSQL contrib docs. To refresh, re-run the relevant `curl` above — nothing else is needed.

## Provenance and scope

- Built from the public PostgreSQL docs at https://www.postgresql.org/docs/current/contrib.html (Appendix F), and the canonical per-module pages: `pgstatstatements.html`, `auto-explain.html`, `pgtrgm.html`, `pgcrypto.html`, `postgres-fdw.html`, `btree-gin.html`, `btree-gist.html`, `hstore.html`, `citext.html`, `ltree.html`, `intarray.html`, `cube.html`, `bloom.html`, `amcheck.html`, `pgprewarm.html`, `pgbuffercache.html`, `pgvisibility.html`, `pgwalinspect.html`, `pageinspect.html`, `pgstattuple.html`, `tablefunc.html`, `unaccent.html`, `file-fdw.html`, `dblink.html`, `fuzzystrmatch.html`, `isn.html`, `lo.html`, `passwordcheck.html`, `pgrowlocks.html`, `pgfreespacemap.html`, `pgsurgery.html`, `seg.html`, `sslinfo.html`, `tcn.html`, `test-decoding.html`, `uuid-ossp.html`, `xml2.html`, `auth-delay.html`, `earthdistance.html`, plus `sql-createextension.html` and `extend-extensions.html` for the extension lifecycle.
- Covers PostgreSQL 18 (release notes audited 2026-05-07) with notes on functionality promoted to core in earlier versions (e.g., `gen_random_uuid()` moved from `pgcrypto` to core in PG 13; `uuidv7()` added in PG 18).
- Intended as a fast-retrieval reference for an LLM, not a substitute for the upstream docs. When in doubt, link the user to the canonical page.
- Out of scope: non-bundled extensions (PostGIS, TimescaleDB, pgvector, Citus, pg_partman, pg_cron, pg_repack, plv8) and the PGXN distribution network — these are mentioned only as "see also" pointers.
- Claims not directly stated in the docs are explicitly hedged in-text.
