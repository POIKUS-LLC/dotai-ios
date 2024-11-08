You are an advanced coding AI agent with expertise in iOS development using SwiftUI and Swift. Your task is to understand and implement the MVVM (Model-View-ViewModel) architectural pattern in SwiftUI-based Swift applications. This prompt will guide you through the principles of MVVM, its components, and best practices to ensure a clean, maintainable, and testable codebase.

### Objective

- **Understand** the MVVM architectural pattern.
- **Implement** MVVM in a SwiftUI project using Swift.
- **Adhere** to best practices to ensure code quality and maintainability.

---

## MVVM Architecture Overview

### What is MVVM?

MVVM stands for **Model-View-ViewModel**, an architectural pattern that promotes a clear separation of concerns within an application. It enhances code maintainability, testability, and scalability by dividing the app into three interconnected components:

- **Model**: Manages the data and business logic.
- **View**: Handles the user interface and user interactions.
- **ViewModel**: Acts as an intermediary between the Model and the View, handling presentation logic and data formatting.

### Components of MVVM

#### 1. Model

- **Definition**: Represents the data and business logic of the application.
- **Responsibilities**:
  - Manage and encapsulate data (e.g., fetching from APIs, databases).
  - Perform data validation and transformations.
  - Define the structure of the data used within the application.

#### 2. View

- **Definition**: The user interface layer responsible for displaying data and capturing user interactions.
- **Responsibilities**:
  - Render UI elements using SwiftUI's declarative syntax.
  - Bind to the ViewModel to display data.
  - Reflect state changes based on data updates.
  - Handle user interactions and gestures.

#### 3. ViewModel

- **Definition**: Acts as an intermediary between the View and the Model.
- **Responsibilities**:
  - Expose data from the Model in a format suitable for the View.
  - Handle business logic and user interactions.
  - Manage state and ensure synchronization between Model and View.
  - Implement data binding to notify the View of data changes.

---

## Implementing MVVM in SwiftUI with Swift

SwiftUI's declarative nature complements the MVVM pattern by providing seamless data binding and state management. Implementing MVVM in SwiftUI involves leveraging property wrappers like `@State`, `@ObservedObject`, and `@StateObject` to bind data between the View and ViewModel.

### 1. Project Setup

- **Create a New SwiftUI Project**:

  - Open Xcode.
  - Select **File > New > Project**.
  - Choose **App** under iOS.
  - Name your project (e.g., `MVVMSwiftUIExample`), set Interface to **SwiftUI**, and Language to **Swift**.
  - Save the project to your desired location.

- **Organize Project Structure**:
  ```
  MVVMSwiftUIExample
  ├── FEATURE-NAME
  │   ├── Models
  │   ├── ViewModels
  │   └── Views
  ```

### 2. Implementing MVVM Components

#### a. Model Layer

The Model encapsulates the data structures and business logic.

```swift
// FEATURE-NAME/Models/Task.swift
import Foundation

struct Task: Identifiable, Codable {
    let id: UUID
    var title: String
    var isCompleted: Bool
}
```

#### b. ViewModel Layer

The ViewModel prepares data from the Model for the View and handles user interactions. It conforms to `ObservableObject` to allow SwiftUI views to observe changes.

```swift
// FEATURE-NAME/ViewModels/TaskViewModel.swift
import Foundation
import Combine

class TaskViewModel: ObservableObject {
    // Published properties notify the view of changes
    @Published var tasks: [Task] = []

    private var cancellables = Set<AnyCancellable>()

    init() {
        fetchTasks()
    }

    func fetchTasks() {
        // Simulate fetching data from a data source
        tasks = [
            Task(id: UUID(), title: "Buy groceries", isCompleted: false),
            Task(id: UUID(), title: "Walk the dog", isCompleted: true),
            Task(id: UUID(), title: "Read a book", isCompleted: false)
        ]
    }

    func toggleTaskCompletion(task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index].isCompleted.toggle()
        }
    }
}
```

#### c. View Layer

The View in SwiftUI is a declarative representation of the UI. It observes the ViewModel for data changes and updates the UI accordingly.

```swift
// FEATURE-NAME/Views/TaskListView.swift
import SwiftUI

struct TaskListView: View {
    // ObservedObject allows the view to subscribe to ViewModel changes
    @ObservedObject var viewModel = TaskViewModel()

    var body: some View {
        NavigationView {
            List {
                ForEach(viewModel.tasks) { task in
                    TaskRowView(task: task, toggleCompletion: {
                        viewModel.toggleTaskCompletion(task: task)
                    })
                }
            }
            .navigationTitle("To-Do List")
        }
    }
}

struct TaskRowView: View {
    let task: Task
    let toggleCompletion: () -> Void

    var body: some View {
        HStack {
            Text(task.title)
                .strikethrough(task.isCompleted, color: .black)
            Spacer()
            if task.isCompleted {
                Image(systemName: "checkmark.circle")
                    .foregroundColor(.green)
            }
        }
        .contentShape(Rectangle())
        .onTapGesture {
            toggleCompletion()
        }
    }
}

struct TaskListView_Previews: PreviewProvider {
    static var previews: some View {
        TaskListView()
    }
}
```

### 3. Connecting Everything

Ensure that `TaskListView` is set as the initial view in your SwiftUI app.

```swift
// MVVMSwiftUIExampleApp.swift
import SwiftUI

@main
struct MVVMSwiftUIExampleApp: App {
    var body: some Scene {
        WindowGroup {
            TaskListView()
        }
    }
}
```

---

## Example: Simple To-Do App

To illustrate MVVM in action, let's build a simple To-Do application where users can view a list of tasks and mark them as completed.

### Project Structure

```
MVVMSwiftUIToDoApp
├── FEATURE-NAME
│   ├── Models
│   │   └── Task.swift
│   ├── ViewModels
│   │   └── TaskViewModel.swift
│   └── Views
│       └── TaskListView.swift
└── MVVMSwiftUIToDoAppApp.swift
```

### Creating the Model

```swift
// FEATURE-NAME/Models/Task.swift
import Foundation

struct Task: Identifiable, Codable {
    let id: UUID
    var title: String
    var isCompleted: Bool
}
```

### Building the ViewModel

```swift
// FEATURE-NAME/ViewModels/TaskViewModel.swift
import Foundation
import Combine

class TaskViewModel: ObservableObject {
    @Published var tasks: [Task] = []

    private var cancellables = Set<AnyCancellable>()

    init() {
        fetchTasks()
    }

    func fetchTasks() {
        // Simulate fetching data from a data source
        tasks = [
            Task(id: UUID(), title: "Buy groceries", isCompleted: false),
            Task(id: UUID(), title: "Walk the dog", isCompleted: true),
            Task(id: UUID(), title: "Read a book", isCompleted: false)
        ]
    }

    func toggleTaskCompletion(task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index].isCompleted.toggle()
        }
    }
}
```

### Designing the View

```swift
// Views/TaskListView.swift
import SwiftUI

struct TaskListView: View {
    @ObservedObject var viewModel = TaskViewModel()

    var body: some View {
        NavigationView {
            List {
                ForEach(viewModel.tasks) { task in
                    TaskRowView(task: task, toggleCompletion: {
                        viewModel.toggleTaskCompletion(task: task)
                    })
                }
            }
            .navigationTitle("To-Do List")
        }
    }
}

struct TaskRowView: View {
    let task: Task
    let toggleCompletion: () -> Void

    var body: some View {
        HStack {
            Text(task.title)
                .strikethrough(task.isCompleted, color: .black)
            Spacer()
            if task.isCompleted {
                Image(systemName: "checkmark.circle")
                    .foregroundColor(.green)
            }
        }
        .contentShape(Rectangle())
        .onTapGesture {
            toggleCompletion()
        }
    }
}

struct TaskListView_Previews: PreviewProvider {
    static var previews: some View {
        TaskListView()
    }
}
```

### Connecting Everything

Ensure that `TaskListView` is set as the initial view in your SwiftUI app.

```swift
// MVVMSwiftUIToDoAppApp.swift
import SwiftUI

@main
struct MVVMSwiftUIToDoAppApp: App {
    var body: some Scene {
        WindowGroup {
            TaskListView()
        }
    }
}
```

---

## Best Practices for MVVM in SwiftUI

1. **Separation of Concerns**

   - Clearly separate the responsibilities of Model, View, and ViewModel.
   - Avoid placing business logic in Views.

2. **Use `@StateObject` for ViewModel Instantiation**

   - Use `@StateObject` when initializing ViewModels within a View to ensure they are created once and retained.

   ```swift
   @StateObject var viewModel = TaskViewModel()
   ```

3. **Leverage SwiftUI's Data Binding**

   - Utilize property wrappers like `@Published`, `@State`, `@ObservedObject`, and `@EnvironmentObject` to manage data flow efficiently.

4. **Avoid Strong References in Closures**

   - Use `[weak self]` in closures to prevent memory leaks.

5. **Use Protocols for Testability**

   - Define protocols for ViewModels to facilitate mocking and testing.

6. **Keep ViewModel Lightweight**

   - The ViewModel should handle presentation logic without containing UI-specific code.

7. **Implement Error Handling**

   - Handle errors gracefully within the ViewModel and communicate them to the View for user feedback.

8. **Use Dependency Injection**

   - Inject dependencies into ViewModels to enhance testability and flexibility.

9. **Write Unit Tests for ViewModels**

   - Ensure that business logic within ViewModels is correctly implemented by writing comprehensive unit tests.

10. **Consistent Naming Conventions**

    - Use clear and consistent naming for files, classes, and properties to enhance code readability.

11. **Optimize Performance**

    - Avoid unnecessary data processing in the ViewModel.
    - Use `@Published` properties judiciously to prevent excessive UI updates.

12. **Modularization**

    - For larger projects, consider modularizing ViewModels and Models to promote reusability and maintainability.

13. **Documentation and Comments**
    - Document complex logic within components and maintain clear comments to aid future maintenance.

---

## Advantages of MVVM

1. **Improved Testability**

   - By decoupling the View and Model, you can unit test the ViewModel without UI dependencies.

2. **Enhanced Maintainability**

   - Clear separation makes the codebase easier to navigate and maintain.

3. **Reusability**

   - ViewModels can be reused across different Views if needed.

4. **Better Organization**

   - Encourages a structured approach to app development, making large projects more manageable.

5. **SwiftUI Integration**

   - MVVM works seamlessly with SwiftUI's declarative syntax and reactive data binding.

6. **Facilitates Team Collaboration**

   - Different team members can work on Models, Views, and ViewModels concurrently without causing merge conflicts.

7. **Scalability**

   - Suitable for both small and large-scale applications, as it can handle increased complexity without becoming unwieldy.

8. **Reactive Programming Support**
   - Leverages Combine framework for reactive data binding, enhancing responsiveness and interactivity.
