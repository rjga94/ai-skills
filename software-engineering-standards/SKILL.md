---
name: software-engineering-standards
description: Apply SOLID, testability, modularity and maintainability standards when implementing, refactoring, testing or reviewing code.
argument-hint: "[project, language, or quality concern]"
metadata:
  author: Ricardo Abreu
  version: "1.3.0"
---

# Software Engineering Standards

Use this skill whenever editing code, creating or modifying a coding or software related
project. Apply the guidance proportionately to the
project, even when the user does not explicitly mention standards.

## Continuous Evaluation and Refactoring

Whenever this skill is triggered, continuously evaluate the requested change
against the standards below and inspect both directly and indirectly related
files. This includes affected dependency chains, consumers, interfaces, tests,
build configuration, generated templates, documentation, and module or runtime
boundaries. Do not limit the evaluation to the files named in the request.

Refactor qualifying issues as part of the current task. Make one bounded pass
across the affected dependency neighborhood: always fix standards violations
that affect correctness, compilation, tests, or consistency; usually fix
related consumers, tests, build files, generated templates, and documentation;
and fix nearby design issues when the behavior is understood and protected by
appropriate verification. Keep the work tied to the requested change and its
impact surface.

Breaking API, ABI, schema, persistence, module-boundary, or externally
observable behavior changes are allowed when they materially improve design,
scalability, testability, maintainability, or future-proofing. Ask the user for
approval before applying any such breaking change. Do not introduce speculative
abstractions, unrelated rewrites, or hypothetical compatibility layers.

When the user refers to "standards" in a project involving code, interpret it
as a request to practice sound software engineering across the relevant scope.
Treat the following as the baseline, not as an exhaustive checklist:

- **SOLID principles:** Keep responsibilities focused, dependencies replaceable, and
  abstractions justified by real variation or ownership boundaries.
- **Testability:** Design seams for focused unit tests and verify important
  user or system behavior with end-to-end tests. Keep tests deterministic,
  meaningful, and maintainable.
- **Duplication reduction:** Identify repeated logic, knowledge, and
  configuration. Reuse only when the duplicated behavior has a coherent
  abstraction; do not force unrelated code together merely to remove similar
  lines.
- **Modularity:** Organize responsibilities into cohesive modules at both the
  project level and within individual projects. Keep dependencies explicit and
  boundaries useful.
- **Project structure:** Use a predictable folder and source layout that
  reflects ownership, build boundaries, runtime layers, and test placement.
- **Scalability and future-proofing:** Evaluate whether the design remains
  maintainable at scale while avoid speculative abstractions and premature optimization.
- **Naming:** Give functions, classes, variables, modules, and other objects
  intention-revealing names. Names should communicate purpose and behavior,
  make meaningful distinctions between related concepts, and avoid disinformation
  such as misleading types, stale terminology, ambiguous abbreviations, or names
  that imply guarantees the object does not provide.
- **Object design:** Keep objects, including classes, structs, modules, and
  similar abstractions, focused on one responsibility. Keep their operations
  small and focused on one thing, with each operation at one level of abstraction
  rather than mixing high-level coordination with low-level implementation details.
- **File size:** Keep files small and single-purpose. A file that has grown
  large enough to require scrolling through many responsibilities is a signal
  to split it — extract cohesive modules or functions rather than accumulating
  unrelated code. Apply proportionately: framework glue, generated code, and
  large data tables may justify longer files, but flag those exceptions
  explicitly instead of silently accepting bloat.
- **Comments:** Prefer code that explains itself through clear structure and
  naming. Add concise comments when they explain inherently complex domain or
  technical behavior only, but refactor complexity caused by poor design before
  documenting it with comments.

## Adaptive Practice

The baseline concepts do not limit the practices that may be recommended or
applied. When the prompt or project context calls for it, also consider
separation of concerns, dependency management, appropriate design patterns,
API design, documentation, observability, security, accessibility, performance,
resilience, and maintainability.

Introduce an additional practice when it has a concrete benefit for
organization, maintainability, testability, reliability, security, or user
outcomes. Apply it proportionately to the project; avoid cargo-cult rules,
unnecessary abstractions, and process that does not improve the result.

## Review Checklist

- Are responsibilities cohesive and dependencies appropriately directed?
- Is the design testable without excessive mocking or infrastructure setup?
- Does the test suite cover behavior at unit and end-to-end boundaries where
  appropriate, and does it remain deterministic and maintainable?
- Is repeated knowledge centralized without creating a premature abstraction?
- Are modules, projects, and folders organized around clear responsibilities?
- Have both direct and indirect consumers, dependencies, boundaries, build
  files, generated templates, and documentation been evaluated?
- Will the design scale with plausible growth in data, users, modules,
  integrations, platforms, or contributors?
- Are coupling, bottlenecks, rigid assumptions, and migration risks addressed or
  explicitly reported as deferred findings?
- Do names reveal intent, distinguish related concepts meaningfully, and avoid
  misleading or disinformative terminology?
- Are objects focused on one responsibility, with small operations written at
  one level of abstraction?
- Are files kept small and single-purpose, with any oversized files justified by
  a concrete reason rather than accumulated cruft?
- Does the code explain itself, with comments reserved for unavoidable complexity
  rather than compensating for code that needs refactoring?
- Would another relevant engineering practice materially improve this change?
- Are the chosen practices proportionate to the project's size and context?
- Were all deferred standards findings reported with their impact, rationale,
  and recommended follow-up?
