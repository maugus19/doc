# Repaso de Angular con Ejemplos

## 1. Overview de Angular

### ¿Qué es Angular?
- **Angular** es un framework de desarrollo frontend desarrollado por Google.
- Orientado a la construcción de aplicaciones SPA (Single Page Applications).

### Principales Características
- **Basado en TypeScript**: Mejora la productividad del desarrollo con tipado estático.
- **Arquitectura modular**: Facilita la escalabilidad y el mantenimiento.
- **Soporte para componentes y directivas**: Divide la UI en piezas reutilizables.
- **Integración de herramientas**: Como RxJS para manejo reactivo y Angular CLI para automatización.

---

## 2. Routing en Angular

### Importancia
El sistema de routing permite manejar las rutas de una SPA, cargando vistas dinámicamente sin recargar toda la página.

### Configuración Básica
1. Crea el módulo de rutas:
    ```bash
    ng generate module app-routing --flat --module=app
    ```

2. Configura rutas en `app-routing.module.ts`:
    ```typescript
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { HomeComponent } from './home/home.component';
    import { AboutComponent } from './about/about.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'about', component: AboutComponent },
      { path: '**', redirectTo: '' },
    ];

    @NgModule({
      imports: [RouterModule.forRoot(routes)],
      exports: [RouterModule],
    })
    export class AppRoutingModule {}
    ```

3. Usa `<router-outlet>` en `app.component.html`:
    ```html
    <nav>
      <a routerLink="/">Home</a>
      <a routerLink="/about">About</a>
    </nav>
    <router-outlet></router-outlet>
    ```

### Lazy Loading
1. Crea un módulo con lazy loading:
    ```bash
    ng generate module features/dashboard --route=dashboard --module=app
    ```
2. Esto genera automáticamente el código necesario para cargar el módulo bajo demanda.

### Manejo de Parámetros de Ruta
```typescript
import { ActivatedRoute } from '@angular/router';
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-item-detail',
  template: `<p>Item ID: {{ itemId }}</p>`
})
export class ItemDetailComponent implements OnInit {
  itemId!: string;

  constructor(private route: ActivatedRoute) {}

  ngOnInit(): void {
    this.itemId = this.route.snapshot.paramMap.get('id') || '';
  }
}
```

---

## 3. Conceptos Relevantes

### Componentes y Módulos
Un componente es una unidad básica de Angular:
```typescript
@Component({
  selector: 'app-example',
  template: `<h1>{{ title }}</h1>`
})
export class ExampleComponent {
  title = 'Hello Angular';
}
```
Los módulos agrupan componentes:
```typescript
@NgModule({
  declarations: [ExampleComponent],
  imports: [],
  bootstrap: [ExampleComponent],
})
export class AppModule {}
```

### Directivas
- **Estructurales**:
    ```html
    <div *ngIf="isVisible">Visible</div>
    ```
- **De Atributos**:
    ```html
    <button [disabled]="!isEnabled">Click</button>
    ```

### Data Binding
- **Interpolación**:
    ```html
    <p>{{ title }}</p>
    ```
- **Property Binding**:
    ```html
    <img [src]="imageUrl" />
    ```
- **Event Binding**:
    ```html
    <button (click)="handleClick()">Click me</button>
    ```
- **Two-Way Binding**:
    ```html
    <input [(ngModel)]="username" />
    ```

### Lifecycle Hooks
- `ngOnInit` para inicialización:
    ```typescript
    ngOnInit() {
      console.log('Component initialized');
    }
    ```

---

## 4. Signals

### Introducción a Signals
Signals son un nuevo mecanismo para manejar el estado reactivo en Angular, introducido en Angular 16.

### Ejemplo
1. Configuración básica:
    ```typescript
    import { Component, signal, computed, effect } from '@angular/core';

    @Component({
      selector: 'app-signals-demo',
      template: `
        <p>Count: {{ count() }}</p>
        <p>Double Count: {{ doubleCount() }}</p>
        <button (click)="increment()">Increment</button>
      `,
    })
    export class SignalsDemoComponent {
      count = signal(0);
      doubleCount = computed(() => this.count() * 2);

      constructor() {
        effect(() => console.log('Count changed:', this.count()));
      }

      increment() {
        this.count.update((current) => current + 1);
      }
    }
    ```
2. Resultado:
    - Muestra el conteo actual y su valor duplicado.
    - Actualiza dinámicamente el estado al hacer clic en el botón.
