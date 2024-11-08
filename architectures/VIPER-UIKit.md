You are an advanced coding AI agent with expertise in iOS development using UIKit and Swift. Your task is to understand and implement the VIPER (View, Interactor, Presenter, Entity, Router) architectural pattern in UIKit-based Swift applications. This prompt will guide you through the principles of VIPER, its components, and best practices to ensure a clean, maintainable, and testable codebase.

### Objective

- **Understand** the VIPER architectural pattern.
- **Implement** VIPER in a UIKit project using Swift.
- **Adhere** to best practices to ensure code quality and maintainability.

---

## VIPER Architecture Overview

### What is VIPER?

VIPER is an architectural pattern that emphasizes the separation of concerns, making it highly suitable for large-scale and complex iOS applications. VIPER stands for:

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
  - Render UI elements.
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

## Implementing VIPER in UIKit with Swift

### 1. Project Setup

- **Create a New UIKit Project**:

  - Open Xcode.
  - Select **File > New > Project**.
  - Choose **App** under iOS.
  - Name the project (e.g., `VIPERExample`), set Interface to **Storyboard**, and Language to **Swift**.
  - Save the project.

- **Organize Project Structure**:
  ```
  VIPERExample
  ├── Entities
  ├── Interactors
  ├── Presenters
  ├── Routers
  ├── Views
  └── Modules
  ```

### 2. Implementing VIPER Components

#### a. Entity Layer

The Entity layer defines the data models used throughout the application.

```swift
// Entities/Task.swift
import Foundation

struct Task: Codable {
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

The View is typically managed by a `UIViewController` in UIKit and is responsible for displaying data and capturing user interactions.

```swift
// Views/TaskViewController.swift
import UIKit

class TaskViewController: UIViewController {

    var presenter: TaskPresenterProtocol?
    private let tableView = UITableView()
    private var tasks: [Task] = []

    override func viewDidLoad() {
        super.viewDidLoad()
        setupUI()
        presenter?.viewDidLoad()
    }

    private func setupUI() {
        title = "To-Do List"
        view.backgroundColor = .white

        // Setup TableView
        tableView.translatesAutoresizingMaskIntoConstraints = false
        view.addSubview(tableView)

        // Constraints
        NSLayoutConstraint.activate([
            tableView.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor),
            tableView.leftAnchor.constraint(equalTo: view.leftAnchor),
            tableView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
            tableView.rightAnchor.constraint(equalTo: view.rightAnchor)
        ])

        tableView.register(UITableViewCell.self, forCellReuseIdentifier: "TaskCell")
        tableView.dataSource = self
        tableView.delegate = self
    }
}

extension TaskViewController: TaskViewProtocol {
    func displayTasks(_ tasks: [Task]) {
        self.tasks = tasks
        tableView.reloadData()
    }

    func updateTask(_ task: Task) {
        if let index = tasks.firstIndex(where: { $0.id == task.id }) {
            tasks[index] = task
            let indexPath = IndexPath(row: index, section: 0)
            tableView.reloadRows(at: [indexPath], with: .automatic)
        }
    }
}

extension TaskViewController: UITableViewDataSource, UITableViewDelegate {
    // DataSource Methods
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
         return tasks.count
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "TaskCell", for: indexPath)
        let task = tasks[indexPath.row]
        cell.textLabel?.text = task.title
        cell.accessoryType = task.isCompleted ? .checkmark : .none
        return cell
    }

    // Delegate Methods
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        let task = tasks[indexPath.row]
        presenter?.didSelectTask(task)
        tableView.deselectRow(at: indexPath, animated: true)
    }
}
```

#### e. Router Layer

The Router handles navigation and the assembly of VIPER modules.

```swift
// Routers/TaskRouter.swift
import UIKit

protocol TaskRouterProtocol: AnyObject {
    static func createModule() -> UIViewController
    // Add navigation functions if needed
}

class TaskRouter: TaskRouterProtocol {
    static func createModule() -> UIViewController {
        let view = TaskViewController()
        let presenter = TaskPresenter()
        let interactor = TaskInteractor()
        let router = TaskRouter()

        view.presenter = presenter
        presenter.view = view
        presenter.interactor = interactor
        presenter.router = router
        interactor.presenter = presenter

        return view
    }

    // Implement navigation functions here
}
```

### 3. Connecting Everything in AppDelegate

Ensure that the `TaskViewController` is set as the root view controller using the Router to create the module.

```swift
// AppDelegate.swift
import UIKit

@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {

        window = UIWindow(frame: UIScreen.main.bounds)
        let taskVC = TaskRouter.createModule()
        let navigationController = UINavigationController(rootViewController: taskVC)
        window?.rootViewController = navigationController
        window?.makeKeyAndVisible()

        return true
    }
}
```

---

## Best Practices for VIPER in UIKit

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
