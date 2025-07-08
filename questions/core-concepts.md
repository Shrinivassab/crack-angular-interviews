
# 📘 Angular Core Concepts – Interview Q&A

This document covers foundational Angular topics, formatted for quick review and interview prep. Each question includes a linkable anchor for direct reference.

## 📑 Table of Contents <a id="toc"></a>

1. [What is an Angular Module?](#q1-module)  
2. [What are the key parts of an Angular Component?](#q2-component)  
3. [Explain Angular Lifecycle Hooks](#q3-hooks)  
4. [What is Dependency Injection (DI) in Angular?](#q4-di)  
5. [What are the types of Data Binding in Angular?](#q5-binding)  
6. [Bonus: Structural vs Attribute Directives](#q6-directives)
7. [How do Components Communicate?](#q7-components-communicate)
8. [What are Angular Services and why are they Singletons?](#q8-angular-services-singletons)
---

## <a id="q1-module"></a>1. What is an Angular Module?

Angular Modules (NgModule) are containers that group related components, directives, pipes, and services. They define how parts of your application fit together. Every Angular application has at least one root module, conventionally named `AppModule`.

```ts
@NgModule({
  declarations: [AppComponent, DashboardComponent],
  imports: [BrowserModule, FormsModule],
  providers: [UserService],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

ℹ️ **Tip:** Use feature modules (e.g., `UserModule`, `AdminModule`) to improve separation of concerns, organize your application, and enable lazy loading, which can significantly improve initial application load times.

[🔙 Back to Table of Contents](#toc)

---

## <a id="q2-component"></a>2. What are the key parts of an Angular Component?

An Angular component is the fundamental building block of an Angular application's UI. It controls a part of the screen called a view. Each component consists of several key parts:

- `@Component` decorator: Marks a class as an Angular component.
- `selector`: Defines how the component is used in HTML.
- `templateUrl` or `template`: HTML template for the component.
- `styleUrls` or `styles`: Component-specific styles.
- A class: Contains logic and data.
- Template: Defines the UI view.
- Optional styles: Scoped to the component.

```ts
// hello.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-hello',
  templateUrl: './hello.component.html',
  styleUrls: ['./hello.component.css']
})
export class HelloComponent {
  name = 'Shrinivass';
  greet(): string {
    return `Hello, ${this.name}!`;
  }
}
```

[🔙 Back to Table of Contents](#toc)

---

## <a id="q3-hooks"></a>3. Explain Angular Lifecycle Hooks

Angular components and directives have a lifecycle managed by Angular itself. Lifecycle hooks are methods Angular calls at specific points during a component’s life.

- `ngOnChanges()`: Respond to input changes.
- `ngOnInit()`: Initialization logic.
- `ngDoCheck()`: Custom change detection.
- `ngAfterContentInit()`: Projected content ready.
- `ngAfterContentChecked()`: Projected content checked.
- `ngAfterViewInit()`: Component views initialized.
- `ngAfterViewChecked()`: Component views checked.
- `ngOnDestroy()`: Cleanup just before destroy.

[🔙 Back to Table of Contents](#toc)

---

## <a id="q4-di"></a>4. What is Dependency Injection (DI) in Angular?

Dependency Injection (DI) is a design pattern in Angular that provides dependencies to a class rather than creating them inside it.

- Declared in constructors.
- Provided using `@Injectable`, `@NgModule`, or `@Component`.
- Automatically injected by Angular.

```ts
@Injectable({
  providedIn: 'root'
})
export class UserService {
  constructor(private http: HttpClient) {}
  getUsers(): Observable<any[]> {
    return this.http.get<any[]>('/api/users');
  }
}

@Component({
  selector: 'app-root',
  template: `<h1>Users</h1><button (click)="loadUsers()">Load Users</button>`
})
export class AppComponent {
  constructor(private userService: UserService) {}
  loadUsers() {
    this.userService.getUsers().subscribe(users => console.log(users));
  }
}
```

[🔙 Back to Table of Contents](#toc)

---

## <a id="q5-binding"></a>5. What are the types of Data Binding in Angular?

Data binding connects the component’s logic and view.

- **One-Way (Component to View)**:
  - Interpolation: `{{ name }}`
  - Property Binding: `[src]="imageUrl"`

- **One-Way (View to Component)**:
  - Event Binding: `(click)="saveData()"`

- **Two-Way Binding**:
  - `[(ngModel)]="userName"`

```html
<input [(ngModel)]="userName">
<!-- Equivalent to: -->
<input [ngModel]="userName" (ngModelChange)="userName = $event">
```

[🔙 Back to Table of Contents](#toc)

---

## <a id="q6-directives"></a>6. Bonus: Structural vs Attribute Directives

### Structural Directives
- Modify the DOM structure.
- Examples: `*ngIf`, `*ngFor`, `*ngSwitchCase`.

```html
<p *ngIf="isLoggedIn">Welcome!</p>
<ul><li *ngFor="let item of items">{{ item }}</li></ul>
```

### Attribute Directives
- Modify appearance or behavior of existing elements.
- Examples: `[ngClass]`, `[ngStyle]`, `[appHighlight]`.

```html
<p [ngClass]="{ 'highlight': isActive }">Status</p>
<div [ngStyle]="{'font-size.px': 14}">Text</div>
```

[🔙 Back to Table of Contents](#toc)

## <a id="q7-components-communicate"></a>7. How do components communicate (@Input(), @Output(), EventEmitter)

Components communicate primarily through @Input() and @Output() decorators, which define their public API for data flow.    

**Parent-to-Child Communication (@Input())**
- **Mechanism**: The `@Input()` decorator allows a parent component to pass data into a child component.   
- **Usage**: A property in the child component is decorated with `@Input()`. The parent binds a value to this input property using property binding.

```typescript
// child.component.ts
import { Component, Input } from '@angular/core';
@Component({ selector: 'app-child', template: `<p>Child Message: {{ childMessage }}</p>` })
export class ChildComponent {
  @Input() childMessage: string; // Input property
}

// parent.component.html
<app-child [childMessage]="parentData"></app-child>
```

**Child-to-Parent Communication (@Output() and EventEmitter)**
- **Mechanism**: The `@Output()` decorator allows a child component to send data out to its parent. It's typically initialized as an EventEmitter.
- **Usage**: The child component calls `emit()` on its `EventEmitter`instance to send data. The parent listens for this custom event using event binding () in its template. 

```typescript
// child.component.ts
import { Component, Output, EventEmitter } from '@angular/core';
@Component({ selector: 'app-child', template: `<button (click)="sendMessage()">Send to Parent</button>` })
export class ChildComponent {
  @Output() messageToParent = new EventEmitter<string>(); // Output property
  sendMessage() {
    this.messageToParent.emit('Hello from Child!');
  }
}

// parent.component.html
<app-child (messageToParent)="receiveMessage($event)"></app-child>
```
> [!TIP]
> For communication between unrelated components (e.g., siblings), use a shared service with `Observables` to maintain loose coupling and encourage modularity.

[🔙 Back to Table of Contents](#toc)


## <a id="q8-angular-services-singletons"></a>8. What are Angular Services and why are they Singletons?

Angular services are classes designed to encapsulate reusable logic and data that can be shared across various components or modules. Their primary purpose is to organize code, separate concerns, and increase modularity.

### 🧩 Singleton Pattern

**Default Behavior**  
By default, Angular services follow the Singleton Pattern. This means when a service is provided at the root level (`providedIn: 'root'`) or via an eagerly loaded module, Angular creates only **one instance** for the entire application lifecycle.

**Shared Instance**  
This instance is shared across all components, directives, or other services that inject it, ensuring consistency and avoiding duplication.

### ✅ Benefits of Singleton Services

- **Shared State**: Great for holding and managing global or application-level state (e.g., user session, app configuration).
- **Centralized Logic**: Keeps business logic like logging, authentication, and API calls centralized, supporting the DRY principle.
- **Memory Efficiency**: Avoids unnecessary instantiations, conserving memory and improving performance.

[🔙 Back to Table of Contents](#toc)