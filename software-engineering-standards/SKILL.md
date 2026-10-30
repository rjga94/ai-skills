---
name: software-engineering-standards
description: Apply software engineering and architecture guidance when designing, implementing, refactoring, testing or reviewing software.
argument-hint: "[project, architecture, or quality concern]"
metadata:
  author: Ricardo Abreu
  version: "1.5.5"
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
- **Files:** Keep files small and single-purpose, prefer one object per file unless objects relate directly only to each other. A file that has grown large enough to require scrolling through many responsibilities is a signal to split it — extract cohesive modules or functions rather than accumulating unrelated code.
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
  File names are suffixed with 'LocalDataSource' or 'RemoteDataSource' appropriately.
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

Each screen has its own folder named appropriately for what the screen is (placed inside the Screen sub-folder). Keep the screen, its ViewModel, and its State together in that folder; do not place them as siblings directly under `Screen`. The folder contains these three files:

- Declarative UI file
- ViewModel file
- State file

Declarative UI file: A screen is a navigation destination that provides destination-level context, defines the UI structure, and arranges components. Build the UI from components in the main `Component` folder; keep the screen focused on composition rather than implementing component UI pieces itself. The screen object is named the same as its screen folder with the "Screen" suffix and remains agnostic of domain state and business logic.

ViewModel file: An object which has the responsibility maintaining the screen's state and logic, it's also the link between UI and Domain layer, can use repositories and use cases directly.
The object is named the same as it's screen folder with the "ViewModel" suffix.

State file: A model object, the simplest of the three, declares the model of the state that will be maintained by the view model and used by the screen UI. The object is named the same as it's screen folder with the "State" suffix.

##### Component sub-folder

A Component is an isolated piece of UI, designed with a clear reusable interface to reduce duplication and make future reuse possible. Its interface is its minimal usage contract: the state or data it accepts, the user actions it reports, and any intentional customization it supports. Callers should use this contract without depending on the component's internal implementation. This does not require a formal interface or protocol, or extensive configuration; keep the contract as small as the component needs. No component is private to one screen. Put every component in the main `Component` folder, including components currently used by only one screen; do not keep component implementations in a screen folder. Build UI from components, while screens define the structure and placement that aggregate them.

**MUST COMPLY**

Do not build look alike components.

Always consider adapting a component first before creating a new one.

Whenever a component changes, you MUST check all it's implementors to make sure none of them broke visually or logically.

There are two types of components:

- Stateless: A single file with a single object in it, takes in state/data from the outside and outputs user interactions. The object is named the same as it's component folder with the "Component" suffix.
- Stateful: A component that owns the state needed for its piece of UI. Keep it in the main `Component` folder and give it a focused interface; unlike a screen, it is not a navigation destination. Do not apply screen-folder organization to stateful components.

Use a stateless component by default when its screen ViewModel supplies the state/data and handles the feature behavior; the component renders that input and reports user actions. Choose a stateful component when it owns a cohesive, independently managed feature slice, including its state/data loading and feature-specific behavior.

As a practical test, ask: if this component moved to another screen, would it still need to load the same feature data and perform the same business validation? If yes, that is evidence the responsibility belongs to an isolated component with its own focused ViewModel, rather than being tied to either screen. If no, keep it stateless and let the screen ViewModel own that behavior. A shared endpoint alone is not enough reason to add a ViewModel; the data and business responsibility should belong to the same feature. Keep transient interaction state close to the component when appropriate, but do not add a ViewModel solely to manage simple UI state. If the ownership boundary remains unclear, confirm it with the user.

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
- Are each screen's UI, ViewModel, and State files colocated in that screen's own folder?
- Is every UI piece implemented as a component in the main `Component` folder, including pieces currently used by only one screen?
- Does each screen act only as a destination shell that provides context and aggregates components?
- While working on this change, have you considered all direct and indirect related code,
  consumers, dependencies, boundaries and documentation?
