# AngularTaskApp

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 13.3.6.

## Instruction

An task application made by Angular framework with angular CLI 13.3.6.

- Learned basic structure of Angular framework with Ngmodule and components.
- Created and updated components and module declarations by Angular CLI command.
- Shared data between components with using Input, Output, EventEmitter modules.
- Generated services to handle task data and adding task control features.
- Added toggle feature with Observable and Subscribe module.
- Used RouteModule and Route to control routing between components.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Configuration files

- `package.json`: The package. json file is the heart of any Node project. It records important metadata about a project which is required before publishing to NPM, and also defines functional attributes of a project that npm uses to install dependencies, run scripts, and identify the entry point to our package.
- `tsconfig.json`: A given Angular workspace contains several TypeScript configuration files. At the root tsconfig.json file specifies the base TypeScript and Angular compiler options that all projects in the workspace inherit.
- `angular.json`: A file named angular.json at the root level of an Angular workspace provides workspace-wide and project-specific configuration defaults for build and development tools provided by the Angular CLI. Path values given in the configuration are relative to the root workspace folder.

## Structure of Angular source

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/182270093-4dcf2b86-9439-4bf5-aa1c-e5a53f06523a.png">

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/185768279-295acbb1-3d1f-4744-a146-b70de1d5040d.png">

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/185773453-c054fbc1-840a-4269-a9ac-7e0d58a0907f.png">


- `NgModule`: @NgModule takes a metadata object that describes how to compile a component's template and how to create an injector at runtime. It identifies the module's own components, directives, and pipes, making some of them public, through the exports property, so that external components can use them.

```typescript
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

- `Component`: Angular components are a subset of directives, always associated with a template. Unlike other directives, only one component can be instantiated for a given element in a template. A component must belong to an NgModule in order for it to be available to another component or application.

## Global style code

To implement global styles on this project, we can change `src/styles.css`.

## Make a new component

Now, let's make a new component with angularCLI. If you want to make `header`component, `ng generate component components/header` command would start adding new component.

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/182270509-2ef1207a-aa66-4412-aec4-af986752fc9f.png">

As you can see, AngularCLI automatically create css, html, and typescript file of header.component. And also, it add `HeaderComponent` inside of `app.module.ts` file `declarations` part like below.

```typescript
@NgModule({
  declarations: [AppComponent, HeaderComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

`header.component.ts` file has Component annotation and OnInit interface like below.

```typescript
import { Component, OnInit } from "@angular/core";

@Component({
  selector: "app-header",
  templateUrl: "./header.component.html",
  styleUrls: ["./header.component.css"],
})
export class HeaderComponent implements OnInit {
  constructor() {}

  ngOnInit(): void {}
}
```

- `OnInit`: A lifecycle hook that is called after Angular has initialized all data-bound properties of a directive. Define an ngOnInit() method to handle any additional initialization tasks such as HTTP request. interface OnInit { ngOnInit(): void }

If you want to make specific style setup, you need to add some code on the `header.component.css`.

## Embed button component inside of header component

For now, we gonna make another angular component by using angularCLI. Enter the command `ng generate component components/button`. The button component should be embedded inside of header component. For doing this, you can add `<app-button></app-button>` tag on the `header.component.html`.

```html
<header>
  <h1>{{ title }}</h1>
  <app-button></app-button>
</header>
```

And then, we can see 'button works!' text is added on the localhost page. After changing this tag into <button>Click</button>, there is a button with text click.

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/183690769-62889647-ee12-43c8-adf9-74858f8d9a31.png">

But button component `color` and `text` should be changed by diffenrent context. For doing this, you can use `@Input` and `[ngStyle]`.

```typescript
import { Component, OnInit, Input } from '@angular/core';
...
export class ButtonComponent implements OnInit {
  @Input() text: string;
  @Input() color: string;

  constructor() {
    this.text = '';
    this.color = '';
  }

  ngOnInit(): void {}
}
```

As you can see, `Input` make variable that can be used inside of html with {{}}. And `[ngStyle]` can controll CSS style directly with `color` variable.

```html
<button [ngStyle]="{ 'background-color': color }" class="btn">
  {{ text }}
</button>
```

Now you can change color and text of button component on the `header.component.html` with attribute.

```html
<header>
  <h1>{{ title }}</h1>
  <app-button color="green" text="Add"></app-button>
</header>
```

## Customize function

Depending on purpose of the button component, you probably want to customize button feature. In order to do this, we should use `Output` and `EventEmitter`.

### 1. Make onClick() for button click.

```html
<button
  [ngStyle]="{ 'background-color': color }"
  class="btn"
  (click)="onClick()"
>
  {{ text }}
</button>
```

### 2. Import Output and EventEmitter

In this project, we are making header component with button component. If other component want to use same button component, each button click would make different functionalities. For doing this, you need to add customized function and import several modules.

- Output: @Output decorator is used to pass the data from child to parent component. @Output decorator binds a property of a component, to send data from one component to the calling component. @Output binds a property of the type of angular EventEmitter class.
- EventEmitter: EventEmitter is a module that helps share data between components using emit() and subscribe() methods. EventEmitter is in the Observables layer, which observes changes and values and emits the data to the components subscribed to that EventEmitter instance.

First of all, add a `(click)` attribute on the `<button>` tag.

```html
<button
  [ngStyle]="{ 'background-color': color }"
  class="btn"
  (click)="onClick()"
>
  {{ text }}
</button>
```

After than, make an `onClick()` methon on the `button.component.ts` with using `@Output` and `EventEmitter`.

```typescript
import { Component, OnInit, Input, Output, EventEmitter } from "@angular/core";

@Component({
  selector: "app-button",
  templateUrl: "./button.component.html",
  styleUrls: ["./button.component.css"],
})
export class ButtonComponent implements OnInit {
  @Input() text: string;
  @Input() color: string;
  @Output() btnClick = new EventEmitter();

  constructor() {
    this.text = "";
    this.color = "";
  }

  ngOnInit(): void {}

  onClick() {
    this.btnClick.emit();
  }
}
```

`btnClick` method is for sharing data between button and header component. Inside of `header.component.html`, add a ``(btnClick)="toggleAddTask()` attribute to get information whether button is clicked or not.

```html
<header>
  <h1>{{ title }}</h1>
  <app-button
    color="green"
    text="Add"
    (btnClick)="toggleAddTask()"
  ></app-button>
</header>
```

And define `toggleAddTask()` function on the `header.component.ts`.

```typescript
import { Component, OnInit } from "@angular/core";

@Component({
  selector: "app-header",
  templateUrl: "./header.component.html",
  styleUrls: ["./header.component.css"],
})
export class HeaderComponent implements OnInit {
  title: string = "Task Tracker";
  constructor() {}

  ngOnInit(): void {}

  toggleAddTask() {
    console.log("toggle");
  }
}
```

When you click the button, you can see 'toggle' on the console window like below.

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/183738259-ee6df4aa-3dc5-4d36-aeec-fdd32a79f2c1.png">

### 3. Task data json

These data below is for test UI in this project. `Task.ts` exports `Task` as an interface. `mock-tasks.ts` imports this `Task` and exports list of JSON format as `TASKS`.

Task.ts

```typescript
export interface Task {
  id?: number;
  text: string;
  day: string;
  reminder: boolean;
}
```

mock-tasks.ts

```typescript
import { Task } from "./Task";

export const TASKS: Task[] = [
  {
    id: 1,
    text: "Doctors Appointment",
    day: "May 5th at 2:30pm",
    reminder: true,
  },
  {
    id: 2,
    text: "Meeting at School",
    day: "May 6th at 1:30pm",
    reminder: true,
  },
  {
    id: 3,
    text: "Food Shopping",
    day: "May 7th at 12:30pm",
    reminder: false,
  },
];
```

## Create task component

Using angularCLI, make task component. Import `Task` and `TASKS` from data files, and show `tasks.text` on the `tasks.component.html`.

```html
<p *ngFor="let task of tasks">{{ task.text }}</p>
```

- *ngFor: *ngFor is a predefined directive in Angular. It accepts an array to iterate data over atemplate to replicate the template with different data. It's the same as the forEach() method in JavaScript, which also iterates over an array.

## Angular font awesome

If we need to use font awesome, you can use angularCLI command `ng add @fortawesome/angular-fontawesome@<version>`. You should check version compatibilities between angular and [angular font awesome](https://github.com/FortAwesome/angular-fontawesome) module.

Now we can use font awesome icons with adding attribute on the specific html tag.

- import specific modules from @fortawesome module

```typescript
import { Component, OnInit, Input } from "@angular/core";
import { Task } from "src/app/Task";
import { faTimes } from "@fortawesome/free-solid-svg-icons";

@Component({
  selector: "app-task-item",
  templateUrl: "./task-item.component.html",
  styleUrls: ["./task-item.component.css"],
})
export class TaskItemComponent implements OnInit {
  @Input() task!: Task;
  faTimes = faTimes;

  constructor() {}

  ngOnInit(): void {}
}
```

- Add attribute and change style with [ngStyle].

```typescript
<div class="task">
  <h3>
    {{ task.text }}
    <fa-icon [ngStyle]="{ color: 'red' }" [icon]="faTimes"></fa-icon>
  </h3>
  <p>{{ task.day }}</p>
</div>
```

## Generate services

What is the `service` on the Angular?

Service is a broad category encompassing any value, function, or feature that an application needs. A service is typically a class with a narrow, well-defined purpose. It should do something specific and do it well. Angular distinguishes components from services to increase modularity and reusability.

task.service.ts

```typescript
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class TaskService {
  constructor() {}
}
```

- Injectable: The @Injectable() decorator defines a class as a service in Angular and allows Angular to inject it into a component as a dependency. Likewise, the @Injectable() decorator indicates that a component, class, pipe, or NgModule has a dependency on a service. The injector is the main mechanism.

To control task data inside of `task.service.ts`, we can use `Observable` and `of` after import `Task` and `TASKS`.

- Objervable: Observables provide support for passing messages between parts of your application. They are used frequently in Angular and are a technique for event handling, asynchronous programming, and handling multiple values.

task.service.ts

```typescript
import { Injectable } from "@angular/core";
import { Observable, of } from "rxjs";
import { Task } from "src/app/Task";
import { TASKS } from "src/app/mock-tasks";

@Injectable({
  providedIn: "root",
})
export class TaskService {
  constructor() {}

  getTasks(): Observable<Task[]> {
    const tasks = of(TASKS);
    return tasks;
  }
}
```

task.component.ts

```typescript
import { Component, OnInit } from "@angular/core";
import { Task } from "src/app/Task";
import { TaskService } from "src/app/services/task.service";
@Component({
  selector: "app-tasks",
  templateUrl: "./tasks.component.html",
  styleUrls: ["./tasks.component.css"],
})
export class TasksComponent implements OnInit {
  tasks: Task[] = [];

  constructor(private taskService: TaskService) {}

  ngOnInit(): void {
    this.taskService.getTasks().subscribe((tasks) => (this.tasks = tasks));
  }
}
```

## JSON server

[JSON server](https://www.npmjs.com/package/json-server) is a full fake REST API with zero coding in less than 30 seconds.

Install: `npm i json-server`
Add scripts: `package.json > "scripts" > "server": "json-server --watch db.json --port 5000"`

Add `db.json` file on the root position of this project and enter this command`npm run server`. And then, you can find fake database on `localhost:5000/tasks`.

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/184803134-ebbc2b38-84f6-4fd0-99f3-9160502f00f6.png">

## Connect with backend

In order to connect backend with angular project, we need several modules from `@angular/common/http`.

- HttpClientModule: The HttpClientModule is a service module provided by Angular that allows us to perform HTTP requests and easily manipulate those requests and their responses. It is called a service module because it only instantiates services and does not export any components, directives or pipes.

- HttpClient: Performs HTTP requests. This service is available as an injectable class, with methods to perform HTTP requests. Each request method has multiple signatures, and the return type varies based on the signature that is called (mainly the values of observe and responseType).

- HttpHeaders: Represents the header configuration options for an HTTP request. Instances are immutable. Modifying methods return a cloned instance with the change. The original object is never changed.

And `TaskService` class would be changed like below.

```typescript
export class TaskService {
  private apiUrl = "http://localhost:5000/tasks";
  constructor(private http: HttpClient) {}

  getTasks(): Observable<Task[]> {
    return this.http.get<Task[]>(this.apiUrl);
  }
}
```

## Delete task feature

If we click `x` icon, that specific task should be deleted. In order to do this feature, you need to add delete method not only `task-item component` but also `task.service`.

### 1. onDelete(task)

On the task-item component, `onDelete` method should be created.

task-item.component.html

```html
<fa-icon
  (click)="onDelete(task)"
  [ngStyle]="{ color: 'red' }"
  [icon]="faTimes"
></fa-icon>
```

task-item.component.ts

```typescript
  // Method for click event
  onDelete(task: Task) {
    this.onDeleteTask.emit(task);
  }
```

### 2. Output for task component

Task component need to figure out delete click event from task-item component. To do this, we can add @Output on the task-item.component.ts.

task-item.component.ts

```typescript
  @Output() onDeleteTask: EventEmitter<Task> = new EventEmitter();
```

And task component take this `onDeleteTask` method on the task.component.html.

task.component.html

```html
<app-task-item
  *ngFor="let task of tasks"
  [task]="task"
  (onDeleteTask)="deleteTask(task)"
></app-task-item>
```

### 3. deleteTask(task) method between service and task component.

Now, you need make `deleteTask` method on the task.component.ts and define on service.

task.component.ts

```typescript
  // Take deletaTask function from service
  // Delete onDelete task and subscribe which is filtered
  deleteTask(task: Task) {
    this.taskService
      .deleteTask(task)
      .subscribe(
        () => (this.tasks = this.tasks.filter((t) => t.id !== task.id))
      );
  }
```

task.service.ts

```typescript
  // Function to delete task -> tasks.component.ts
  deleteTask(task: Task): Observable<Task> {
    const url = `${this.apiUrl}/${task.id}`;
    return this.http.delete<Task>(url);
  }
```

## FormsModule

- FormsModule: Exports the required providers and directives for template-driven forms, making them available for import by NgModules that import this module. ReactiveFormsModule. Exports the required infrastructure and directives for reactive forms, making them available for import by NgModules that import this module.

### Data binding with html tag

add-task.component.html

```html
<input
  type="text"
  name="text"
  [(ngModel)]="text"
  id="text"
  placeholder="Add Task"
/>
```

We can use `[(ngModel)]=""` for data binding. Each of the input tag has their own `ngModel` attribute to bind data.

## Button toggle when user clicked

Now, you probably want to make button text and color changable when user clicked. In order to do this, we need to make another service. `ng generate service services/ui`

Then we will make two private variable and functions.

ui.service.ts

```typescript
export class UiService {
  private showAddTask: boolean = false;
  private subject = new Subject<any>();

  constructor() {}

  toggleAddTask(): void {
    this.showAddTask = !this.showAddTask;
    this.subject.next(this.showAddTask);
  }

  onToggle(): Observable<any> {
    return this.subject.asObservable();
  }
}
```

On the header component, import `UiService` and subscribe after making `uiService` object by using constructor.

header.component.ts

```typescript
export class HeaderComponent implements OnInit {
  title: string = "Task Tracker";
  // To add toggle feature
  showAddTask: boolean = true;
  subscription!: Subscription;

  constructor(private uiService: UiService) {
    this.subscription = this.uiService
      .onToggle()
      .subscribe((value) => (this.showAddTask = value));
  }

  ngOnInit(): void {}

  toggleAddTask() {
    this.uiService.toggleAddTask();
  }
}
```

And you can change color and text of the button by using attribute below.

header.component.html

```html
<app-button
  color="{{ showAddTask ? 'red' : 'green' }}"
  text="{{ showAddTask ? 'Close' : 'Add' }}"
  (btnClick)="toggleAddTask()"
></app-button>
```

## Showing add-task form with toggle button

Finally, we will add showing add-task form feature with toggle button. To do this, you need to use `UiService`, `Subscription` on the `add-task` component such as the header component. We also make variables `showAddTask`, `subscription` and `UiService` object. After this, `*ngIf="showAddTask"` should be added on the `form` tag of add-task component.

```html
<form *ngIf="showAddTask" class="add-form" (ngSubmit)="onSubmit()"></form>
```

## Angular Router feature

In order to fully use Angular framework, `RouteModule` and `Route` is mandatory. With route controll, you can handle components with url such as `/about`. For using this, there are several steps.

### 1. Import route modules and make route list

app.module.ts

```typescript
import { RouterModule, Routes } from '@angular/router';
...
// Make Route list
const appRoutes: Routes = [
  { path: '', component: TasksComponent },
  { path: 'about', component: AboutComponent },
];
```

### 2. Use routerLink on component html

To use router feature, `routerLink` should be added on the specific html tag with targeted url.

```html
<div>
  <h2>Task App</h2>
  <h4>Version: 1.0.0</h4>
  <a routerLink="/">Go Back</a>
</div>
```

## References

- https://youtu.be/3dHNOWTI7H8
- https://heynode.com/tutorial/what-packagejson/
- https://angular.io/guide/typescript-configuration
- https://angular.io/guide/workspace-config
- https://angular.io/api/core/Component
- https://angular.io/api/core/OnInit
- https://www.pluralsight.com/guides/repeating-data-with-ngfor
- https://angular.io/guide/architecture-services
- https://indepth.dev/posts/1142/exploring-the-httpclientmodule-in-angular
- https://angular.io/api/common/http/HttpClient#description
