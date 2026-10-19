---
name: kotlin-multiplatform
description: Use whenever implementing, refactoring, testing, or reviewing a Kotlin Multiplatform project. Provides frontend architecture guidance and suggested libraries.
argument-hint: "[feature or library choice]"
metadata:
  author: Ricardo Abreu
  version: "1.0.0"
---

# Kotlin Multiplatform

Use this skill whenever working on a Kotlin Multiplatform (KMP) project. It is a guide and template, not a set of requirements; follow the project's existing choices and the user's direction when they differ.

## Frontend architecture

Assume a shared UI across targets by default and use Compose Multiplatform. Use native platform UI only when the user requests it or the existing project is already built around it.

Use [Android's architecture recommendations](https://developer.android.com/topic/architecture) as a reference, together with Domain-Driven Design (DDD) and Model-View-ViewModel (MVVM). Apply these ideas across targets, adapting implementation details to the chosen UI framework.

Use a dedicated ViewModel for each screen. Keep Compose views focused on rendering state and forwarding user actions; keep screen state and presentation logic in the ViewModel wherever practical. When multiple ViewModels need the same domain behavior, consider extracting it into a Use Case in the domain layer.

Structure the data layer around repositories and data sources. Repositories provide the domain-facing data contract and coordinate data sources, which handle the underlying APIs, databases, or other storage. Adapt these patterns to the project's needs rather than treating them as mandatory.

## Suggested libraries

These are optional starting points, not required dependencies. Choose alternatives when they better fit the project or its targets.

| Concern | Suggested library |
| --- | --- |
| Dependency injection | Koin |
| Networking | Ktor |
| Persistence | Room when it supports the required targets; consider SQLDelight as an alternative |
| Asynchronous and reactive code | kotlinx.coroutines and Flow |
| Serialization | kotlinx.serialization |
| Cross-platform date and time | kotlinx.datetime when needed |
