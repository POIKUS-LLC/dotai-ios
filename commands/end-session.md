# End Session Instructions for Swift Projects

**Invocation:** `@end-session.md`

## Process

1. **Call `@learn.md` to update the learnings.**

2. **Call `@codex.md` to update the codex.**

3. **Call `@split-codex.md` to split the codex into smaller files.**

4. **Create coding instructions:** If user asks specific coding instructions, summarize the instructions and save to `.ai/coding-instructions/YYYY-MM-DD-HH-MM-SS-<session-id>.md`.

5. **Create a new file:** `.ai/status/YYYY-MM-DD-HH-MM-SS-<session-id>.md`

6. **Structure the update with sections:**

   - Development Steps
   - Key Decisions
   - Next Steps

7. **For each section:**

   - Use bullet points
   - Be specific and use technical terms
   - Keep content concise

8. **Development Steps:**

   - List modified files in dependency order
   - Number each step
   - Briefly describe changes and purpose for each file
   - Use past tense

9. **Key Decisions:**

   - List important decisions, their reasoning, and trade-offs
   - Use past tense

10. **Next Steps:**

    - List 3-5 prioritized tasks/features to work on next
    - Specify affected components/code areas
    - Highlight blockers or challenges
    - Use future tense
    - **Note:** We don't care about performance and optimization for now. We need to get the core functionality working for MVP.

11. **For code blocks, prepend triple quotes with a space**

12. **Generate git commit message** using instructions in `.ai/commands/create-commit-message.md`.

13. **End with a brief summary** of overall progress and next session's focus

---

## Example:

# Session Update: 2024-04-27

## Git Commit Message

```
{COMMIT_MESSAGE}
```

## Development Steps

1. `Sources/App/AppDelegate.swift`: Added `@UIApplicationMain` attribute
   - Ensured proper application lifecycle management
2. `Sources/Networking/APIService.swift`: Refactored optional handling
   - Replaced force unwrapping with `guard let` statements to enhance stability
3. `Sources/Views/LoginView.swift`: Implemented `@State` for user authentication
   - Managed login state effectively using SwiftUI's state management
4. `Sources/ViewModels/AuthViewModel.swift`: Structured ViewModel using MVVM pattern
   - Separated business logic from views for better maintainability

## Key Decisions

- Chose to implement the MVVM architectural pattern to enhance code scalability and testability
- Decided to use SwiftUI's `@State` and `@Binding` for state management to leverage SwiftUI's reactive capabilities

## Next Steps

1. **Implement user authentication system**
   - Affected Components: `AuthViewModel.swift`, `LoginView.swift`, `UserService.swift`
   - **Blockers:** Need to integrate with backend authentication API
2. **Create dashboard view for authenticated users**
   - Affected Components: `DashboardView.swift`, `DashboardViewModel.swift`
   - **Challenges:** Designing intuitive UI/UX for the dashboard
3. **Set up CoreData for persistent storage**
   - Affected Components: `DataController.swift`, `User.swift`
   - **Blockers:** Configuring CoreData stack correctly
4. **Integrate Combine for reactive programming**
   - Affected Components: `APIService.swift`, `AuthViewModel.swift`
   - **Challenges:** Learning curve associated with Combine's operators

**Progress:** Completed main application setup and initial networking layer. Next session will focus on implementing user authentication and setting up the dashboard view.
