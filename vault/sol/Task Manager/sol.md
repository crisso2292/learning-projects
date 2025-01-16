Here’s a complete solution for the **Task Management System in TypeScript** project. The implementation includes all features described in the project instructions.

---

### **Solution Code**

#### **1. Enums and Types**
Define enums and types for priorities, statuses, and tasks.

```typescript
enum Priority {
    Low = "Low",
    Medium = "Medium",
    High = "High"
}

type TaskStatus = "Pending" | "In Progress" | "Completed";

interface Task {
    id: number;
    title: string;
    description?: string;
    priority: Priority;
    status: TaskStatus;
    createdAt: Date;
    updatedAt: Date;
}
```

---

#### **2. TaskManager Class**
The `TaskManager` class handles core operations like adding, updating, deleting, and fetching tasks.

```typescript
class TaskManager {
    private tasks: Task[] = [];

    // Add a new task
    addTask(task: Task): void {
        this.tasks.push(task);
        this.saveToLocalStorage();
    }

    // Update an existing task
    updateTask(id: number, updates: Partial<Task>): void {
        const task = this.tasks.find(task => task.id === id);
        if (!task) {
            throw new Error(`Task with ID ${id} not found`);
        }
        Object.assign(task, updates, { updatedAt: new Date() });
        this.saveToLocalStorage();
    }

    // Delete a task by ID
    deleteTask(id: number): void {
        this.tasks = this.tasks.filter(task => task.id !== id);
        this.saveToLocalStorage();
    }

    // Fetch a task by ID
    getTaskById(id: number): Task | undefined {
        return this.tasks.find(task => task.id === id);
    }

    // Fetch tasks by status
    getTasksByStatus(status: TaskStatus): Task[] {
        return this.tasks.filter(task => task.status === status);
    }

    // Save tasks to localStorage
    saveToLocalStorage(): void {
        const serializedTasks = JSON.stringify(this.tasks);
        localStorage.setItem("tasks", serializedTasks);
    }

    // Load tasks from localStorage
    loadFromLocalStorage(): void {
        const serializedTasks = localStorage.getItem("tasks");
        if (serializedTasks) {
            this.tasks = JSON.parse(serializedTasks).map((task: Task) => ({
                ...task,
                createdAt: new Date(task.createdAt),
                updatedAt: new Date(task.updatedAt)
            }));
        }
    }
}
```

---

#### **3. Generic Filter Function**
Create a generic function to filter tasks based on any property.

```typescript
function filterByProperty<T, K extends keyof T>(items: T[], property: K, value: T[K]): T[] {
    return items.filter(item => item[property] === value);
}
```

Usage example:
```typescript
const taskManager = new TaskManager();
const highPriorityTasks = filterByProperty(taskManager.getTasksByStatus("Pending"), "priority", Priority.High);
```

---

#### **4. CLI Interface**
A simple CLI using `readline` to interact with the task manager.

```typescript
import * as readline from "readline";

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

const taskManager = new TaskManager();
taskManager.loadFromLocalStorage();

function showMenu() {
    console.log(`
        1. Add Task
        2. View Tasks
        3. Update Task
        4. Delete Task
        5. Exit
    `);

    rl.question("Choose an option: ", option => {
        switch (option) {
            case "1":
                addTaskCLI();
                break;
            case "2":
                viewTasksCLI();
                break;
            case "3":
                updateTaskCLI();
                break;
            case "4":
                deleteTaskCLI();
                break;
            case "5":
                rl.close();
                break;
            default:
                console.log("Invalid option. Please try again.");
                showMenu();
                break;
        }
    });
}

function addTaskCLI() {
    rl.question("Enter task title: ", title => {
        rl.question("Enter task description (optional): ", description => {
            rl.question("Enter task priority (Low, Medium, High): ", priorityInput => {
                const priority = priorityInput as Priority;
                if (!Object.values(Priority).includes(priority)) {
                    console.log("Invalid priority. Please try again.");
                    addTaskCLI();
                    return;
                }
                const newTask: Task = {
                    id: Date.now(),
                    title,
                    description: description || undefined,
                    priority,
                    status: "Pending",
                    createdAt: new Date(),
                    updatedAt: new Date()
                };
                taskManager.addTask(newTask);
                console.log("Task added successfully!");
                showMenu();
            });
        });
    });
}

function viewTasksCLI() {
    const tasks = taskManager.getTasksByStatus("Pending");
    console.log("Pending Tasks:");
    tasks.forEach(task => {
        console.log(`ID: ${task.id}, Title: ${task.title}, Priority: ${task.priority}`);
    });
    showMenu();
}

function updateTaskCLI() {
    rl.question("Enter the task ID to update: ", idInput => {
        const id = parseInt(idInput);
        const task = taskManager.getTaskById(id);
        if (!task) {
            console.log("Task not found.");
            showMenu();
            return;
        }
        rl.question("Enter new status (Pending, In Progress, Completed): ", status => {
            if (["Pending", "In Progress", "Completed"].includes(status)) {
                taskManager.updateTask(id, { status: status as TaskStatus });
                console.log("Task updated successfully!");
            } else {
                console.log("Invalid status. Please try again.");
            }
            showMenu();
        });
    });
}

function deleteTaskCLI() {
    rl.question("Enter the task ID to delete: ", idInput => {
        const id = parseInt(idInput);
        taskManager.deleteTask(id);
        console.log("Task deleted successfully!");
        showMenu();
    });
}

showMenu();
```

---

#### **5. Testing with Jest**
Write tests to ensure the functionality works as expected.

Install Jest:
```bash
pnpm add -D jest ts-jest @types/jest
pnpm exec ts-jest config:init
```

Example test cases:
```typescript
import { TaskManager, Priority, Task } from "./TaskManager";

test("addTask should add a task to the list", () => {
    const taskManager = new TaskManager();
    const task: Task = {
        id: 1,
        title: "Test Task",
        priority: Priority.High,
        status: "Pending",
        createdAt: new Date(),
        updatedAt: new Date()
    };
    taskManager.addTask(task);
    expect(taskManager.getTaskById(1)).toEqual(task);
});

test("updateTask should update a task's status", () => {
    const taskManager = new TaskManager();
    const task: Task = {
        id: 1,
        title: "Test Task",
        priority: Priority.High,
        status: "Pending",
        createdAt: new Date(),
        updatedAt: new Date()
    };
    taskManager.addTask(task);
    taskManager.updateTask(1, { status: "Completed" });
    expect(taskManager.getTaskById(1)?.status).toBe("Completed");
});
```

---

This solution implements all the features from the project, including the core functionality, CLI interaction, and testing. Let me know if you'd like additional enhancements or explanations!