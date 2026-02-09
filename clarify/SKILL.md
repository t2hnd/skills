---
name: clarify
description: >
  Review an existing CLAUDE.md, identify gaps, and refine it through targeted questions.
  Then ask broader project questions to capture learnings in MEMORY.md.
  Use after /cmd-generator or whenever the CLAUDE.md feels incomplete.
  Triggers: "clarify", "確認", "深掘り", "CLAUDE.md確認", "もっと詳しく", "refine CLAUDE.md".
---

# Clarify Skill

Refine a project's CLAUDE.md by identifying gaps, then capture broader project knowledge in MEMORY.md.

## Workflow

### Phase 1: Refine CLAUDE.md

#### 1. Locate and Read CLAUDE.md

Search for the project's CLAUDE.md in this order:
1. `./CLAUDE.md` (current working directory)
2. Nearest ancestor directory's `CLAUDE.md`
3. If none found, tell the user to run `/cmd-generator` first and stop.

Read the file fully before proceeding.

#### 2. Gap Analysis

Analyze the CLAUDE.md for weak areas. Check each section:

| Section | Look for |
|---------|----------|
| **Commands** | Missing key commands (dev, build, test, lint, deploy), undocumented flags/options |
| **Architecture** | Important directories not explained, missing layers (API, DB, middleware) |
| **Conventions** | Missing naming patterns, import order, error handling, logging conventions |
| **Do NOT** | Common pitfalls not yet listed, footguns specific to the stack |
| **Testing** | Missing test patterns, fixture setup, mocking approaches, coverage expectations |
| **Key Decisions** | Undocumented architectural choices that affect daily coding |

Also scan the actual codebase (package.json, directory structure, config files) to find things the CLAUDE.md should mention but doesn't.

#### 3. Ask Targeted Questions (Round 1)

Use `AskUserQuestion` with up to 4 questions about the weakest areas found in step 2.
Focus on gaps that would most improve Claude's effectiveness in this project.

#### 4. Update CLAUDE.md and Re-analyze

Apply the answers to the appropriate sections of CLAUDE.md. Show the user a summary of what was added or changed.

Then re-evaluate: are there still significant gaps? If so, proceed to Round 2.
If CLAUDE.md is now solid, skip directly to Phase 2.

#### 5. Ask Follow-up Questions (Round 2, if needed)

Use `AskUserQuestion` with up to 4 more questions, now informed by the Round 1 answers.
Good candidates for Round 2:
- Details that Round 1 answers revealed (e.g., user mentioned a test framework → ask about fixture conventions)
- Sections still weak after Round 1
- Cross-cutting concerns (error handling, logging, env config) not yet captured

Update CLAUDE.md with the answers and show what changed.

### Phase 2: Broader Project Q&A

#### 6. Ask Broader Questions (Round 3)

Ask up to 4 questions about topics NOT covered by CLAUDE.md:
- Workflow preferences (branching strategy, PR process, CI/CD pipeline)
- Team conventions (review process, deployment cadence, release tagging)
- Known pain points or tech debt areas
- Personal preferences (response language, verbosity, commit message style)

Skip any topic already well-documented in CLAUDE.md or MEMORY.md.

#### 7. Classify and Write Answers

Classify each answer and write to the appropriate file:

| Classification | Destination |
|---------------|-------------|
| Project-specific conventions, architecture, commands | Project `CLAUDE.md` |
| Project-specific workflow, gotchas, team conventions | Project `MEMORY.md` |
| Personal preferences, general tools/habits | `~/.claude/MEMORY.md` |

Before creating any MEMORY.md file, ask the user for approval.

#### 8. Show Summary

Present what was updated:
```
## Changes Made
- CLAUDE.md: added X, updated Y
- MEMORY.md (project): created with Z
- ~/.claude/MEMORY.md: appended W
```

## Important Rules

- **Always read CLAUDE.md first** — never ask about what's already documented
- **Use `AskUserQuestion` tool** for all questions (not free-text), with concrete options when possible
- **Maximum 3 rounds** of questions (4 per round, 12 total max) — Round 2 is conditional on remaining gaps
- **Always show what changed** after each update
- **Do NOT create MEMORY.md** files without explicit user approval
- **Do NOT ask generic questions** — every question must be grounded in an observed gap
- **Scan the codebase** to validate and supplement CLAUDE.md content (don't rely solely on the file)
- **Match the user's language** (Japanese or English) based on the existing CLAUDE.md
