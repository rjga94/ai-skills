# Apple Platform Guidance

Apply the platform-neutral concepts in [Frontend Architecture Guidance](SKILL.md) to Apple platform projects using Xcode, Swift and SwiftUI/UIKit. Adapt implementation details to the project's deployment targets and existing choices; this is guidance, not a set of requirements.

## App and presentation

Use SwiftUI as the default UI framework for new Apple-platform screens unless the project or user calls for UIKit or another approach.

Keep the `@main` type conforming to `App` and its root view in `Presentation`. Use the app's window or scene to compose the initial screen, and keep feature behavior out of the app entry point.

Treat each SwiftUI `View` as a declarative description of UI. Keep `body` focused on composing views from state, and move substantial presentation behavior into a screen-scoped presentation model rather than embedding it in view construction.

For new projects whose deployment targets support the Observation framework, use `@Observable` presentation models and let the owning view manage their lifetime with `@State`. Use `@Bindable` when a view needs bindings to observable model properties. For older deployment targets or established projects, follow the project's `ObservableObject` pattern, such as `@StateObject` for an owned model and `@ObservedObject` for an injected one.

Use `@State` for short-lived interaction state that belongs to one view, such as whether a sheet is presented or a control is expanded. Keep screen state in the screen's presentation model when it coordinates multiple views or represents screen behavior.

Use Swift concurrency for asynchronous work. Keep networking, persistence, and domain operations out of `View.body`; invoke them through the appropriate domain or data boundary, and represent loading, success, empty, and failure states in presentation state.

## Components, navigation, and theme

Follow the generic guidance for screen-specific versus shared components. Compose small SwiftUI views from standard controls, and promote a component to the shared `Component` directory when it is genuinely reused across screens.

Use `NavigationStack` for stack-based navigation where supported. Keep navigation state at the level needed by the flow, and avoid putting feature logic into view modifiers or destination closures.

Keep design tokens in `Theme`, using semantic system colors and text styles where appropriate. Support Dynamic Type, color schemes, VoiceOver labels, and other relevant accessibility settings when defining custom styling and controls.

## Suggested Apple frameworks

Prefer platform frameworks before adding dependencies. Choose based on the project's deployment targets and existing architecture.

| Concern | Suggested framework |
| --- | --- |
| UI | SwiftUI |
| Observation | Observation for supported targets; Combine / `ObservableObject` for older targets or established projects |
| Asynchronous work | Swift Concurrency (`async`/`await`) |
| Networking | `URLSession` |
| Persistence | SwiftData when supported; Core Data for older targets or existing projects |

## Review checklist

- Does the app entry point compose the root UI without owning feature behavior?
- Are SwiftUI views primarily declarative, with screen behavior in an appropriate presentation model?
- Is each state value owned at the narrowest scope that fits its lifetime and sharing needs?
- Are asynchronous and data operations kept out of `View.body`?
- Do custom views respect the app's theme, accessibility needs, and supported deployment targets?
- Are shared components reused across screens rather than promoted speculatively?
