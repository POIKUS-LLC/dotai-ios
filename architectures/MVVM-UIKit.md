You are a proficient iOS developer with expertise in UIKit and Swift. Your task is to understand and implement the MVVM (Model-View-ViewModel) architectural pattern in UIKit-based Swift applications. This prompt will guide you through the principles of MVVM, its components, and best practices to ensure a clean, maintainable, and testable codebase.

### Objective

- **Understand** the MVVM architectural pattern.
- **Implement** MVVM in a UIKit project using Swift.
- **Adhere** to best practices to ensure code quality and maintainability.

### MVVM Architecture Overview

#### 1. Components of MVVM

- **Model**

  - **Definition**: Represents the data and business logic of the application.
  - **Responsibilities**:
    - Manage data (e.g., fetching from APIs, databases).
    - Perform data validation and transformations.
    - Notify the ViewModel of any data changes.

- **View**

  - **Definition**: The user interface layer that displays data and captures user interactions.
  - **Responsibilities**:
    - Render UI elements.
    - Handle user input and gestures.
    - Bind to the ViewModel to display data.

- **ViewModel**
  - **Definition**: Acts as an intermediary between the View and the Model.
  - **Responsibilities**:
    - Expose data from the Model in a format suitable for the View.
    - Handle business logic and user interactions.
    - Manage state and ensure synchronization between Model and View.

#### 2. Data Binding

- Establish a communication mechanism between the View and ViewModel.
- Options for data binding in UIKit:
  - **Closures**: Simple and lightweight.
  - **Delegates**: Traditional method using protocol-based communication.
  - **Combine/RxSwift**: Reactive frameworks for more complex scenarios.

### Implementing MVVM in UIKit with Swift

#### 1. Project Setup

- **Create a New UIKit Project**:

  - Open Xcode.
  - Select **File > New > Project**.
  - Choose **App** under iOS.
  - Name the project (e.g., `MVVMExample`), set Interface to **Storyboard**, and Language to **Swift**.
  - Save the project.

- **Organize Project Structure**:
  ```
  MVVMExample
  ├── Models
  ├── ViewModels
  ├── Views
  └── Controllers
  ```

#### 2. Implementing MVVM Components

- **Model Layer**

  ```swift
  // Models/Task.swift
  import Foundation

  struct Task: Codable {
      let id: UUID
      var title: String
      var isCompleted: Bool
  }
  ```

- **ViewModel Layer**

  ```swift
  // ViewModels/TaskViewModel.swift
  import Foundation

  class TaskViewModel {
      // Observable properties using closures
      var tasks: [Task] = [] {
          didSet {
              self.reloadTableView?()
          }
      }

      var reloadTableView: (() -> Void)?

      func fetchTasks() {
          // Simulate data fetching
          tasks = [
              Task(id: UUID(), title: "Buy groceries", isCompleted: false),
              Task(id: UUID(), title: "Walk the dog", isCompleted: true),
              Task(id: UUID(), title: "Read a book", isCompleted: false)
          ]
      }

      func toggleTaskCompletion(at index: Int) {
          guard index < tasks.count else { return }
          tasks[index].isCompleted.toggle()
          reloadTableView?()
      }
  }
  ```

- **View Layer (ViewController)**

  ```swift
  // Controllers/TaskViewController.swift
  import UIKit

  class TaskViewController: UIViewController {

      private let viewModel = TaskViewModel()
      private let tableView = UITableView()

      override func viewDidLoad() {
          super.viewDidLoad()
          setupUI()
          bindViewModel()
          viewModel.fetchTasks()
      }

      private func setupUI() {
          title = "To-Do List"
          view.backgroundColor = .white

          // Setup TableView
          tableView.translatesAutoresizingMaskIntoConstraints = false
          view.addSubview(tableView)

          // Constraints
          NSLayoutConstraint.activate([
              tableView.topAnchor.constraint(equalTo: view.topAnchor),
              tableView.leftAnchor.constraint(equalTo: view.leftAnchor),
              tableView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
              tableView.rightAnchor.constraint(equalTo: view.rightAnchor)
          ])

          tableView.register(UITableViewCell.self, forCellReuseIdentifier: "TaskCell")
          tableView.dataSource = self
          tableView.delegate = self
      }

      private func bindViewModel() {
          // Bind the reloadTableView closure to reload the table view
          viewModel.reloadTableView = { [weak self] in
              DispatchQueue.main.async {
                  self?.tableView.reloadData()
              }
          }
      }
  }

  extension TaskViewController: UITableViewDataSource, UITableViewDelegate {
      // DataSource Methods
      func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
           return viewModel.tasks.count
      }

      func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
          let cell = tableView.dequeueReusableCell(withIdentifier: "TaskCell", for: indexPath)
          let task = viewModel.tasks[indexPath.row]
          cell.textLabel?.text = task.title
          cell.accessoryType = task.isCompleted ? .checkmark : .none
          return cell
      }

      // Delegate Methods
      func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
          viewModel.toggleTaskCompletion(at: indexPath.row)
          tableView.deselectRow(at: indexPath, animated: true)
      }
  }
  ```

- **Connecting Everything in AppDelegate**

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
          let taskVC = TaskViewController()
          let navigationController = UINavigationController(rootViewController: taskVC)
          window?.rootViewController = navigationController
          window?.makeKeyAndVisible()

          return true
      }
  }
  ```

### Best Practices for MVVM in UIKit

1. **Separation of Concerns**

   - Clearly separate the responsibilities of Model, View, and ViewModel.
   - Avoid placing business logic in ViewControllers.

2. **Use Protocols for Communication**

   - Define protocols to abstract interactions between View and ViewModel.
   - Enhances testability and flexibility.

3. **Data Binding Mechanism**

   - Implement a robust data-binding mechanism.
   - Consider using reactive frameworks like Combine or RxSwift for scalability.

4. **Avoid Strong References in Closures**

   - Use `[weak self]` in closures to prevent memory leaks.

5. **Keep ViewModel Lightweight**

   - The ViewModel should handle presentation logic without containing UI-specific code.

6. **Unit Testing**

   - Write unit tests for ViewModels to ensure business logic correctness.
   - Mock Models and Services to isolate ViewModel tests.

7. **Consistent Naming Conventions**

   - Use clear and consistent naming for files and classes to enhance readability.

8. **Error Handling**

   - Implement comprehensive error handling within ViewModels.
   - Communicate errors to the View for user feedback.

9. **Scalability Considerations**

   - Design ViewModels to handle increased complexity as the app grows.
   - Modularize ViewModels where necessary.

10. **Documentation and Comments**
    - Document complex logic within ViewModels.
    - Maintain clear comments to aid future maintenance.
    - Checkout [Documenting Swift Code](../coding-instructions/documenting-swift-code.md)
