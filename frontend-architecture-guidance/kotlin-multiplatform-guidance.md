# Kotlin Multiplatform Guidance

Apply the platform-neutral frontend architecture concepts in [Frontend Architecture Guidance](SKILL.md) to Kotlin Multiplatform (KMP), adapting implementation details to the project's targets and existing choices. This is guidance, not a set of requirements.

## UI and presentation

Assume a shared UI across targets and use Compose Multiplatform by default. Use native platform UI when the user requests it or the existing project is already built around it.

Prefer the app's standard Compose UI components and apply the app's theme. Build custom components by composing and styling those base components rather than recreating their behavior from scratch.

Implement the generic presentation-model role with a screen-scoped ViewModel where appropriate. Keep Compose views focused on rendering its state and forwarding user actions; keep screen state and presentation behavior in the ViewModel wherever practical. Most UI components should remain stateless, with component-owned state reserved for sufficiently complex interactions.

## Suggested libraries

These are optional starting points, not required dependencies. Choose alternatives when they better fit the project or its targets.

| Concern | Suggested library |
| --- | --- |
| Dependency injection | Koin |
| Persistence | Room when it supports the required targets; consider SQLDelight as an alternative |
| Asynchronous and reactive code | kotlinx.coroutines and Flow |
| Serialization | kotlinx.serialization |
| Cross-platform date and time | kotlinx.datetime when needed |

## Review checklist

- Is shared Compose UI appropriate for the project's targets, or is native UI needed?
- Are standard Compose components themed for the app, with custom components built on top of them where practical?
- Does each screen use a suitable ViewModel, with Compose views primarily rendering state and forwarding actions?
- Is behavior shared across ViewModels extracted into domain use cases where appropriate?
- Can domain and presentation behavior be tested without relying on live services or storage?
