# AI Codex

## Usage

- **Review:** `@codex.md` (silent load, no output)
- **Update:** `@learn.md`
- **File paths:** Always use absolute paths from project root

## Errors

**E000:**

- **Context:** `/Sources/App/AppDelegate.swift`
- **Error:** Missing `@UIApplicationMain` attribute in AppDelegate
- **Correction:** Add `@UIApplicationMain` above the AppDelegate class declaration
- **Prevention:** Ensure `@UIApplicationMain` is included when setting up the AppDelegate
- **Related:** None

**E001:**

- **Context:** `/Sources/Networking/APIService.swift`
- **Error:** Force unwrapping optionals leading to potential runtime crashes
- **Correction:** Use optional binding (`if let` or `guard let`) to safely unwrap optionals
- **Prevention:** Avoid using `!` for unwrapping; implement safe unwrapping practices
- **Related:** L002

**E002:**

- **Context:** `/Sources/Models/User.swift`
- **Error:** Conformance to `Codable` without implementing required methods for custom parsing
- **Correction:** Implement custom `init(from decoder: Decoder)` and `encode(to encoder: Encoder)` methods
- **Prevention:** Ensure all properties are Codable or provide custom encoding/decoding logic
- **Related:** None

## Learnings

**L007:**

- **Context:** `/Sources/Views/LoginView.swift`
- **Insight:** Utilizing SwiftUI’s `@State` and `@Binding` for state management enhances UI responsiveness
- **Application:** Implement `@State` for local view states and `@Binding` for passing state between views
- **Impact:** Improved UI reactivity and cleaner state management across SwiftUI views
- **Related:** L005, L008

**L008:**

- **Context:** `/Sources/ViewModels/AuthViewModel.swift`
- **Insight:** Implementing the MVVM pattern separates concerns and facilitates easier testing
- **Application:** Structure view models to handle business logic and bind them to SwiftUI views using `@ObservedObject`
- **Impact:** Enhanced code maintainability and scalability, enabling more efficient feature additions and bug fixes
- **Related:** L007

**L000:**

- **Context:** `/Sources/App/AppDelegate.swift`
- **Insight:** Proper setup of the AppDelegate is crucial for application lifecycle management
- **Application:** Ensure all necessary lifecycle methods (`application(_:didFinishLaunchingWithOptions:)`, etc.) are correctly implemented
- **Impact:** Stable application behavior and reliable response to lifecycle events
- **Related:** None

**L001:**

- **Context:** `@codex.md` usage
- **Insight:** `@codex.md` is designated for context loading and should not be directly modified
- **Application:** Use `@codex.md` solely for silent loading and context retrieval; perform updates through designated commands
- **Impact:** Maintained integrity and consistency of the codex documentation
- **Related:** None

**L002:**

- **Context:** `/Sources/Networking/APIService.swift`
- **Insight:** Avoid force unwrapping optionals to prevent unexpected crashes
- **Application:** Implement safe unwrapping techniques such as `guard let` or `if let` when dealing with optionals
- **Impact:** Increased application stability and reduced runtime errors related to optionals
- **Related:** E001
