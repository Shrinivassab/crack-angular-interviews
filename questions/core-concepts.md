# 📘 Angular Core Concepts – Interview Q&A

This document covers foundational Angular topics, formatted for quick review and interview prep. Each question includes a linkable anchor for direct reference.

---

## 📑 Table of Contents

- [1. What is an Angular Module?](#1-what-is-an-angular-module)
- [2. What are the key parts of an Angular Component? ](#2-what-are-the-key-parts-of-an-angular-component)
- [3. Explain Angular Lifecycle Hooks](#3-explain-angular-lifecycle-hooks)
- [4. What is Dependency Injection (DI) in Angular?](#4-what-is-dependency-injection-di-in-angular)
- [5. What are the types of Data Binding in Angular?](#5-what-are-the-types-of-data-binding-in-angular)
- [6. Bonus: Structural vs Attribute Directives](#6-bonus-structural-vs-attribute-directives)

---

### 1. What is an Angular Module?

Angular Modules (`NgModule`) are containers that group related components, directives, pipes, and services.

```ts
@NgModule({
  declarations: [AppComponent, DashboardComponent],
  imports: [BrowserModule, FormsModule],
  providers: [UserService],
  bootstrap: [AppComponent]
})
export class AppModule {}

:information_source: ℹ️ Use feature modules (UserModule, AdminModule) to improve separation of concerns and lazy loading.

[🔙 Back to Question](#1-what-is-an-angular-module)

