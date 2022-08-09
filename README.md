# AngularTaskApp

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 13.3.6.

## Instruction

An task application made by Angular framework with angular CLI 13.3.6.
- Learned basic structure of Angular framework with Ngmodule and components.
- Created and updated components and module declarations by Angular CLI command.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Configuration files

- ```package.json```: The package. json file is the heart of any Node project. It records important metadata about a project which is required before publishing to NPM, and also defines functional attributes of a project that npm uses to install dependencies, run scripts, and identify the entry point to our package.
- ```tsconfig.json```: A given Angular workspace contains several TypeScript configuration files. At the root tsconfig.json file specifies the base TypeScript and Angular compiler options that all projects in the workspace inherit.
- ```angular.json```: A file named angular.json at the root level of an Angular workspace provides workspace-wide and project-specific configuration defaults for build and development tools provided by the Angular CLI. Path values given in the configuration are relative to the root workspace folder.

## Structure of Angular source

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/182270093-4dcf2b86-9439-4bf5-aa1c-e5a53f06523a.png">


- ```NgModule```: @NgModule takes a metadata object that describes how to compile a component's template and how to create an injector at runtime. It identifies the module's own components, directives, and pipes, making some of them public, through the exports property, so that external components can use them.
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

- ```Component```: Angular components are a subset of directives, always associated with a template. Unlike other directives, only one component can be instantiated for a given element in a template. A component must belong to an NgModule in order for it to be available to another component or application.

## Global style code

To implement global styles on this project, we can change ```src/styles.css```.

## Make a new component

Now, let's make a new component with angularCLI. If you want to make ```header```component, ```ng generate component components/header``` command would start adding new component.

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/182270509-2ef1207a-aa66-4412-aec4-af986752fc9f.png">

As you can see, AngularCLI automatically create css, html, and typescript file of header.component. And also, it add ```HeaderComponent``` inside of ```app.module.ts``` file ```declarations``` part like below.

```typescript
@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

```header.component.ts``` file has Component annotation and OnInit interface like below.

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css']
})
export class HeaderComponent implements OnInit {

  constructor() { }

  ngOnInit(): void {
  }

}
```

- ```OnInit```: A lifecycle hook that is called after Angular has initialized all data-bound properties of a directive. Define an ngOnInit() method to handle any additional initialization tasks such as HTTP request. interface OnInit { ngOnInit(): void }

If you want to make specific style setup, you need to add some code on the ```header.component.css```.

## Embed button component inside of header component

For now, we gonna make another angular component by using angularCLI. Enter the command ```ng generate component components/button```. The button component should be embedded inside of header component. For doing this, you can add ```<app-button></app-button>``` tag on the ```header.component.html```.

```html
<header>
  <h1>{{ title }}</h1>
  <app-button></app-button>
</header>

```

And then, we can see 'button works!' text is added on the localhost page. After changing this tag into <button>Click</button>, there is a button with text click.

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/183690769-62889647-ee12-43c8-adf9-74858f8d9a31.png">

But button component ```color``` and ```text``` should be changed by diffenrent context. For doing this, you can use ```@Input``` and ```[ngStyle]```.

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

As you can see, ```Input``` make variable that can be used inside of html with {{}}. And ```[ngStyle]``` can controll CSS style directly with ```color``` variable.

```html
<button [ngStyle]="{ 'background-color': color }" class="btn">
  {{ text }}
</button>
```

Now you can change color and text of button component on the ```header.component.html``` with attribute.

```html
<header>
  <h1>{{ title }}</h1>
  <app-button color="green" text="Add"></app-button>
</header>
```

## Customize function

Depending on purpose of the button component, you probably want to customize button feature. In order to do this, we should use ```Output``` and ```EventEmitter```.

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

First of all, add a ```(click)``` attribute on the ```<button>``` tag.

```html
<button
  [ngStyle]="{ 'background-color': color }"
  class="btn"
  (click)="onClick()"
>
  {{ text }}
</button>
```

After than, make an ```onClick()``` methon on the ```button.component.ts``` with using ```@Output``` and ```EventEmitter```.

```typescript
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-button',
  templateUrl: './button.component.html',
  styleUrls: ['./button.component.css'],
})
export class ButtonComponent implements OnInit {
  @Input() text: string;
  @Input() color: string;
  @Output() btnClick = new EventEmitter();

  constructor() {
    this.text = '';
    this.color = '';
  }

  ngOnInit(): void {}

  onClick() {
    this.btnClick.emit();
  }
}
```

```btnClick``` method is for sharing data between button and header component. Inside of ```header.component.html```, add a ```(btnClick)="toggleAddTask()`` attribute to get information whether button is clicked or not.

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

And define ```toggleAddTask()``` function on the ```header.component.ts```.

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css'],
})
export class HeaderComponent implements OnInit {
  title: string = 'Task Tracker';
  constructor() {}

  ngOnInit(): void {}

  toggleAddTask() {
    console.log('toggle');
  }
}
```

When you click the button, you can see 'toggle' on the console window like below.

<img width="450" alt="image" src="https://user-images.githubusercontent.com/39740066/183738259-ee6df4aa-3dc5-4d36-aeec-fdd32a79f2c1.png">


## References
- https://youtu.be/3dHNOWTI7H8
- https://heynode.com/tutorial/what-packagejson/
- https://angular.io/guide/typescript-configuration
- https://angular.io/guide/workspace-config
- https://angular.io/api/core/Component
- https://angular.io/api/core/OnInit
