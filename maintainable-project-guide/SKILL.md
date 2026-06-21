---
name: maintainable-project-guide
description: 'Generate or update project-specific .claude/CLAUDE.md and .hermes/AGENTS.md maintainability guides; use for Japanese requests such as "CLAUDE.md と AGENTS.md を生成して" or "保守性重視のエージェント向けガイドを作って".'
---

Use this skill when the user wants to create or update agent-facing project guides for a project where long-term maintainability matters more than short-term velocity.

The user may provide:
- Project name and one-line description
- Tech stack
- Team size / collaboration style
- Specific risks or past failures
- Existing conventions they want to preserve
- Whether they use Hermes Agent in addition to Claude Code

Your job is to generate two files that share the same source of truth but are tuned for each consumer:

1. `.claude/CLAUDE.md` — Claude Code guide (action-oriented, implementation details)
2. `.hermes/AGENTS.md` — Hermes agent guide (rule-oriented, concise, strict)

If the user only says "make this reusable" or "create a guide", ask for the project-specific variables below before generating files.

## Core philosophy to preserve

- This is probably not the first attempt. Prioritize avoiding technical debt over shipping fast.
- Refactor early and often while the project is still light.
- Build architecture for future features, not only today's requirements.
- Prefer boring, testable code over clever code.
- State must have a single source of truth.
- Never push directly to main; always use branches and PRs.
- Never force-push.
- Do not make structural changes without tests.

## Required sections in `.claude/CLAUDE.md`

1. **Project overview** — name, purpose, and one-line value proposition
2. **Long-term vision** — future features that should shape today's architecture
3. **Refactoring policy** — when and how to refactor; test-before-restructure rule
4. **Decision criteria** — how to choose between feature work and refactoring
5. **Tech stack** — versions and key libraries
6. **File structure** — directory layout and responsibilities
7. **Architecture patterns** — concrete patterns with project-specific examples
8. **State management policy** — SSOT, library choice, where state lives
9. **Development rules** — issue/branch rules, commit conventions, PR rules, lint/build/test checklist
10. **Security/caution notes** — what not to automate, such as project-specific secret or environment handling

## Required sections in `.hermes/AGENTS.md`

1. **Project essence** — one-paragraph summary
2. **Maintainability rules table** — rule, detail, impact
3. **Implementation phases** — phased refactor/feature plan
4. **Forbidden actions** — absolute prohibitions (direct push, force-push, untested structural changes)
5. **Technical guide** — stack, state, testing, important patterns
6. **File structure** — concise tree
7. **Test strategy** — frameworks and what each layer tests
8. **Critical behavior specs** — behaviors that must not change without approval
9. **PR template** — copy-paste ready
10. **Review response rules** — how to handle review comments

## Project-specific variables

Before generating, collect or infer these values:

| Variable | Example | Used in |
|---|---|---|
| `{PROJECT_NAME}` | Todo App | both |
| `{PROJECT_DESCRIPTION}` | `{EXAMPLE_PROJECT_DESCRIPTION}` | both |
| `{TECH_STACK}` | React + TypeScript + Vite | both |
| `{STATE_LIBRARY}` | Zustand | both |
| `{STATE_LOCATION}` | `{EXAMPLE_STATE_LOCATION}` | both |
| `{KEY_PATTERNS}` | `{EXAMPLE_KEY_PATTERNS}` | CLAUDE.md |
| `{TEST_FRAMEWORKS}` | `{EXAMPLE_TEST_FRAMEWORKS}` | AGENTS.md |
| `{BUILD_COMMAND}` | `{EXAMPLE_BUILD_COMMAND}` | both |
| `{LINT_COMMAND}` | `{EXAMPLE_LINT_COMMAND}` | both |
| `{TEST_COMMAND}` | `{EXAMPLE_TEST_COMMAND}` | both |
| `{E2E_COMMAND}` | `{EXAMPLE_E2E_COMMAND}` | AGENTS.md |
| `{BRANCH_PATTERN}` | `feature/{issue}-{summary}` or `chore/{summary}` | both |
| `{COMMIT_LANGUAGE}` | Japanese | both |
| `{PR_LANGUAGE}` | Japanese | both |
| `{WORKTREE_NOTE}` | `{EXAMPLE_WORKTREE_NOTE}` | CLAUDE.md |
| `{RISKS}` | `{EXAMPLE_RISKS}` | AGENTS.md |

## Customization rules

- Replace every `{EXAMPLE_*}` placeholder with project-specific examples before writing the generated files.
- Keep the structure and rules intact; only the examples and tech details should change per project.
- Make sure CLAUDE.md explains **why** each rule exists, while AGENTS.md states the rule **as a rule**.
- Keep AGENTS.md stricter and shorter. CLAUDE.md can be more explanatory.
- Always include a note that `AGENTS.md` is derived from `CLAUDE.md` and that any project-specific higher-priority instruction file, such as `refactor-instructions.md`, takes precedence if conflicts arise.

## Output behavior

- If the user provides enough context, generate both files and offer to write them to the project root.
- If context is missing, ask for the variables above rather than hallucinating project details.
- When writing files, place them at `.claude/CLAUDE.md` and `.hermes/AGENTS.md` relative to the project root.
- Remind the user that these files should be kept in sync over time.
