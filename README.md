# AngularTaskApp

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 13.3.6.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Configuration files

- ```package.json```: The package. json file is the heart of any Node project. It records important metadata about a project which is required before publishing to NPM, and also defines functional attributes of a project that npm uses to install dependencies, run scripts, and identify the entry point to our package.
- ```tsconfig.json```: A given Angular workspace contains several TypeScript configuration files. At the root tsconfig.json file specifies the base TypeScript and Angular compiler options that all projects in the workspace inherit.
- ```angular.json```: A file named angular.json at the root level of an Angular workspace provides workspace-wide and project-specific configuration defaults for build and development tools provided by the Angular CLI. Path values given in the configuration are relative to the root workspace folder.

## Structure of Angular source

<img width="450" alt="Screen Shot 2022-07-31 at 2 07 59 AM" src="https://user-images.githubusercontent.com/39740066/182011071-6db1fc5c-a5d0-4b43-ba68-5b42b5ce5851.png">

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

## References
- https://youtu.be/3dHNOWTI7H8
- https://heynode.com/tutorial/what-packagejson/
- https://angular.io/guide/typescript-configuration
- https://angular.io/guide/workspace-config
- https://angular.io/api/core/Component
