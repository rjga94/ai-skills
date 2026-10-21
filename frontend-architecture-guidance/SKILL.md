---
name: frontend-architecture-guidance
description: Use when designing, implementing, refactoring, testing, or reviewing frontend architecture across platforms. Provides platform-neutral architecture guidance and platform-specific companion guidance.
argument-hint: "[feature or library choice]"
metadata:
  author: Ricardo Abreu
  version: "1.0.0"
---

# Frontend Architecture Guidance

Use this skill when designing, implementing, refactoring, testing, or reviewing frontend architecture. It is a guide, not a set of requirements; follow the project's existing choices and the user's direction when they differ.

For platform-specific guidance, apply the relevant companion alongside these concepts. For Kotlin Multiplatform projects, see [Kotlin Multiplatform guidance](kotlin-multiplatform.md).

## Architecture concepts

Use Domain-Driven Design (DDD) to model domain concepts and business rules independently of UI and infrastructure. Use Model-View-ViewModel (MVVM) where it fits: views render state and forward actions, while a presentation model—often a ViewModel—coordinates screen state and presentation behavior.

Keep domain rules out of the presentation model. Extract domain operations into use cases when they are shared across screens or when a distinct application operation benefits from a clear boundary.

Organize data access around repositories and data sources with clear responsibilities. Use repository contracts to keep domain behavior independent from concrete APIs, databases, or other storage when that boundary is useful.

Inject dependencies where it helps isolate behavior and test it with fakes instead of relying on live services or storage. Keep simple dependencies and components free of unnecessary abstraction.

Prefer components that receive the state they need and report user actions. Keep local interaction state close to a component when it does not need to be shared.

## Project structure

Keep frontend UI code under a `Presentation` directory. Place the main UI entry-point file directly in `Presentation`, alongside these subdirectories:

- `Screen`: one directory per screen. Each screen directory contains its UI, ViewModel, state models, and any screen-specific components. Build the screen UI from focused components.
- `Theme`: design-system tokens, with a separate file for each category, such as `Typography`, `Colors`, and `Dimens`.
- `Component`: components intended for reuse across screens.

Keep a component in its screen directory, in its own file, when only that screen needs it. Whenever creating or changing a component, check the other screens for a genuine reuse opportunity; move it to `Component` when it serves multiple screens rather than promoting it speculatively.

Components should usually be stateless, with the screen's ViewModel owning their state. A sufficiently complex component may have its own ViewModel when its behavior warrants an independent state boundary.

## Review checklist

- Is UI code primarily responsible for rendering state and forwarding actions?
- Are domain rules independent of UI and platform details?
- Are repositories and data sources separated where a meaningful boundary is needed?
- Can important behaviors be tested without live services or storage?
- Are abstractions and state ownership appropriate to the complexity they serve?
- Are screens, screen-specific components, shared components, and theme tokens organized in the appropriate directories?
