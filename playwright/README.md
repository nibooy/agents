# Playwright agent prompts

Reference knowledge for [Playwright](https://playwright.dev/) — the cross-browser end-to-end testing and automation framework from Microsoft. Same browser engine across Chromium, Firefox, and WebKit; first-party test runner (`@playwright/test`) with fixtures, projects, parallel workers, web-first assertions, traces, and reporters.

Covers locators (`getByRole`/`getByLabel`/`getByTestId`, filtering, chaining), actionability and auto-waiting, web-first assertions and `expect.poll`, custom fixtures via `test.extend`, `playwright.config.ts`, projects and sharding, `storageState`-based authentication, network interception with `page.route` and HAR replay, `APIRequestContext`, emulation, visual comparisons (`toHaveScreenshot`), the trace viewer, the CLI, and CI integration with the official `mcr.microsoft.com/playwright` Docker image. Grounded in the live docs at https://playwright.dev/docs/intro, with inline `Full docs: <url>` links under every section so the agent can fetch upstream when it needs more detail.

Scoped primarily to the Node.js/TypeScript flavor — the most-used and the one that ships the full test-runner story. The preamble points at the per-language docs (Python / Java / .NET) so the agent can pivot when needed.

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
curl -fsSL https://raw.githubusercontent.com/ytimocin/agents/main/playwright/claude.md \
  -o ~/.claude/agents/playwright-specialist.md

# Project-level
mkdir -p .claude/agents
curl -fsSL https://raw.githubusercontent.com/ytimocin/agents/main/playwright/claude.md \
  -o .claude/agents/playwright-specialist.md
```

The frontmatter registers it as a subagent named `playwright-specialist`. Invoke it by asking Claude Code to "use the playwright-specialist agent", or delegate programmatically via the `Agent` tool with `subagent_type: "playwright-specialist"`.

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
curl -fsSL https://raw.githubusercontent.com/ytimocin/agents/main/playwright/codex.md \
  -o ~/.codex/AGENTS.md

# Per-project — scoped to the current directory
curl -fsSL https://raw.githubusercontent.com/ytimocin/agents/main/playwright/codex.md \
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
curl -fsSL https://raw.githubusercontent.com/ytimocin/agents/main/playwright/copilot.md \
  -o .github/copilot-instructions.md
```

For an always-on personal setup (active in every directory), check `copilot --help` for the user-level custom-instructions path — last checked it lives under `~/.copilot/`.

**3. Launch** with `copilot` in the workspace.

---

## Updating

These files track the live Playwright docs. To refresh, re-run the relevant `curl` above — nothing else is needed.

## Provenance and scope

- Built from the public docs at https://playwright.dev/docs/intro and the repo at https://github.com/microsoft/playwright.
- Scoped to the Node.js/TypeScript `@playwright/test` runner. Per-language docs (Python / Java / .NET) are linked in the preamble so the agent can pivot when the user is on one of those ports.
- Intended as a fast-retrieval reference for an LLM, not a substitute for the upstream docs. When in doubt, link the user to the canonical page.
- Claims not directly stated in the docs are explicitly hedged in-text.
