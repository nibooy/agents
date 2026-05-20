# PostgreSQL SQL agent prompts

Reference knowledge for the **PostgreSQL SQL language** — Part II of the PostgreSQL manual: SQL syntax, DDL (CREATE TABLE, constraints, partitioning, RLS, schemas), DML (INSERT, UPDATE, DELETE, RETURNING, MERGE), queries (SELECT, joins, GROUP BY, set ops, CTEs), data types (incl. JSONB, arrays, ranges, multiranges), functions & operators, type conversion, indexes (B-tree, GIN, GiST, BRIN, SP-GiST, hash), full-text search, MVCC and transaction isolation, EXPLAIN/planner tuning, and parallel query.

Covers PostgreSQL 18 (current stable as of 2026-05) with audit notes against versions 15–18. Grounded in the official docs at https://www.postgresql.org/docs/current/sql.html, with inline `Full docs: <url>` links under every section so the agent can fetch upstream when it needs more detail.

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
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-sql/claude.md \
  -o ~/.claude/agents/postgres-sql-specialist.md

# Project-level
mkdir -p .claude/agents
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-sql/claude.md \
  -o .claude/agents/postgres-sql-specialist.md
```

The frontmatter registers it as a subagent named `postgres-sql-specialist`. Invoke it by asking Claude Code to "use the postgres-sql-specialist agent", or delegate programmatically via the `Agent` tool with `subagent_type: "postgres-sql-specialist"`.

The preamble directs the agent to WebFetch the linked upstream page on questions the summary doesn't fully cover, so it stays accurate as PostgreSQL evolves (new releases ship every September).

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
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-sql/codex.md \
  -o ~/.codex/AGENTS.md

# Per-project — scoped to the current directory
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-sql/codex.md \
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
curl -fsSL https://raw.githubusercontent.com/nibooy/agents/main/postgres/postgres-sql/copilot.md \
  -o .github/copilot-instructions.md
```

For an always-on personal setup (active in every directory), check `copilot --help` for the user-level custom-instructions path — last checked it lives under `~/.copilot/`.

---

## Updating

These files track the live PostgreSQL docs. To refresh, re-run the relevant `curl` above — nothing else is needed.

## Provenance and scope

- Built from the public docs at https://www.postgresql.org/docs/current/sql.html — the entry point for **Part II "The SQL Language"** of the PostgreSQL manual.
- Canonical chapter URLs: SQL syntax (`sql-syntax.html`), DDL (`ddl.html`), DML (`dml.html`), queries (`queries.html`), data types (`datatype.html`), functions and operators (`functions.html`), type conversion (`typeconv.html`), indexes (`indexes.html`), full-text search (`textsearch.html`), MVCC (`mvcc.html`), performance tips (`performance-tips.html`), parallel query (`parallel-query.html`).
- Covers through PostgreSQL 18 (current stable as of audit on 2026-05-07), with notes for 15–17 where syntax or defaults differ.
- **Out of scope**: SQL Command Reference (Part VI of the manual — `CREATE TABLE` exact syntax, `ALTER TABLE` action list, etc.). That is covered by a separate agent. This prompt focuses on the conceptual chapters of Part II.
- Intended as a fast-retrieval reference for an LLM, not a substitute for the upstream docs. When in doubt, link the user to the canonical page.
- Claims not directly stated in the docs are explicitly hedged in-text.
