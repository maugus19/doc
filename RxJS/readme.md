# RxJS: Introducción y Conceptos Principales

RxJS (Reactive Extensions for JavaScript) es una biblioteca para la programación reactiva que permite trabajar con flujos de datos de forma asíncrona y basada en eventos. Se utiliza ampliamente en Angular para gestionar datos asíncronos, como las respuestas HTTP, eventos de usuario y actualizaciones en tiempo real.

## Principales Conceptos de RxJS

### 1. Observable
Un Observable es una fuente de datos que puede emitir eventos (valores, errores o una señal de finalización) a lo largo del tiempo. Los datos se transmiten de forma asíncrona.

```typescript
import { Observable } from 'rxjs';

const observable = new Observable(subscriber => {
  subscriber.next('Hello');
  subscriber.next('World');
  subscriber.complete();
});

observable.subscribe(value => console.log(value));
// Output:
// Hello
// World
```

### 2. Observer
Es un objeto que escucha los eventos emitidos por un Observable. Define tres métodos opcionales:
- `next`: Para recibir valores emitidos.
- `error`: Para manejar errores.
- `complete`: Para actuar cuando el Observable finaliza.

### 3. Subscription
Es la conexión activa entre un Observable y un Observer. Permite cancelar la suscripción para detener la recepción de eventos.

```typescript
const subscription = observable.subscribe({
  next: value => console.log(value),
  complete: () => console.log('Done!'),
});

subscription.unsubscribe(); // Finaliza la suscripción.
```

### 4. Operators
Son funciones que transforman, filtran o combinan datos de un Observable. Ejemplo: `map`, `filter`, `mergeMap`.

```typescript
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

of(1, 2, 3)
  .pipe(map(x => x * 2))
  .subscribe(value => console.log(value));
// Output:
// 2
// 4
// 6
```

### 5. Subject
Es un tipo especial de Observable que también actúa como Observer. Permite emitir y suscribirse a eventos manualmente.

```typescript
import { Subject } from 'rxjs';

const subject = new Subject<number>();

subject.subscribe(value => console.log('Observer 1:', value));
subject.subscribe(value => console.log('Observer 2:', value));

subject.next(1);
subject.next(2);
// Output:
// Observer 1: 1
// Observer 2: 1
// Observer 1: 2
// Observer 2: 2
```

## Uso de RxJS en Angular

Angular utiliza RxJS principalmente en:

### 1. HttpClient
Para realizar solicitudes HTTP asíncronas.
```typescript
this.http.get('https://api.example.com/data').subscribe(data => console.log(data));
```

### 2. Formularios Reactivos
Para manejar eventos como cambios en los campos.
```typescript
this.formGroup.get('username')?.valueChanges.subscribe(value => console.log(value));
```

### 3. Router
Para escuchar cambios en parámetros o rutas.
```typescript
this.route.params.subscribe(params => console.log(params));
```

## Ventajas de RxJS
- Manejo eficiente de flujos de datos asíncronos.
- Combinación y transformación de múltiples fuentes de datos.
- Integración sencilla con Angular y otros frameworks.
