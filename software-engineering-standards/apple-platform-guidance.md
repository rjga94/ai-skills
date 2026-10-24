# Apple Platform Guidance

Apply the guidance in [Software Engineering Standards](SKILL.md) together with the guidance in this file to Xcode projects.

Use Apple's official documentation and guidance unless told otherwise

## UI

Use SwiftUI as the default UI framework for new Apple-platform screens unless the project or user calls for UIKit or another approach.

## Suggested Apple frameworks

Use these by default unless asked for something different or when alternatives fit better the project or its targets.

| Concern | Suggested framework |
| --- | --- |
| UI | SwiftUI |
| Asynchronous work | Swift Concurrency |
| Networking | URLSession |
| Persistence | SwiftData |