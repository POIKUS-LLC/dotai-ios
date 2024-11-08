Certainly! Below is the **AI Codex Splitting Protocol** tailored specifically for Swift projects. This adaptation aligns with Swift's common project structures, technologies, and best practices to ensure seamless integration and effective organization of your AI Codex.

---

# AI Codex Splitting Protocol for Swift Projects

**Invocation:** `@split-codex.md`

## Purpose

This protocol is used to intelligently split the AI Codex when it becomes too lengthy, grouping related Errors (E) and Learnings (L) into separate codex pages tailored for Swift projects.

## Process

1. **Analyze the current `@codex.md` file**
2. **Identify common themes or categories among entries**
3. **Group related E and L entries**
4. **Create new codex pages for each group**
5. **Update the main `@codex.md` with links to new pages**
6. **Report the new structure**

## Grouping Criteria

- **Project Area:**
  - e.g., **AppDelegate**, **Networking**, **UI Components**
- **Technology or Framework:**
  - e.g., **SwiftUI**, **Combine**, **CoreData**
- **Concept:**
  - e.g., **Authentication**, **State Management**, **Testing**
- **File Type or Location:**
  - e.g., **Configuration Files**, **Utility Classes**, **View Models**

## File Structure

1. **Main Codex:**
   - Location: `.ai/codex/codex.md`
2. **New Pages:**
   - Location: `.ai/codex/[category]-codex.md`
   - Example:
     - `swiftui-codex.md`
     - `networking-codex.md`
     - `authentication-codex.md`

## Update Process

1. **Move Relevant Entries:**
   - Transfer entries to their respective category-specific codex files based on the grouping criteria.
2. **Update Main Codex (`.ai/codex/codex.md`):**

   - Replace moved entries with hyperlinks to the new category-specific codex pages.
   - Example:

     ```markdown
     ## Errors

     - [E001: Force Unwrapping Optionals](./networking-codex.md#e001)
     - [E002: Missing @UIApplicationMain](./appdelegate-codex.md#e002)
     ```

3. **Ensure Cross-References:**
   - Update all related entry references to point to the correct locations across all codex files.
   - Maintain consistency in linking to preserve the integrity of related entries.

## Naming Convention

Use **kebab-case** for new codex file names to ensure readability and consistency. Examples include:

- `swiftui-codex.md`
- `combine-codex.md`
- `authentication-codex.md`
- `networking-codex.md`
- `view-models-codex.md`

## Reporting

After splitting, provide a summary of the changes, including:

1. **Number of New Codex Pages Created:**
   - Example: _5 new codex pages created._
2. **Categories Identified:**
   - Example: _SwiftUI, Combine, Networking, Authentication, View Models._
3. **Entry Distribution Across New Pages:**
   - Example:
     - `swiftui-codex.md`: 10 entries
     - `combine-codex.md`: 5 entries
     - `networking-codex.md`: 7 entries
     - `authentication-codex.md`: 3 entries
     - `view-models-codex.md`: 4 entries

**IMPORTANT:** Maintain the original entry IDs (E001, L001, etc.) in the new files to preserve existing references.

---

## Example Implementation

### Before Splitting (`.ai/codex/codex.md`)

```markdown
# AI Codex

## Errors

E001:

- Context: `/Sources/Networking/APIService.swift`
- Error: Force unwrapping optionals leading to potential runtime crashes
- Correction: Use optional binding (`if let` or `guard let`) to safely unwrap optionals
- Prevention: Avoid using `!` for unwrapping; implement safe unwrapping practices
- Related: L002

E002:

- Context: `/Sources/App/AppDelegate.swift`
- Error: Missing `@UIApplicationMain` attribute in AppDelegate
- Correction: Add `@UIApplicationMain` above the AppDelegate class declaration
- Prevention: Ensure `@UIApplicationMain` is included when setting up the AppDelegate
- Related: None

## Learnings

L002:

- Context: `/Sources/Networking/APIService.swift`
- Insight: Avoid force unwrapping optionals to prevent unexpected crashes
- Application: Implement safe unwrapping techniques such as `guard let` or `if let` when dealing with optionals
- Impact: Increased application stability and reduced runtime errors related to optionals
- Related: E001

L003:

- Context: `/Sources/App/AppDelegate.swift`
- Insight: Proper setup of the AppDelegate is crucial for application lifecycle management
- Application: Ensure all necessary lifecycle methods (`application(_:didFinishLaunchingWithOptions:)`, etc.) are correctly implemented
- Impact: Stable application behavior and reliable response to lifecycle events
- Related: None
```

### After Splitting

#### Main Codex (`.ai/codex/codex.md`)

```markdown
# AI Codex

## Errors

- [E001: Force Unwrapping Optionals](./networking-codex.md#e001)
- [E002: Missing @UIApplicationMain](./appdelegate-codex.md#e002)

## Learnings

- [L002: Safe Unwrapping Techniques](./networking-codex.md#l002)
- [L003: AppDelegate Lifecycle Management](./appdelegate-codex.md#l003)
```

#### Networking Codex (`.ai/codex/networking-codex.md`)

```markdown
# Networking Codex

## Errors

**E001:**

- **Context:** `/Sources/Networking/APIService.swift`
- **Error:** Force unwrapping optionals leading to potential runtime crashes
- **Correction:** Use optional binding (`if let` or `guard let`) to safely unwrap optionals
- **Prevention:** Avoid using `!` for unwrapping; implement safe unwrapping practices
- **Related:** [L002](./networking-codex.md#l002)

## Learnings

**L002:**

- **Context:** `/Sources/Networking/APIService.swift`
- **Insight:** Avoid force unwrapping optionals to prevent unexpected crashes
- **Application:** Implement safe unwrapping techniques such as `guard let` or `if let` when dealing with optionals
- **Impact:** Increased application stability and reduced runtime errors related to optionals
- **Related:** [E001](./networking-codex.md#e001)
```

#### AppDelegate Codex (`.ai/codex/appdelegate-codex.md`)

```markdown
# AppDelegate Codex

## Errors

**E002:**

- **Context:** `/Sources/App/AppDelegate.swift`
- **Error:** Missing `@UIApplicationMain` attribute in AppDelegate
- **Correction:** Add `@UIApplicationMain` above the AppDelegate class declaration
- **Prevention:** Ensure `@UIApplicationMain` is included when setting up the AppDelegate
- **Related:** None

## Learnings

**L003:**

- **Context:** `/Sources/App/AppDelegate.swift`
- **Insight:** Proper setup of the AppDelegate is crucial for application lifecycle management
- **Application:** Ensure all necessary lifecycle methods (`application(_:didFinishLaunchingWithOptions:)`, etc.) are correctly implemented
- **Impact:** Stable application behavior and reliable response to lifecycle events
- **Related:** None
```
