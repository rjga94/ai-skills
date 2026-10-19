---
name: kotlin-multiplatform-guidance
description: Use whenever implementing, refactoring, testing, or reviewing a Kotlin Multiplatform project. Provides frontend and backend architecture guidance and suggested libraries.
argument-hint: "[feature or library choice]"
metadata:
  author: Ricardo Abreu
  version: "1.0.0"
---

# Kotlin Multiplatform Guidance

Use this skill whenever working on a Kotlin Multiplatform (KMP) project. It is a guide and template, not a set of requirements; follow the project's existing choices and the user's direction when they differ.

## Frontend architecture

Assume a shared UI across targets by default and use Compose Multiplatform. Use native platform UI only when the user requests it or the existing project is already built around it.

Use [Android's architecture recommendations](https://developer.android.com/topic/architecture) as a reference, together with Domain-Driven Design (DDD) and Model-View-ViewModel (MVVM). Apply these ideas across targets, adapting implementation details to the chosen UI framework.

Use a dedicated ViewModel for each screen. Keep Compose views focused on rendering state and forwarding user actions; keep screen state and presentation logic in the ViewModel wherever practical. When multiple ViewModels need the same domain behavior, consider extracting it into a Use Case in the domain layer.

Structure the data layer around repositories and data sources with clear interfaces. Expose repository contracts to the domain layer, keep concrete implementations behind those contracts, and have repositories coordinate data sources for APIs, databases, or other storage. Inject these dependencies so domain logic and ViewModels can be tested with fakes instead of relying on live services or storage.

## Backend architecture

For backend services, use Ktor by default unless the project or user chooses otherwise. Organize the server using layered architecture, commonly called Controller–Service–Repository:

- Ktor routes and handlers form the HTTP controller/transport layer. Map requests and responses through DTOs at this boundary; keep transport DTOs out of the domain layer.
- Services coordinate application operations and domain behavior rather than placing it directly in route handlers.
- Services depend on repository interfaces. Repository implementations coordinate data sources for databases and external services.
- Inject these dependencies so services and routes can be tested without live databases or network services.

## Review checklist

- Is shared Compose Multiplatform UI used by default, unless the project or user prefers native UI?
- Does each screen have a dedicated ViewModel, with Compose views primarily rendering state and forwarding actions?
- Is behavior shared across ViewModels extracted into domain Use Cases where appropriate?
- Are repository contracts clear and injectable, with data sources kept behind repository implementations?
- For backend code, do Ktor routes handle transport and DTO mapping while services contain application operations?
- Are domain, ViewModel, service, and route behaviors testable without depending on live databases or network services?
- Do suggested libraries fit the project's targets and existing choices?

## Suggested libraries

These are optional starting points, not required dependencies. Choose alternatives when they better fit the project or its targets.

| Concern | Suggested library |
| --- | --- |
| Dependency injection | Koin |
| Networking and backend services | Ktor |
| Persistence | Room when it supports the required targets; consider SQLDelight as an alternative |
| Asynchronous and reactive code | kotlinx.coroutines and Flow |
| Serialization | kotlinx.serialization |
| Cross-platform date and time | kotlinx.datetime when needed |
