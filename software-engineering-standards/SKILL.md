---
name: software-engineering-standards
description: Apply software engineering and architecture guidance when designing, implementing, refactoring, testing or reviewing software.
argument-hint: "[project, architecture, or quality concern]"
metadata:
  author: Ricardo Abreu
  version: "1.5.0"
---

# Software Engineering Standards

Use this skill whenever editing code, creating or modifying a coding or software related
project. Follow the guidance in this skill carefully and thoroughly, only differ when explicitly told.

**MUST COMPLY**

You are required on every code change, even small changes, to apply this skill everywhere in the project in context, anything that helps improve SOLID Principles and Clean code key points is worth doing, even if that means a big refactor for a small gain. Given these conditions, if a code change means a big refactor, ask the user for confirmation.

## SOLID Principles

*SOLID* is an acronym for the five principles of object-oriented design. These principles establish practices for developing and maintaining software as it grows. Adopting them helps avoid code smells, supports refactoring, and enables adaptive development.

- **Single Responsibility:** Give each module or type one cohesive responsibility and one primary reason to change.
- **Open/Closed:** Make behavior extendable without repeatedly modifying stable code, using abstractions where they provide a real benefit.
- **Liskov Substitution:** Ensure an implementation can stand in for its abstraction without breaking the expectations of its consumers.
- **Interface Segregation:** Keep interfaces focused so consumers do not depend on operations they do not use.
- **Dependency Inversion:** Make high-level policy depend on abstractions rather than low-level implementation details; let details implement those abstractions.

## Clean code key points

- **Testability:** Design seams for focused unit tests and verify important user or system behavior with end-to-end tests. Keep tests deterministic, meaningful, and maintainable.
- **Duplication reduction:** The code base should be kept as small as possible, you should identify repeated code or behaviour and refactor accordingly.
- **Modularity:** Organize responsibilities into cohesive modules. Keep dependencies explicit and boundaries useful.
- **Scalability:** The project should remain scalable, that means continuous refactoring on every change and close attention to SOLID principles.
- **Naming:** Give functions, classes, variables, modules, and other objects intention-revealing names. Names should communicate purpose and behavior, make meaningful distinctions between related concepts, and avoid disinformation such as misleading types, stale terminology, ambiguous abbreviations, or names that imply guarantees the object does not provide.
- **Object design:** Keep objects, including classes, structs, modules, and similar abstractions, focused on one responsibility. Keep their operations small and focused on one thing, with each operation at one level of abstraction rather than mixing high-level coordination with low-level implementation details.
- **File size:** Keep files small and single-purpose, prefer one object per file unless objects relate directly only to each other. A file that has grown large enough to require scrolling through many responsibilities is a signal to split it — extract cohesive modules or functions rather than accumulating unrelated code.
- **Comments:** Prefer code that explains itself through clear structure and naming. Add concise comments when they explain inherently complex domain or technical behavior only, attempt first to refactor complexity caused by poor design before documenting it with comments.

## Frontend Architecture

Use Domain-Driven Design (DDD) to model domain concepts and business rules independently of UI and infrastructure. Use Model-View-ViewModel (MVVM) for the presentation layerl, views render state and forward actions, while a presentation model—often a ViewModel—coordinates screen state and presentation behavior.

Keep domain rules out of presentation models. Extract domain operations into use cases when they are shared across screens or when a distinct application operation benefits from a clear boundary.

Organize data access around repositories and data sources with clear responsibilities. Use repository contracts to keep domain behavior independent from concrete APIs, databases, or other storage when that boundary is useful.

Inject dependencies where doing so helps isolate behavior and test it with fakes instead of relying on live services or storage. Keep simple dependencies and components free of unnecessary abstraction.

Prefer components that receive the state they need and report user actions. Keep local interaction state close to a component when it does not need to be shared.

Prefer one file for each object, named the same as the object itself.


**MUST COMPLY**

Below is a list of project type specific guidance files.

You should detect the project type and check if any of the project specific guidance fits for the project in context, use the project specific guidance together with the generic guidance in this file.

Only use the project specific guidance file that fits, ignore all others, if a project specific guidance is not available for the project in context, ignore all of them.

- [Kotlin Multiplatform guidance](kotlin-multiplatform-guidance.md)
- [Apple platform guidance](apple-platform-guidance.md)

### Frontend Project Structure

At the same application or module boundary, project structure is organized in three main folders `Data`, `Domain` and `Presentation`.

#### Data

Data should be kept and maitained in an Offline first caching fashion and ready for reactive UI.

- Data/DataSource: Integrations for external systems, organized by data type and source type such as Remote and Local.
- Data/Repository: Concrete repository implementations that fulfill the domain contracts and coordinate data sources.
- Data/Mapper: Conversions between domain models and transport or persistence models when those representations differ.

#### Domain layer

- Domain/Model: Domain entities and value types that represent the concepts and rules of the application.
- Domain/Repository: Contracts only in domain layer, organized per data type, each handles domain operations of that data type.
- Domain/UseCase: Add a use case when more than one view model needs to perform the same domain operation.

#### Presentation layer (declarative UI)

**This guidance is specific for Declarative UI, you can still use this for software that doesn't use Declarative UI but know that some things will not fit properly.**

The Presentation layer is organized in an MVVM fashion. 

UI should be reactive to data changes, use skeleton loading style to have a faster feeling UI, segment and parallelize data loading per component whenever possible.

Use blocking loading dialogs only for data changes triggered by user interactions (example: a save button).

Keep all UI code under a Presentation folder. Place the main UI entry-point file directly in Presentation, alongside these sub-folders:

- Screen
- Component
- Theme

##### Screen sub-folder

A Screen is a UI navigation destination, usually taking the whole screen.

Each screen has it's own folder named appropriately for what the screen is (placed inside the Screen sub-folder), the screen specific folder usually contains three files:

- Declarative UI file
- ViewModel file
- State file

Declarative UI file: Has the sole responsibility of containing UI building blocks of the screen, in declarative UI a screen is a single object, agnostic of domain state and and business logic. The object is named the same as it's screen folder with the "Screen" suffix.

ViewModel file: An object which has the responsibility maintaining the screen's state and logic, it's also the link between UI and Domain layer, can use repositories and use cases directly.
The object is named the same as it's screen folder with the "ViewModel" suffix.

State file: A model object, the simplest of the three, declares the model of the state that will be maintained by the view model and used by the screen UI. The object is named the same as it's screen folder with the "State" suffix.

##### Component sub-folder

A Component is a piece/portion of UI, meant to reduce duplication and isolation of UI code in screens and other components that need to use the same UI repeatedly.

**MUST COMPLY**

Do not build look alike components.

Always consider adapting a component first before creating a new one.

Whenever a component changes, you MUST check all it's implementors to make sure none of them broke visually or logically.

There are two types of components:

- Stateless: A single file with a single object in it, takes in state/data from the outside and outputs user interactions. The object is named the same as it's component folder with the "Component" suffix.
- Stateful: Technically exactly the same as a screen, only it still represents a piece of UI and not a navigation destination, so you can follow Screen guidance described in this skill.

**Most cases use stateless components but choosing between that or a stateful component is a complex decision, so whenever this decision needs to be made, confirm with the user.**

##### Theme sub-folder

The theme contains everything that is design system specific, such as Typography, Colors, Dimens, etc.

## Review Checklist

**MUST COMPLY**

These are YES/NO questions to be applyed on the changed software,
if the answer is NO to any of them, review and refactor accordingly.

- Are the SOLID principles in compliance?
- Is Domain Driven Design in compliance?
- Does the the Presentation layer comply to Model View View Model pattern?
- Is the software kept scalable and modular, with clear bounderies throughout?
- Are unit tests covering the changed software appropriately?
- Is the changed software fully absent of duplicated code, logic or behaviour?
- While working on this change, have you considered all direct and indirect related code,
  consumers, dependencies, boundaries and documentation?
