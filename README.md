# AngularTaskApp

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 13.3.6.

## Instruction

An task application made by Angular framework with angular CLI 13.3.6.

- Learned basic structure of Angular framework with Ngmodule and components.
- Created and updated components and module declarations by Angular CLI command.
- Shared data between components with using Input, Output, EventEmitter modules.
- Generated services to handle task data and adding task control features.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Configuration files

- `package.json`: The package. json file is the heart of any Node project. It records important metadata about a project which is required before publishing to NPM, and also defines functional attributes of a project that npm uses to install dependencies, run scripts, and identify the entry point to our package.
- `tsconfig.json`: A given Angular workspace contains several TypeScript configuration files. At the root tsconfig.json file specifies the base TypeScript and Angular compiler options that all projects in the workspace inherit.
- `angular.json`: A file named angular.json at the root level of an Angular workspace provides workspace-wide and project-specific configuration defaults for build and development tools provided by the Angular CLI. Path values given in the configuration are relative to the root workspace folder.

## Structure of Angular source

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/182270093-4dcf2b86-9439-4bf5-aa1c-e5a53f06523a.png">

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

## References

- https://youtu.be/3dHNOWTI7H8
- https://heynode.com/tutorial/what-packagejson/
- https://angular.io/guide/typescript-configuration
- https://angular.io/guide/workspace-config
- https://angular.io/api/core/Component
- https://angular.io/api/core/OnInit
- https://www.pluralsight.com/guides/repeating-data-with-ngfor
- https://angular.io/guide/architecture-services
