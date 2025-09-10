### useful links

useful javascript cheatsheet: [JavaScript Cheat Sheet & Quick Reference](https://quickref.me/javascript.html)
useful typescript cheatsheet: [TypeScript Cheat Sheet & Quick Reference](https://cheatsheets.zip/typescript)
useful html cheatsheet: [HTML Cheat Sheet & Quick Reference](https://quickref.me/html.html)
useful css cheatsheet: [CSS 3 Cheat Sheet & Quick Reference](https://cheatsheets.zip/css3)

### introduction

#### web framework
web framework is a software framework designed to make building and deploying websites and web applications easier by:
- preserving state of the web application as user navigates it
- routing - swapping pages by swapping html pieces already stored in memory without communicating with the server is called routing
	- routing preserves the state of the web application
- breaking down website into smaller pieces that are easier to reason about independently - called components
	- component architecture: components can contain other components
#### angular conceptual overview
angular app is built primarily by creating components and services
components define ui and are built from html and ts which are completely separated in their own files (in angular)
components should contain only small amount of logic that is related to just that component itself
services only contain ts logic and no ui elements
services contain heavier logic related to (for example) communicating with the api
#### angular modules
root app component is always the first one loaded when you navigate to the angular app url
subcomponents are loaded depending on the route - this creates angular component hierarchy
angular modules are containers that group all components and services etc. into modules that can be loaded independently of each other
#### typescript features
static typing - allows us to specify data types
- `let name: string;`
assigning wrong value to the typed variable results in compiler throwing an error
##### interfaces
interface definition like this enforces the shape of objects (age is optional here):

```
interface ICat {
	name: string;
	age?: number;
}
```

`let fluffy: ICat;`
##### class properties

```
class Cat {
	name: string;
	color: string;
	constructor (name) {
		this.name = name;
	}
}
```

public/private accessibility
- class members are public by default
- if i want to make them inaccessible outside of the class, i can use `private` keyword, and this code is shorthand for creating private properties that are initialized via the constructor:

```
class Cat {
	constructor (private name, private color) {
	}
	private speak() { console.log('My name is: ' + this.name); }
}
```

- trying to access those private members outside of the class will throw compile time error
#### node.js
node.js allows me to run js applications on my pc, and also it has npm which allows me to install angular cli
nvm is node version manager that can help me to easily install and switch between different node.js versions
#### angular cli (command-line interface)
is used for
- creating and running a new angular project
- adding items (like components and services) to an existing project
- building an app for production deployment
##### `npm install -g @angular/cli`
run this to install the latest version globally on your system
##### `ng new joes-robot-shop`
run this to create a new angular project in the folder named `joes-robot-shop`
##### `npm start`
run this command in your project directory to start your angular app 
this will start up a web server, which will load angular, and then serve `index.html` file
in the `index.html` file you will find angular component selector `<app-root></app-root>` which is the root of your application - it loads the `app` component
`app` component is at the top of component hierarchy - it is loaded first and it loads everything else
##### `npm install`
run this to install all the npm dependencies for your angular project

### components

#### `ng generate component home`
Use this CLI command to generate new components.
It will create the folder named `home`, add CSS, HTML, spec.ts, and ts files to the folder (named according to conventions), and add the newly created component to the `app.module.ts`.
#### angular component
An **Angular component** is a fundamental building block of an Angular application. It controls a part of the UI (User Interface) and consists of three main parts:
1. **Template**: The HTML view for the component.
2. **Styles**: The CSS styling applied to the component.
3. **Class**: The TypeScript class containing the component’s logic and behaviour.
#### prefixes
Angular uses **prefixes** to differentiate component names and avoid conflicts.
By default, Angular uses the prefix `app-`, but you can customize it in the `angular.json` file. This prefix is important for naming components in HTML.
Example: `<app-header></app-header> <app-footer></app-footer>`
In the above example, `app-` is the prefix that Angular uses for components in the HTML template.
#### inline html templates and styles
Instead of using separate files for HTML templates and CSS styles, you can define them inline within the component's decorator.
- `template`: Defines the inline HTML template.
- `styles`: Defines the inline CSS styles for this component.
- This method can be useful for small components or when you want everything contained in one file.

```
import { Component } from '@angular/core';

@Component({
	selector: 'app-inline-example',
	template: `<h1>Hello, Angular!</h1>`,
	styles: [`h1 { color: blue; }`]
})
export class InlineExampleComponent { }
```

#### images
In Angular, images can be displayed by linking to them in the `src` attribute of an `<img>` tag.
If the image is part of the assets folder (which is common in Angular applications), it can be accessed via the `src` path.
- An image located in `src/assets/images/` can be referenced as: `<img src="/assets/images/example.png" alt="Description of image">`
- The `alt` attribute provides an alternative text for accessibility.
- The `assets` folder is configured in the `angular.json` file under the `"assets"` array, which tells Angular CLI to copy these files to the build output so they can be served statically.

```
import { Component } from '@angular/core';

@Component({
	selector: 'app-image-display',
	template: `<img src="assets/images/my-image.jpg" alt="Sample image">`,
	styles: [`img { max-width: 100%; height: auto; }`]
})
export class ImageDisplayComponent { }
```

#### lifecycle hooks
Angular components have lifecycle hooks that allow you to run code at specific stages of a component's life. Common lifecycle hooks include:
##### `ngOnInit`
Called once the component is initialized. Typically used for initialization logic, like fetching data.

```
import { Component, OnInit } from '@angular/core';

@Component({
	selector: 'app-lifecycle-example',
	template: `<p>{{ message }}</p>`
})
export class LifecycleExampleComponent implements OnInit {
	message: string;
	ngOnInit(): void {
		this.message = 'Component initialized!';
	}
}
```

##### `ngOnChanges`
Called when an input property changes.

```
import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
	selector: 'app-change-example',
	template: `<p>{{ inputData }}</p>`
})
export class ChangeExampleComponent implements OnChanges {
	@Input() inputData: string;
	
	ngOnChanges(changes: SimpleChanges): void {
		console.log('Changes:', changes);
	}
}
```

##### `ngOnDestroy`
Called when the component is about to be destroyed. Useful for cleanup tasks like unsubscribing from observables.

```
import { Component, OnDestroy } from '@angular/core';

@Component({
	selector: 'app-destroy-example',
	template: `<p>Component will be destroyed soon.</p>`
}) export class DestroyExampleComponent implements OnDestroy {
	ngOnDestroy(): void {
		console.log('Component is about to be destroyed');
	}
}
```

##### other hooks
Other lifecycle hooks include `ngAfterContentInit`, `ngAfterViewInit`, `ngDoCheck`, `ngAfterContentChecked`, and `ngAfterViewChecked`, which provide more fine-grained control over the component lifecycle.
#### Component decorator
The `@Component` decorator marks the class as an Angular component and provides metadata about the component.
- `selector`: Defines the name of the component in HTML.
- `templateUrl`: Points to the HTML file.
- `styleUrls`: Points to the CSS file(s).

```
@Component({
	selector: 'app-root',
	templateUrl: './app.component.html',
	styleUrls: ['./app.component.css']
})
```

##### Data Binding
Angular supports different types of data binding, including:
- **Interpolation**: `{{ variable }}`
- **Property Binding**: `[property]="variable"`
- **Event Binding**: `(event)="handler"`
- **Two-way Binding**: `[(ngModel)]="variable"`
##### Component Communication
Components can communicate with each other using `@Input()` and `@Output()` decorators.        
###### Input:

```
@Component({
	selector: 'app-child',
	template: `<p>{{ parentMessage }}</p>`
}) export class ChildComponent {
	@Input() parentMessage: string;
}
```

###### Output:

```
@Component({
	selector: 'app-child',
	template: `<button (click)="sendMessage()">Send Message</button>`
}) export class ChildComponent {
	@Output() messageEvent = new EventEmitter<string>();
	
	sendMessage() {
		this.messageEvent.emit('Hello from child');
	}
}
```

### angular template syntax
#### Overview
Angular template syntax defines how data from a component is displayed in the view and how user interactions are handled. It combines HTML with Angular-specific directives, bindings, and expressions to create dynamic, interactive UIs.
#### Interpolation
Interpolation allows embedding component data directly into the HTML template using double curly braces. Angular evaluates the expression inside the braces and inserts the result into the DOM as text.

```
<p>{{ title }}</p>
```

This is one-way binding from the component to the view and is evaluated whenever the component’s data changes.
#### Binding to Component Data with Interpolation
Interpolation can be used with any property or method from the component class. Angular automatically updates the view when the bound data changes.

```
<h1>{{ getUserName() }}</h1>
```

Functions used in interpolation should be fast and side-effect free, as they may be called frequently during change detection.
#### Attribute Binding
Attribute binding sets the value of an HTML attribute dynamically. It uses square brackets around the attribute name.

```
<img [src]="imageUrl" [alt]="imageDescription">
```

This differs from interpolation in that it binds directly to the DOM property rather than inserting text content.
These square brackets create a one-way binding from the component to the template.
#### Binding with Functions
Functions can also be used in property or attribute bindings, but they should be efficient to avoid performance issues.

```
<button [disabled]="isButtonDisabled()">Click Me</button>
```

#### Angular Directives Overview
In Angular, **directives** are special markers in templates that tell Angular to attach specific behavior to elements or modify the DOM in some way. They fall into three broad categories:
##### Structural Directives
These change the **layout** of the DOM by adding, removing, or manipulating elements. They typically have a `*` prefix in templates because they work by altering the structure of the view. Examples include looping over data, conditionally displaying content, or dynamically inserting templates.
##### Attribute Directives
These change the **appearance or behaviour** of an existing element, component, or another directive without altering the DOM structure itself. They usually modify styles, classes, or element properties dynamically.
##### Component Directives
Technically, every Angular component is a directive with a template. Components combine logic (class) and view (HTML) into reusable building blocks, and they’re the most common type of directive you’ll work with.
#### Repeating Data with \*ngFor
The `*ngFor` structural directive repeats a template for each item in a collection. It can also expose local variables like `index` or `first`.

```
<li *ngFor="let item of items; let i = index">
  {{ i + 1 }} - {{ item }}
</li>
```

#### Handling Events with Event Bindings
Event binding listens for user actions and calls a component method when the event occurs. It uses parentheses around the event name.

```
<button (click)="onClick()">Click Me</button>
```

Event bindings can pass event objects or other data to the handler.
These parentheses create a one-way binding from the template to the component.
#### Handling Null Values with the Safe Navigation Operator
The safe navigation operator `?.` prevents errors when accessing properties of potentially null or undefined objects.

```
<p>{{ user?.profile?.name }}</p>
```

If any part of the chain is null or undefined, Angular returns `null` instead of throwing an error.
#### Hiding and Showing Content with \*ngIf
The `*ngIf` directive conditionally includes or removes elements from the DOM based on a boolean expression.

```
<p *ngIf="isLoggedIn">Welcome back!</p>
```

It can also use `else` to display alternative content.

```
<p *ngIf="isLoggedIn; else guest">Welcome back!</p>
<ng-template #guest><p>Please log in.</p></ng-template>
```

#### Formatting Data with Angular Pipes
Pipes transform data in the template without changing the underlying component data. They are applied using the pipe `|` character.

```
<p>{{ birthday | date:'longDate' }}</p>
<p>{{ price | currency:'EUR' }}</p>
```

Custom pipes can be created for specialized formatting.
You can pass data to pipes by writing `pipeName:arg1:arg2`, where each colon‑separated value is passed as an argument to the pipe.
#### Other General Topics
Angular templates also support:
- Two-way data binding with `[(ngModel)]` for form controls
- Template reference variables using `#varName` to access DOM elements or directives
- Built-in directives like `ngClass` and `ngStyle` for dynamic styling
- Template expressions that support basic operations but avoid complex logic for maintainability

### styling angular components
#### Overview
Angular provides multiple approaches to styling components, ranging from local styles scoped to a single component to global styles that affect the entire application. Styles can be written in plain CSS or in pre-processor languages like SCSS, and Angular offers built‑in directives for applying styles dynamically.
#### Application‑Wide Styling
Global styles are defined in the `styles.css` or `styles.scss` file at the root of the Angular project. These styles apply to all components unless overridden by component‑scoped styles. Global styles are useful for base typography, layout resets, and shared utility classes.
#### Component‑Scoped Styling and CSS Encapsulation
Each Angular component can have its own style file defined in the `styleUrls` or `styles` property of the `@Component` decorator. By default, Angular uses **View Encapsulation** to scope these styles so they only apply to the component’s template. This prevents unintended style leakage between components. Encapsulation modes include:
- Emulated (default) — styles are scoped using generated attributes.
- None — styles are applied globally without scoping.
- ShadowDom — uses native Shadow DOM encapsulation.

```
@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css'],
  encapsulation: ViewEncapsulation.Emulated
})
```

#### NgClass Directive
`NgClass` allows adding or removing CSS classes dynamically based on component state or expressions. It can accept a string, array, or object mapping class names to boolean conditions.

```
<div [ngClass]="{ 'active': isActive, 'disabled': isDisabled }"></div>
```

#### NgStyle Directive
`NgStyle` applies inline styles dynamically. It accepts an object where keys are CSS property names and values are the styles to apply.

```
<div [ngStyle]="{ 'color': textColor, 'font-size.px': fontSize }"></div>
```

#### Using CSS Frameworks and Pre-processors
Angular supports integrating CSS frameworks like Bootstrap, Tailwind, or Material Design by importing their styles into the global styles file or configuring them in `angular.json`. Pre-processors like Sass (SCSS) can be used for variables, nesting, and mixins, improving maintainability and reusability of styles.
When you create the project you can choose to use CSS or Sass (SCSS).

```
$primary-color: #3498db;

.button {
  background-color: $primary-color;
  &:hover {
    background-color: darken($primary-color, 10%);
  }
}
```

#### Other General Notes
Styles in component templates can also be applied inline using the `style` attribute, though this is less maintainable for complex designs.
Angular Material provides a theming system that integrates with SCSS for consistent design across components.
Utility‑first frameworks like Tailwind can be combined with Angular’s directives for highly dynamic styling.
When mixing global and component styles, be mindful of specificity and encapsulation settings to avoid conflicts.

### communication between angular components
#### Overview
Angular components often need to share data or react to each other’s actions. Communication can occur between parent and child components, between siblings, or across unrelated parts of the application. Angular provides built‑in mechanisms for passing data and emitting events, and common patterns help keep this communication predictable and maintainable.
#### One‑Way Dataflow Pattern
A widely recommended approach is **one‑way dataflow**, often summarized as “data goes down, events go up.” In this pattern:
- Data flows from container (parent) components down to presentational (child) components via inputs.
- Events flow upward from child components to parents via outputs. This separation of responsibilities is sometimes called the **container‑presenter pattern**, where containers handle state and logic, and presenters focus on displaying data and emitting events.
#### Communicating with a Child Using Inputs
The `@Input()` decorator allows a parent component to bind values to a child component’s properties. This is the primary way to pass data down the component tree.

```
// child.component.ts
@Input() title: string = '';
```

```
<!-- parent.component.html -->
<app-child [title]="parentTitle"></app-child>
```

#### Communicating with a Parent Using Outputs
The `@Output()` decorator combined with an `EventEmitter` allows a child component to send events or data back to its parent. This is the primary way to send information upward.

```
// child.component.ts
@Output() saved = new EventEmitter<string>();

saveItem() {
  this.saved.emit('Item saved');
}
```

```
<!-- parent.component.html -->
<app-child (saved)="onItemSaved($event)"></app-child>
```

#### General Component Communication Notes
Sibling components can communicate by sharing a common parent that mediates data and events, or by using a shared service with observables.
For unrelated components, shared services or state management libraries (e.g., NgRx) can provide a central communication channel.
Keeping communication unidirectional simplifies debugging and makes data flow easier to trace.
Avoid tightly coupling components; prefer passing only the data and events needed for the interaction.
Angular’s change detection works best when data is immutable and flows in a single direction.

### creating and using angular services
#### Overview
Angular services are classes that encapsulate reusable logic, data access, or state that can be shared across components. They help keep components lean by moving non‑UI logic out of the component layer. Services are typically provided through Angular’s dependency injection system, making them easy to reuse and test.
#### Creating an Angular Service
You can create an Angular service by typing `ng generate service cart` in the terminal.
This generates a `cart.service.ts` file (and a test file) with `providedIn: 'root'` so it’s available application‑wide.
A service is usually created as a class decorated with `@Injectable()`. This decorator marks the class as available for dependency injection and can specify where it should be provided.

```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  getData() {
    return ['Item 1', 'Item 2', 'Item 3'];
  }
}
```

Using `providedIn: 'root'` makes the service a singleton available throughout the application without needing to add it to a module’s `providers` array.
#### Dependency Injection via Constructor
Angular’s dependency injection system automatically supplies instances of services to classes that declare them as constructor parameters. This is the most common way to use a service in a component.

```
constructor(private dataService: DataService) {}
```

The `private` modifier both declares and assigns the service to a property, making it available throughout the component.
#### Using the `inject()` Function
In addition to constructor injection, Angular provides the `inject()` function for retrieving a service instance directly within a class body or function. This can be useful in standalone components or when constructor injection is not practical.

```
import { Component, OnInit, inject } from '@angular/core';
import { DataService } from '../data.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent implements OnInit {
  private dataService: DataService = inject(DataService);
  items: string[] = [];

  ngOnInit() {
    this.items = this.dataService.getData();
  }
}
```

#### Using a Service in a Component
Once injected, a service’s methods and properties can be used within the component to supply data, trigger actions, or manage state.

```
ngOnInit() {
  this.items = this.dataService.getData();
}
```

#### Encapsulating Business Logic in a Service
Services are ideal for holding business rules, data transformations, and API calls. This keeps components focused on presentation and interaction logic, improving maintainability and testability. By centralizing logic in services, changes can be made in one place and reflected across all components that use the service.
#### Other General Notes
Services can be scoped to specific modules or components by providing them in the `providers` array of that module or component.
Singleton services (provided in root) maintain state across the entire application lifecycle.
Services can depend on other services, and Angular will resolve these dependencies automatically.
For cross‑component communication, services can use RxJS `Subject` or `BehaviorSubject` to emit and subscribe to events.
Keeping services stateless where possible makes them easier to test and reuse.

### making http requests with angular
#### Overview
Angular provides the `HttpClient` service, part of the `@angular/common/http` package, to handle HTTP requests. It supports various request methods such as GET, POST, PUT, DELETE, and more. All HTTP methods return **Observables**, which makes them powerful for handling asynchronous data streams and reactive programming patterns.
#### Observables in HTTP Requests
Observables are way of dealing with asynchronous data.
HTTP methods in Angular return Observables from the RxJS library. Observables are lazy, meaning no request is sent until you subscribe to them. They allow you to handle asynchronous data, chain operators for transformation, and manage cancellation or retries. Subscribing to an Observable triggers the HTTP call, and you can use operators like `map`, `catchError`, and `tap` to process responses.

```
const userDataObs = this.http.get('https://api.example.com/user');

userDataObs.subscribe({
  next: data => console.log('Received data:', data),
  error: err => console.error('Error occurred:', err),
  complete: () => console.log('Request completed')
});
```

`this.http.get(...)` returns an **Observable** that will emit the HTTP response once it arrives.
We store it in `userDataObs` so it’s clear we’re working with an Observable.
`.subscribe({...})` lets us handle:
- `next`: runs when data is received
- `error`: runs if the request fails
- `complete`: runs when the Observable finishes (HTTP Observables complete after one emission)
#### Setting Up the Proxy Server
When working with APIs during development, a proxy configuration can help avoid CORS issues. Angular CLI supports a `proxy.conf.json` file that maps API requests to a backend server. This file is referenced in the `ng serve` command with the `--proxy-config` option. The proxy intercepts matching requests and forwards them to the target server, making development smoother.
You can reference this proxy file in two ways:
- **Via CLI**: `ng serve --proxy-config src/proxy.conf.json`
- **Via** `angular.json`: In the `serve > configurations > development` section, add `"proxyConfig": "src/proxy.conf.json"`. This way, you don’t need to pass the flag every time you run `ng serve`.
Example `proxy.conf.json`:

```
{
  "/api": {
    "target": "http://localhost:3000",
    "secure": false
  }
}
```

#### Making a GET Request
A GET request retrieves data from a server. Using `HttpClient`, you inject it into a service or component and call the `get` method with the desired URL. The method returns an Observable, which you subscribe to in order to access the data.

```
this.http.get<{ id: number }>('https://api.example.com/user')
  .pipe(
    retry(2), // retry the first request up to 2 times if it fails
    switchMap(user => 
      this.http.get<{ id: number; total: number }[]>(`https://api.example.com/orders/${user.id}`)
    ),
    map(orders => orders.map(o => ({ ...o, totalWithTax: o.total * 1.2 }))) // transform each order
  )
  .subscribe({
    next: orders => console.log('Orders with tax:', orders),
    error: err => console.error('Error after retries or in second request:', err),
    complete: () => console.log('All requests completed')
  });
```

`this.http.get<{ id: number }>(...)` sends the first GET request to fetch a user object containing an `id`.
`.pipe(...)` allows chaining of RxJS operators to transform or control the Observable before subscribing.
`retry(2)` if the first request fails, it will automatically retry up to 2 more times before passing the error downstream.
`switchMap(...)` takes the result of the first request (`user`) and uses its `id` to make a second GET request for that user’s orders.
- Cancels any previous inner request if a new value comes in (important for streams, but here it just runs once).
`map(...)` is an RxJS operator that transforms the emitted value(s) from the Observable before they reach the subscriber.
- It does **not** change the original source Observable — it creates a new one with transformed values.
- In this example, each order object is extended with a `totalWithTax` property.
- `map` is great for shaping or filtering data right in the stream, so your subscription only deals with the final form you need.
`.subscribe({...})` handles the final result from the second request:
- `next`: runs when the orders are received
- `error`: runs if retries fail or the second request errors
- `complete`: runs when the Observable finishes (both requests done)
#### Making a POST Request
A POST request sends data to the server, often to create or update resources. The `post` method takes the URL and the payload as arguments, returning an Observable of the server’s response.

```
this.http.post<{ id: number }>('https://api.example.com/items', { name: 'New Item' })
  .pipe(
    retry(2), // retry the first request up to 2 times if it fails
    switchMap(createdItem =>
      this.http.get<{ id: number; price: number }>(`https://api.example.com/items/${createdItem.id}`)
    ),
    map(item => ({
      ...item,
      priceWithTax: item.price * 1.2 // transform: add a derived property
    }))
  )
  .subscribe({
    next: itemDetails => console.log('Created item details with tax:', itemDetails),
    error: err => console.error('Error after retries or in second request:', err),
    complete: () => console.log('POST + follow-up GET completed')
  });
```

`this.http.post(...)` sends a POST request to create a new item, expecting a response with an `id`.
`.pipe(...)` allows chaining of RxJS operators to transform or control the Observable before subscribing.
`retry(2)` if the POST fails, it will retry up to 2 more times before passing the error downstream.
`switchMap(...)` takes the result of the POST (`createdItem`) and uses its `id` to make a second GET request for the full item details.
`map(...)` transforms the emitted value(s) from the Observable before they reach the subscriber.
- It’s perfect for shaping API responses into the exact form you need.
- In this example, the `item` object is extended with a `priceWithTax` property.
- `map` does not modify the original source Observable — it creates a new one with transformed values.
`.subscribe({...})` handles the final result from the second request:
- `next`: runs when the item details are received
- `error`: runs if retries fail or the second request errors
- `complete`: runs when both requests are done
#### General Notes
Always import `HttpClientModule` in your root module to enable HTTP services. Handle errors gracefully using RxJS operators like `catchError`. Consider using Angular’s `HttpInterceptor` for cross-cutting concerns such as adding authentication headers or logging. For performance and maintainability, encapsulate HTTP logic inside dedicated services rather than calling `HttpClient` directly in components.
