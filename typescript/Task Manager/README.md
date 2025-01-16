# Task Management System in TypeScript

This project will help you apply core TypeScript concepts by building a task management system. You’ll practice using TypeScript features like interfaces, enums, classes, and generics while also learning professional workflows with **pnpm**, GitHub, and **Conventional Commits**.

---

## **Why Use pnpm with Homebrew?**

We’ll use **pnpm** as our package manager because it’s faster, uses less disk space, and enforces strict dependency resolution. By installing pnpm through **Homebrew**, we ensure a system-wide setup that’s simple and effective.

---

### **Step 1: Install pnpm via Homebrew**

1. If Homebrew is not installed, install it by running:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. Install pnpm using Homebrew:
   ```bash
   brew install pnpm
   ```

3. Verify the installation:
   ```bash
   pnpm --version
   ```

---

### **Step 2: Set Up Your Project**

1. **Create a New Folder**  
   Create a folder for your project and navigate into it:
   ```bash
   mkdir task-manager && cd task-manager
   ```

2. **Initialize the Project**  
   Create a `package.json` file by running:
   ```bash
   pnpm init
   ```

3. **Install TypeScript**  
   Install TypeScript and ts-node as dev dependencies:
   ```bash
   pnpm add -D typescript ts-node
   ```

4. **Set Up TypeScript**  
   Initialize TypeScript configuration:
   ```bash
   pnpm exec tsc --init
   ```

   Update your `tsconfig.json` file:
   - Enable `"strict": true`.
   - Set `"target": "ES6"`.
   - Set `"module": "commonjs"`.

5. **Create a `.gitignore` File**  
   Add the following to `.gitignore` to exclude unnecessary files:
   ```
   node_modules/
   dist/
   *.log
   ```

---

### **Step 3: Design Your Data Models**

1. **Define an Enum for Task Priorities**  
   Use an `enum` to define priority levels:
   ```typescript
   enum Priority {
       Low = "Low",
       Medium = "Medium",
       High = "High"
   }
   ```

2. **Define a Type for Task Statuses**  
   Use a union type for task statuses:
   ```typescript
   type TaskStatus = "Pending" | "In Progress" | "Completed";
   ```

3. **Create an Interface for Tasks**  
   Define an interface for the structure of a task:
   ```typescript
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

### **Step 4: Create the TaskManager Class**

Write a `TaskManager` class to manage the tasks:
1. Create a private `tasks` array to store all tasks.
2. Add methods:
   - `addTask(task: Task): void` — Adds a new task.
   - `updateTask(id: number, updates: Partial<Task>): void` — Updates an existing task.
   - `deleteTask(id: number): void` — Deletes a task by its ID.
   - `getTaskById(id: number): Task | undefined` — Fetches a task by ID.
   - `getTasksByStatus(status: TaskStatus): Task[]` — Fetches all tasks with a specific status.

---

### **Step 5: Use Generics for Filtering**

1. **Write a Generic Filter Function**  
   Use generics to create a reusable filter function:
   ```typescript
   function filterByProperty<T, K extends keyof T>(items: T[], property: K, value: T[K]): T[] {
       return items.filter(item => item[property] === value);
   }
   ```

2. **Integrate with TaskManager**  
   Use this function to filter tasks by properties like `priority` or `status`.

---

### **Step 6: Save and Load Tasks**

1. **Why This Step is Important**  
   - **Data Persistence**: Ensures tasks persist even after restarting the application.
   - **Real-world Simulation**: Mimics how most apps store and retrieve data.
   - **Serialization and Deserialization**: Helps you understand how to handle structured data.

2. **Save Tasks to Local Storage**  
   Serialize tasks to a JSON string and store them in `localStorage`:
   ```typescript
   saveToLocalStorage(): void {
       const serializedTasks = JSON.stringify(this.tasks);
       localStorage.setItem("tasks", serializedTasks);
   }
   ```

3. **Load Tasks from Local Storage**  
   Retrieve the JSON string from `localStorage` and restore it to the `tasks` array:
   ```typescript
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
   ```

4. **Integrate with TaskManager**  
   Call `saveToLocalStorage` every time a task is added, updated, or deleted.

---

### **Step 7: Testing with Jest**

1. **Install Jest**  
   ```bash
   pnpm add -D jest ts-jest @types/jest
   pnpm exec ts-jest config:init
   ```

2. **Write Test Cases**  
   Test key methods like `addTask`, `updateTask`, and `deleteTask`.

3. **Run Tests**  
   Add a test script to `package.json`:
   ```json
   "scripts": {
       "test": "jest"
   }
   ```
   Run the tests:
   ```bash
   pnpm test
   ```

---

### **Step 8: Set Up Git and GitHub**

1. **Initialize Git**  
   ```bash
   git init
   ```

2. **Create a GitHub Repository**  
   Create a new repository on GitHub and link it:
   ```bash
   git remote add origin <your-repo-URL>
   ```

3. **Push Initial Code**  
   ```bash
   git add .
   git commit -m "feat: initialize project with TypeScript"
   git branch -M main
   git push -u origin main
   ```

---

### **Step 9: Establish a Workflow**

1. **Use Feature Branches**  
   ```bash
   git checkout -b feature/add-task
   ```

2. **Follow Conventional Commits**  
   Examples:
   - `feat: implement addTask method`
   - `fix: resolve bug in task deletion`

3. **Tag Releases**  
   Use Git tags for significant milestones:
   ```bash
   git tag -a v1.0.0 -m "Initial release"
   git push origin v1.0.0
   ```

---

### **Step 10: Automate Quality Checks**

1. **Install ESLint and Prettier**  
   ```bash
   pnpm add -D eslint prettier eslint-config-prettier eslint-plugin-prettier
   ```

2. **Set Up Husky for Pre-commit Hooks**  
   ```bash
   pnpm add -D husky
   pnpm husky install
   pnpm husky add .husky/pre-commit "pnpm lint && pnpm test"
   ```

---

### **Step 11: Reflect and Expand**

1. **Reflect**  
   - Did the generics improve reusability?
   - How well does the system handle edge cases?

2. **Expand**  
   - Add deadlines for tasks.
   - Build a frontend interface.
   - Use a database for persistent storage.

---

By completing this project, you’ll gain practical experience in TypeScript, learn how to structure code for real-world applications, and practice professional workflows. Let me know if you’d like help with any step!