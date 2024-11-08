You are an advanced coding AI agent with expertise in iOS development using SwiftUI and Swift. Your task is to understand and implement the VIPER (View, Interactor, Presenter, Entity, Router) architectural pattern in SwiftUI-based Swift applications. This prompt will guide you through the principles of VIPER, its components, and best practices to ensure a clean, maintainable, and testable codebase.

### Objective

- **Understand** the VIPER architectural pattern.
- **Implement** VIPER in a SwiftUI project using Swift.
- **Adhere** to best practices to ensure code quality and maintainability.

---

## VIPER Architecture Overview

### What is VIPER?

VIPER is an architectural pattern that emphasizes a high level of separation of concerns, making it highly suitable for large-scale and complex iOS applications. VIPER stands for:

- **View**
- **Interactor**
- **Presenter**
- **Entity**
- **Router**

Each component has a distinct responsibility, fostering a modular, testable, and maintainable codebase.

### Components of VIPER

#### 1. View

- **Definition**: The user interface layer responsible for displaying data and capturing user interactions.
- **Responsibilities**:
  - Render UI elements using SwiftUI's declarative syntax.
  - Receive user input and forward it to the Presenter.
  - Display data provided by the Presenter.
  - Reflect state changes (e.g., loading indicators, error messages).

#### 2. Interactor

- **Definition**: Contains the business logic of the application.
- **Responsibilities**:
  - Fetch, manipulate, and process data from Entities or external services.
  - Perform data validation and transformations.
  - Handle network requests and database operations.
  - Communicate results back to the Presenter.

#### 3. Presenter

- **Definition**: Acts as an intermediary between the View and the Interactor.
- **Responsibilities**:
  - Receive input from the View and process it.
  - Request data from the Interactor.
  - Format data received from the Interactor for display.
  - Manage the flow of data between View and Interactor.

#### 4. Entity

- **Definition**: Represents the data models used by the Interactor.
- **Responsibilities**:
  - Define the structure of data used within the application.
  - Typically consists of plain Swift structs or classes.

#### 5. Router

- **Definition**: Manages navigation and routing within the application.
- **Responsibilities**:
  - Handle navigation logic (e.g., transitioning between screens).
  - Create and assemble VIPER modules.
  - Pass data between modules during navigation.

---

## Implementing VIPER in SwiftUI with Swift

Adapting VIPER for SwiftUI involves integrating its components with SwiftUI's declarative and reactive paradigms. While UIKit relies heavily on protocols and delegates, SwiftUI leverages data binding and state management, which requires slight adjustments in VIPER's implementation.

### 1. Project Setup

- **Create a New SwiftUI Project**:

  - Open Xcode.
  - Select **File > New > Project**.
  - Choose **App** under iOS.
  - Name your project (e.g., `VIPERSwiftUIExample`), set Interface to **SwiftUI**, and Language to **Swift**.
  - Save the project to your desired location.

- **Organize Project Structure**:
  ```
  VIPERSwiftUIExample
  ├── Entities
  ├── Interactors
  ├── Presenters
  ├── Routers
  ├── Views
  └── ViewModels
  ```

### 2. Implementing VIPER Components

#### a. Entity Layer

The Entity layer defines the data models used throughout the application.

```swift
// Entities/Task.swift
import Foundation

struct Task: Identifiable, Codable {
    let id: UUID
    var title: String
    var isCompleted: Bool
}
```

#### b. Interactor Layer

The Interactor contains the business logic and data manipulation.

```swift
// Interactors/TaskInteractor.swift
import Foundation

protocol TaskInteractorProtocol: AnyObject {
    func fetchTasks()
    func toggleTaskCompletion(task: Task)
}

protocol TaskInteractorOutputProtocol: AnyObject {
    func tasksFetched(_ tasks: [Task])
    func taskCompletionToggled(_ task: Task)
}

class TaskInteractor: TaskInteractorProtocol {
    weak var presenter: TaskInteractorOutputProtocol?

    private var tasks: [Task] = []

    func fetchTasks() {
        // Simulate data fetching
        tasks = [
            Task(id: UUID(), title: "Buy groceries", isCompleted: false),
            Task(id: UUID(), title: "Walk the dog", isCompleted: true),
            Task(id: UUID(), title: "Read a book", isCompleted: false)
        ]
        presenter?.tasksFetched(tasks)
    }

    func toggleTaskCompletion(task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index].isCompleted.toggle()
            presenter?.taskCompletionToggled(tasks[index])
        }
    }
}
```

#### c. Presenter Layer

The Presenter acts as the middleman between the View and the Interactor.

```swift
// Presenters/TaskPresenter.swift
import Foundation

protocol TaskPresenterProtocol: AnyObject {
    func viewDidLoad()
    func didSelectTask(_ task: Task)
}

protocol TaskViewProtocol: AnyObject {
    func displayTasks(_ tasks: [Task])
    func updateTask(_ task: Task)
}

class TaskPresenter: TaskPresenterProtocol {
    weak var view: TaskViewProtocol?
    var interactor: TaskInteractorProtocol?
    var router: TaskRouterProtocol?

    private var tasks: [Task] = []

    func viewDidLoad() {
        interactor?.fetchTasks()
    }

    func didSelectTask(_ task: Task) {
        interactor?.toggleTaskCompletion(task: task)
    }
}

extension TaskPresenter: TaskInteractorOutputProtocol {
    func tasksFetched(_ tasks: [Task]) {
        self.tasks = tasks
        view?.displayTasks(tasks)
    }

    func taskCompletionToggled(_ task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index] = task
            view?.updateTask(task)
        }
    }
}
```

#### d. View Layer

The View in SwiftUI is a declarative representation of the UI. It observes the Presenter for data changes and updates the UI accordingly.

```swift
// Views/TaskListView.swift
import SwiftUI

struct TaskListView: View {
    @ObservedObject var viewModel: TaskViewModel

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
            .onAppear {
                viewModel.presenter?.viewDidLoad()
            }
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
        let viewModel = TaskViewModel()
        TaskListView(viewModel: viewModel)
    }
}
```

**Note**: To bridge SwiftUI's declarative nature with VIPER's structure, the ViewModel in this setup will act as a bridge between SwiftUI's Views and VIPER's Presenter.

#### e. Router Layer

The Router handles navigation and the assembly of VIPER modules.

```swift
// Routers/TaskRouter.swift
import SwiftUI

protocol TaskRouterProtocol: AnyObject {
    static func createModule() -> some View
    // Add navigation functions if needed
}

class TaskRouter: TaskRouterProtocol {
    static func createModule() -> some View {
        // Initialize VIPER components
        let viewModel = TaskViewModel()
        let view = TaskListView(viewModel: viewModel)
        let presenter = TaskPresenter()
        let interactor = TaskInteractor()
        let router = TaskRouter()

        // Connect VIPER components
        presenter.view = TaskViewAdapter(view: view)
        presenter.interactor = interactor
        presenter.router = router
        interactor.presenter = presenter

        // Assign Presenter to ViewModel
        viewModel.presenter = presenter

        return view
    }

    // Implement navigation functions here if needed
}

class TaskViewAdapter: TaskViewProtocol {
    private weak var view: TaskListView?

    init(view: TaskListView) {
        self.view = view
    }

    func displayTasks(_ tasks: [Task]) {
        view?.viewModel.tasks = tasks
    }

    func updateTask(_ task: Task) {
        if let index = view?.viewModel.tasks.firstIndex(where: { $0.id == task.id }) {
            view?.viewModel.tasks[index] = task
        }
    }
}
```

**Explanation**:

- **TaskRouter**: Creates and assembles the VIPER module by initializing the View, Presenter, Interactor, and Router. It connects these components appropriately.
- **TaskViewAdapter**: Acts as an adapter to conform the SwiftUI `TaskListView` to the `TaskViewProtocol`. This is necessary because SwiftUI Views do not naturally conform to protocols expecting classes.

#### f. ViewModel Bridge

To effectively bridge SwiftUI's reactive views with VIPER's Presenter, introduce a ViewModel that conforms to `ObservableObject` and interacts with the Presenter.

```swift
// ViewModels/TaskViewModel.swift
import Foundation
import Combine

class TaskViewModel: ObservableObject {
    @Published var tasks: [Task] = []

    weak var presenter: TaskPresenterProtocol?

    private var cancellables = Set<AnyCancellable>()

    init() {
        // Presenter will be assigned by the Router
    }

    func toggleTaskCompletion(task: Task) {
        presenter?.didSelectTask(task)
    }
}
```

**Explanation**:

- **TaskViewModel**: Acts as a bridge between SwiftUI's `TaskListView` and VIPER's Presenter. It holds the published `tasks` array that SwiftUI observes and updates the UI accordingly.
- **toggleTaskCompletion**: Invokes the Presenter's method to handle task completion toggling.

---

## Example: Simple To-Do App

To illustrate VIPER in action with SwiftUI, let's build a simple To-Do application where users can view a list of tasks and mark them as completed.

### Project Structure

```
VIPERSwiftUIToDoApp
├── Entities
│   └── Task.swift
├── Interactors
│   └── TaskInteractor.swift
├── Presenters
│   └── TaskPresenter.swift
├── Routers
│   └── TaskRouter.swift
├── Views
│   └── TaskListView.swift
├── ViewModels
│   └── TaskViewModel.swift
└── VIPERSwiftUIToDoAppApp.swift
```

### Creating the Entity

```swift
// Entities/Task.swift
import Foundation

struct Task: Identifiable, Codable {
    let id: UUID
    var title: String
    var isCompleted: Bool
}
```

### Building the Interactor

```swift
// Interactors/TaskInteractor.swift
import Foundation

protocol TaskInteractorProtocol: AnyObject {
    func fetchTasks()
    func toggleTaskCompletion(task: Task)
}

protocol TaskInteractorOutputProtocol: AnyObject {
    func tasksFetched(_ tasks: [Task])
    func taskCompletionToggled(_ task: Task)
}

class TaskInteractor: TaskInteractorProtocol {
    weak var presenter: TaskInteractorOutputProtocol?

    private var tasks: [Task] = []

    func fetchTasks() {
        // Simulate data fetching
        tasks = [
            Task(id: UUID(), title: "Buy groceries", isCompleted: false),
            Task(id: UUID(), title: "Walk the dog", isCompleted: true),
            Task(id: UUID(), title: "Read a book", isCompleted: false)
        ]
        presenter?.tasksFetched(tasks)
    }

    func toggleTaskCompletion(task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index].isCompleted.toggle()
            presenter?.taskCompletionToggled(tasks[index])
        }
    }
}
```

### Designing the Presenter

```swift
// Presenters/TaskPresenter.swift
import Foundation

protocol TaskPresenterProtocol: AnyObject {
    func viewDidLoad()
    func didSelectTask(_ task: Task)
}

protocol TaskViewProtocol: AnyObject {
    func displayTasks(_ tasks: [Task])
    func updateTask(_ task: Task)
}

class TaskPresenter: TaskPresenterProtocol {
    weak var view: TaskViewProtocol?
    var interactor: TaskInteractorProtocol?
    var router: TaskRouterProtocol?

    private var tasks: [Task] = []

    func viewDidLoad() {
        interactor?.fetchTasks()
    }

    func didSelectTask(_ task: Task) {
        interactor?.toggleTaskCompletion(task: task)
    }
}

extension TaskPresenter: TaskInteractorOutputProtocol {
    func tasksFetched(_ tasks: [Task]) {
        self.tasks = tasks
        view?.displayTasks(tasks)
    }

    func taskCompletionToggled(_ task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index] = task
            view?.updateTask(task)
        }
    }
}
```

### Designing the View

```swift
// Views/TaskListView.swift
import SwiftUI

struct TaskListView: View {
    @ObservedObject var viewModel: TaskViewModel

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
            .onAppear {
                viewModel.presenter?.viewDidLoad()
            }
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
        let viewModel = TaskViewModel()
        TaskListView(viewModel: viewModel)
    }
}
```

### Implementing the Router

```swift
// Routers/TaskRouter.swift
import SwiftUI

protocol TaskRouterProtocol: AnyObject {
    static func createModule() -> some View
    // Add navigation functions if needed
}

class TaskRouter: TaskRouterProtocol {
    static func createModule() -> some View {
        // Initialize VIPER components
        let viewModel = TaskViewModel()
        let view = TaskListView(viewModel: viewModel)
        let presenter = TaskPresenter()
        let interactor = TaskInteractor()
        let router = TaskRouter()

        // Connect VIPER components
        presenter.view = TaskViewAdapter(view: view)
        presenter.interactor = interactor
        presenter.router = router
        interactor.presenter = presenter

        // Assign Presenter to ViewModel
        viewModel.presenter = presenter

        return view
    }

    // Implement navigation functions here if needed
}

class TaskViewAdapter: TaskViewProtocol {
    private weak var view: TaskListView?

    init(view: TaskListView) {
        self.view = view
    }

    func displayTasks(_ tasks: [Task]) {
        view?.viewModel.tasks = tasks
    }

    func updateTask(_ task: Task) {
        if let index = view?.viewModel.tasks.firstIndex(where: { $0.id == task.id }) {
            view?.viewModel.tasks[index] = task
        }
    }
}
```

### Connecting Everything

Ensure that the `TaskListView` is set as the initial view in your SwiftUI app using the Router to create the module.

```swift
// VIPERSwiftUIToDoAppApp.swift
import SwiftUI

@main
struct VIPERSwiftUIToDoAppApp: App {
    var body: some Scene {
        WindowGroup {
            TaskRouter.createModule()
        }
    }
}
```

---

## Best Practices for VIPER in SwiftUI

1. **Single Responsibility Principle**

   - Each VIPER component should have a single responsibility. Avoid mixing concerns across components.

2. **Strong Separation of Concerns**

   - Maintain clear boundaries between View, Interactor, Presenter, Entity, and Router to enhance code clarity and maintainability.

3. **Use Protocols for Communication**

   - Define protocols to abstract interactions between VIPER components, enhancing testability and flexibility.

4. **Avoid Strong References in Delegates**

   - Use `weak` references for delegates to prevent retain cycles and memory leaks.

5. **Consistent Naming Conventions**

   - Use clear and consistent naming for files, classes, and protocols to enhance code readability.

6. **Modularization**

   - For larger projects, consider modularizing VIPER components to promote reusability and maintainability.

7. **Error Handling**

   - Implement comprehensive error handling within Interactors and communicate errors to the Presenter for user feedback.

8. **Unit Testing**

   - Write unit tests for Presenters and Interactors to ensure business logic correctness without UI dependencies.

9. **Avoid Business Logic in Views**

   - Keep Views as passive as possible, delegating all business logic to Presenters and Interactors.

10. **Documentation and Comments**

    - Document complex logic within components and maintain clear comments to aid future maintenance.

11. **Leverage SwiftUI’s Data Binding**

    - Utilize SwiftUI’s property wrappers (`@State`, `@ObservedObject`, `@StateObject`) to manage data flow efficiently between Views and ViewModels.

12. **Use Adapters for Protocol Conformance**

    - Implement adapters (like `TaskViewAdapter`) to bridge SwiftUI Views with VIPER protocols, ensuring smooth communication between components.

13. **Optimize Performance**

    - Avoid unnecessary data processing in the Presenter and Interactor.
    - Use `@Published` properties judiciously to prevent excessive UI updates.

14. **Use Dependency Injection**

    - Inject dependencies into VIPER components to enhance testability and flexibility.

15. **Consistent UI Updates**
    - Ensure that the View reflects the latest state from the ViewModel to maintain UI consistency.

---

## Advantages of VIPER

1. **Enhanced Testability**

   - Clear separation allows for isolated unit testing of each component, especially Presenters and Interactors.

2. **Improved Maintainability**

   - Modular structure makes it easier to manage and update code, especially in large projects.

3. **Scalability**

   - Suitable for complex applications, as it can handle increased complexity without becoming unwieldy.

4. **Reusability**

   - Components, especially Routers and Presenters, can be reused across different modules.

5. **Team Collaboration**

   - Different team members can work on separate VIPER components concurrently, reducing merge conflicts and improving workflow.

6. **Clear Structure**

   - Provides a well-defined architecture, making it easier for new developers to understand and contribute to the project.

7. **Separation of Concerns**

   - Each component handles its specific responsibility, reducing code coupling and enhancing clarity.

8. **SwiftUI Integration**
   - When adapted correctly, VIPER can leverage SwiftUI's declarative syntax and reactive data binding for robust UI management.
