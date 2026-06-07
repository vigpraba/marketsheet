# MarketSheet Documentation

This folder is the source of truth for MarketSheet product, architecture, design, and implementation planning.

GitHub Issues track the work. These docs preserve the decisions, system design, diagrams, and implementation guidance behind that work.

## Structure

```text
docs/
  phase-0/     Product and technical design docs mapped to Phase 0 stories.
  architecture/ Future deep-dive docs for production architecture topics.
  diagrams/    Mermaid diagrams rendered by GitHub.
  decisions/   Future architecture decision records.
  templates/   Reusable templates for stories, subtasks, ADRs, and design docs.
```

## How Issues Map To Docs

Each Phase 0 story should have one matching document:

| GitHub Story | Documentation |
| --- | --- |
| Define MarketSheet product scope and MVP boundaries | `phase-0/01-product-scope.md` |
| Create high-level system architecture | `phase-0/02-system-architecture.md` |
| Define low-level technical design | `phase-0/03-low-level-technical-design.md` |
| Define data model and core entities | `phase-0/04-data-model.md` |
| Define job queue and worker design | `phase-0/05-job-queue-design.md` |
| Define agentic AI design and guardrails | `phase-0/06-agentic-ai-design.md` |
| Define provider integration design | `phase-0/07-provider-integrations.md` |
| Define Phase 1 implementation stories | `phase-0/08-phase-1-implementation-stories.md` |

## Documentation Workflow

1. Create or update a GitHub issue for the work.
2. Update the corresponding doc in this folder.
3. Add diagrams in `docs/diagrams` when visual explanation helps.
4. Open a pull request that links the issue.
5. Keep docs and implementation aligned as the system evolves.

## Diagram Format

Diagrams are written in Mermaid inside Markdown files. This keeps diagrams version-controlled, reviewable, and editable in pull requests.

## Current Product Direction

MarketSheet is a sheet-to-campaign agent for digital sellers. It connects customer and product data, recommends the best campaign to run, drafts email and WhatsApp messages, and tracks who clicks, replies, or buys.
