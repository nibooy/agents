# agents

[![verify](https://github.com/nibooy/agents/actions/workflows/verify.yml/badge.svg)](https://github.com/nibooy/agents/actions/workflows/verify.yml)
[![links](https://github.com/nibooy/agents/actions/workflows/links.yml/badge.svg)](https://github.com/nibooy/agents/actions/workflows/links.yml)
[![license](https://img.shields.io/github/license/nibooy/agents)](./LICENSE)

Agent prompts for [Claude Code](https://docs.claude.com/en/docs/claude-code), [OpenAI Codex](https://github.com/openai/codex), and [GitHub Copilot](https://docs.github.com/en/copilot).

Each topic folder holds one source of knowledge and three generated tool-facing files, so the same knowledge is served to whichever AI tool a teammate uses.

## Layout

```
agents/
  kubefleet/
    knowledge.md               # shared body — the only file edited by hand
    claude.frontmatter.yaml    # YAML header wrapped onto claude.md
    claude.md                  # generated: frontmatter + knowledge.md
    codex.md                   # generated: knowledge.md verbatim (Codex AGENTS.md)
    copilot.md                 # generated: knowledge.md verbatim (Copilot instructions)
    README.md                  # per-topic install guide
    images/                    # screenshots referenced by the README
```

CI regenerates `claude.md`, `codex.md`, and `copilot.md` from `knowledge.md` on every push and fails the build if any has drifted, so no tool ends up with different knowledge.

## How to use

Pick the topic folder you care about and follow its `README.md` for install instructions per tool.

## Topics

- [argocd](argocd/) — declarative GitOps continuous delivery for Kubernetes (CNCF graduated)
- [dockerfile](dockerfile/) — the Dockerfile (instructions, parser directives, BuildKit `--mount`, multi-stage, secrets, cache backends, attestations, `.dockerignore`)
- [golang](golang/) — the Go programming language (spec, generics, concurrency, modules, testing, profiling, stdlib)
- [helm](helm/) — the package manager for Kubernetes (charts, templating, hooks, OCI registries, CLI)
- [kubefleet](kubefleet/) — multi-cluster Kubernetes management (CNCF sandbox)
- [kubernetes](kubernetes/) — the container orchestration platform itself (workloads, networking, storage, RBAC, troubleshooting)

More topics may be added over time — see [CONTRIBUTING.md](./CONTRIBUTING.md) for the scaffold.

## Contributing

Corrections, new topics, and feedback are welcome. Start with [CONTRIBUTING.md](./CONTRIBUTING.md) — it covers the editing flow, how to audit a claim against upstream docs, and how to add a new topic.

Found a fact that's wrong? Open an issue with the **Incorrect knowledge** template. Want a new agent for a tool we don't cover yet? Use the **New agent request** template — valid repository and/or docs URLs are required so the topic can actually be built.
