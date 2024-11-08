**Invocation:** `@learn.md`

## Process:

1. **Analyze current session for error and learning indicators**
2. **Extract contextual information**
3. **Format new entries**
4. **Edit `.ai/codex/codex.md` (Append to relevant section)**
5. **Report updates**

---

## Error Identification:

- **Correction Phrases:**

  - "I apologize"
  - "That was incorrect"
  - "Let me correct"

- **Misunderstandings:**

  - Clarifications requested by user

- **Inconsistencies:**
  - Contradictions in AI responses

---

## Learning Identification:

- **New Information Phrases:**

  - "I see"
  - "I understand"
  - "It appears that"

- **Project Updates:**

  - Changes in structure, dependencies, or requirements

- **User Preferences:**
  - Specific requests or feedback on AI's approach

---

## Entry Format:

- **Context:** Specific file, class, function, or project area
- **Error/Insight:** Concise description
- **Correction/Application:** Precise fix or usage
- **Prevention/Impact:** Strategy to avoid future errors or potential effects
- **Related:** IDs of connected entries

**Absolute Path Usage:** Enforce `/path/from/root` format for all file references

---

## Critical Steps:

### 1. **Edit `.ai/codex/codex.md`**

- **Append New Entries to the Relevant Section:**

  - **Errors** or **Learnings**

- **Maintain Descending Order:**

  - Newest entries first

- **Ensure Unique, Incremental IDs:**

  - Each entry must have a distinct identifier

- **Cross-Reference Related Entries:**
  - Link related insights or errors for easy navigation

---

## Exclusions:

- **Configuration Errors:**

  - Xcode project settings
  - SwiftLint and code style issues

- **Dependency Management Issues:**

  - CocoaPods, Carthage, or Swift Package Manager configurations

- **Build Tool Configuration:**

  - Fastlane, Makefiles

- **Environment Setup Issues:**
  - `.env` files or other environment-specific configurations

---

## Focus On:

- **Functional Errors in Application Code:**

  - Logic errors, runtime crashes, unexpected behaviors

- **Architectural and Design Pattern Insights:**

  - MVC, MVVM, VIPER implementations and improvements

- **State Management and Data Flow Learnings:**

  - Effective use of Combine, SwiftUI state management

- **Performance Optimizations:**

  - Efficient memory usage, concurrency handling, reducing load times

- **User Experience Improvements:**

  - Enhancing UI/UX with SwiftUI or UIKit best practices

- **API Integration and Data Handling:**
  - Robust networking, data parsing, and error handling with Codable

---

## Exclusions Detailed:

- **CI/CD Configuration Errors:**

  - Issues with Continuous Integration/Continuous Deployment pipelines specific to Swift projects

- **Linting and Code Style Issues:**

  - Problems related to SwiftLint configurations or code formatting tools

- **Dependency Configuration Problems:**

  - Misconfigurations in dependency managers like CocoaPods, Carthage, or Swift Package Manager

- **Build Tool Configuration:**

  - Errors related to tools like Fastlane or custom build scripts

- **Environment Setup Issues:**
  - Problems with environment variables or setup scripts specific to Swift development

---

## Implementation Notes:

- **Precision and Relevance:**

  - Prioritize accurate and pertinent information over human readability to ensure the AI optimization process is effective.

- **Direct Editing:**
  - Always edit `.ai/codex/codex.md` directly to maintain consistency and integrity of the documentation.

---

## Example Entry:

### **ID: 102**

**Context:** `/path/from/root/Sources/Networking/APIClient.swift`

**Error/Insight:** Improper handling of optional values leading to potential runtime crashes.

**Correction/Application:** Implement optional binding using `guard let` to safely unwrap optionals.

**Prevention/Impact:** Utilize `guard` statements to handle optionals early, reducing the risk of unexpected nil values causing crashes.

**Related:** [#101](#101), [#99](#99)
