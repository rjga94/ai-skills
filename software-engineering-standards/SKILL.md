---
name: software-engineering-standards
description: Apply SOLID, testability, modularity and maintainability standards when implementing, refactoring, testing or reviewing code.
argument-hint: "[project, language, or quality concern]"
metadata:
  author: Ricardo Abreu
  version: "1.4.0"
---

# Software Engineering Standards

Use this skill whenever editing code, creating or modifying a coding or software related
project. Apply the guidance proportionately to the
project. Follow the guidance in this skill carefully and thoroughly.

Continusly evaluate every code change against this skill's standards and match them as closely as possible at all times, that means continuous refactoring of the code directly and indirectly related to what's being changed, including architecture, structure and general project organization.

## SOLID Principles

*SOLID* is an acronym for the first five object-oriented design. These principles establish practices for developing software with considerations for maintaining and extending it as the project grows. Adopting these practiceshelp avoid code smells, refactor code, and develop Agile or Adaptive software.

- **Single Responsibility:** Give each module or type one cohesive responsibility
  and one primary reason to change.
- **Open/Closed:** Make behavior extendable without repeatedly modifying stable
  code, using abstractions where they provide a real benefit.
- **Liskov Substitution:** Ensure an implementation can stand in for its
  abstraction without breaking the expectations of its consumers.
- **Interface Segregation:** Keep interfaces focused so consumers do not depend
  on operations they do not use.
- **Dependency Inversion:** Make high-level policy depend on abstractions rather
  than low-level implementation details; let details implement those abstractions.

## Clean code key points
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

## Review Checklist

Ask yourself these questions to help guide you towards Clean Code and architecture and as a check that the skill had the desired effect on the changed software. If the answer to these is not a Yes then review and try to improve, if you can't reach the Yes on your own ask the user about it.

- Are the SOLID principles applied as closely as possible?
- Are you able to test the changed software entirelly locally using unit tests?
- Are you able to test the changed software entirelly locally by manually using the software yourself?
    - If Yes, can the manual test be turned into an automated test?
- Is the changed software fully absent of duplicated code?
- Have both direct and indirect consumers, dependencies, boundaries, build
  files, generated templates, and documentation been evaluated?
- Is the changed software contributing positively to scalability?
- Do names reveal intent, distinguish related concepts meaningfully, and avoid
  misleading or disinformative terminology?
- Does the code explain itself, with comments reserved for unavoidable complexity
  rather than compensating for code that needs refactoring?
