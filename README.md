# actg — AI Integration

Vendor-neutral AI knowledge base for the **actg** project, a Drupal 11 site.

This repository is the team's shared AI context: project conventions, architecture indexes, business-flow skills, agent definitions and ticket history. It lives **outside** the main `actg` repository so that AI context can evolve independently of application code and is never mixed into the project's git history or deployment artifacts.

It is generated and maintained with [ai-core](#) (stack: `drupal`, executed through DDEV).

## Getting started (new developer)

This repo is not used standalone. It lives at `actg/.ai/`, nested inside your `actg` checkout as its own git repo (hidden from the project's git through `.git/info/exclude`).

1. Clone `actg` as usual and start the environment:
   ```
   ddev start && ddev composer install
   ```
2. From the `actg` root, clone this repo directly into `.ai` (don't create the folder yourself):
   ```
   git clone git@github.com:CGMDEV91/actg-ai.git .ai
   ```
3. Wire the checkout (hooks, tools, agent entry files) by running the ai-core installer against the project:
   ```
   ~/devel/ai-core/install.sh ~/devel/bitbucket/actg
   ```
4. Open a **new** opencode / Claude Code session in the `actg` folder and run `/doctor`. Expected result: `RESULT: OK`.

To get updates: `cd .ai && git pull`.

## Structure

```
.ai/
├── orchestrator.md      # Entry point: environment facts, critical conventions, command reference
├── core-rules.md        # Generated rules shared by every agent
├── agents/              # Specialized agent definitions
├── skills/              # Business-flow knowledge, loaded on demand
├── project/parts/       # Hand-written project facts (intro.md, facts.md) — edit these
├── templates/           # Ticket template
├── scripts/             # Tools: gate, impact, callers, touchpoints, where, indexers
└── tickets/             # One file per ticket (prior art for future work)
```

Generated indexes (`code-index.json`, `api-index.json`, `callgraph.json`, `integrations.md`, known issues) are rebuilt automatically after merges and checkouts.

## Daily workflow

`/ticket <KEY> <what to do>` → `/plan` → `go` → `/gate` → `/impact` → `/review` → `/tests` → manual test → `/close`.

On `/close` two commits are proposed: one in `actg` and one in `.ai`. Push both.

## Editing project knowledge

Edit only `project/parts/intro.md` and `project/parts/facts.md` by hand, then regenerate:

```
~/devel/ai-core/install.sh ~/devel/bitbucket/actg --update --no-doctor
```

Commit and push the result from `.ai`.

## Why a separate repository

- **Clean history**: knowledge churn doesn't pollute the project's commits.
- **Tool-agnostic**: one canonical source read by every AI assistant.
- **Independent lifecycle**: versioned, branched and reviewed on its own schedule.
