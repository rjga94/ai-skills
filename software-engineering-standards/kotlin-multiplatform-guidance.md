# Kotlin Multiplatform Guidance

Apply the guidance in [Software Engineering Standards](SKILL.md) together with the guidance in this file to Kotlin Multiplatform (KMP).

## Working with Room

**This guidance is specific for Room database libraries, if the project in context does not use Room, ignore this guidance**

Keep room specific setup code organized inside a 'room' folder directly inside the Data folder.
Entity and Dao of the same type should be kept in the same file inside the datasource/local folder.

## Presentation layer

Assume Compose shared UI across platform targets.

Use native platform UI only when the UI for that platform is fundamentally different, when the user requests it or the existing project is already built around it.

Compose and Compose Multiplatform has official libraries for Material design, build Screens and Components on top of Material using those libraries.

Keep Compose views focused on rendering its state and forwarding user actions.

Keep screen state and presentation behavior in the ViewModel.

## Suggested libraries

Use these by default unless asked for something different or when alternatives fit better the project or its targets.

| Concern | Suggested library |
| --- | --- |
| Dependency injection | Koin |
| Persistence | Room |
| Asynchronous | Coroutines and Kotlin Flow libraries |
| Networking | Ktor |
| Notifications | Firebase |
| UI | Material official libraries compatible with KMP |
