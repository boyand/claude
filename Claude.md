## Architecture Changes and application refactorings
- Any change to data flow, component lifecycle, or system design requires explicit user confirmation
- Present options with trade-offs, recommend one, then wait for approval
- Examples: changing how sandboxes are managed, adding new database tables, modifying auth flow


## Implementation Team (MANDATORY)

**Every non-trivial implementation MUST use a full review team.** This is non-negotiable.

## Plan preparation
The **architect agent** must review any major implementation plan for side-effects, bad design and architecture patterns, which will prevent the further evolution of the project. All concerns need to be incorporated in the plan before major implementation starts. 


## Team Composition
- **1-2 Senior Engineers** — based on stack (e.g., backend-dev for Python, frontend-dev for TypeScript). These do the implementation work.
- **Architecture Reviewer** — reviews for correctness, side effects, and architectural consistency. Uses `architect` agent type.
- **Security Engineer** — reviews for vulnerabilities, OWASP top 10, secrets exposure. Uses `security-reviewer` agent type.
- **QA Engineer** — validates test coverage, edge cases, and quality standards. Uses `qa-expert` agent type.

## Workflow
1. Senior Engineers implement in parallel (backend/frontend/tests)
2. **After implementation completes**, spin up all 3 reviewers in parallel
3. Address all review findings before committing
4. Only commit after all reviewers approve

## When to Skip
- Single-line fixes (typos, obvious bugs)
- Documentation-only changes
- Configuration tweaks

## Quality Gates

- All tests pass
- Coverage targets (run `pnpm test:coverage`):
  - **Logic/utilities**: ≥80% (validators, transformers, hooks)
  - **Pure UI components**: ≥60% (render + key interactions)
  - **Entry points/re-exports**: No coverage needed
- Type hints on all functions
- Docstrings on public APIs
- No secrets in code

## Testing Strategy

**Test these (logic that can break):**
- Validators and schema enforcement
- Utility functions (cn, transforms)
- Component behavior and interactions
- API integrations and data flows

**Skip tests for:**
- Entry points (`main.tsx`, `index.ts` re-exports)
- Generated/build outputs (`dist/*`)
- Pure type definitions
- Static configuration files
- Simple passthrough components
