# Consumo de API REST con Servicios, RXJS y Observables en Angular

En aplicaciones modernas, es común interactuar con APIs REST para obtener y enviar datos. Angular facilita esta tarea a través de servicios (@Injectable) y la biblioteca RXJS para la gestión de operaciones asíncronas. En esta sección, aprenderás a configurar un servicio en Angular para consumir una API REST, utilizando observables para manejar la emisión y recepción de datos de manera eficiente.

## Configuración del Servicio

Para empezar, necesitamos crear un servicio que se encargue de gestionar las peticiones HTTP. Este servicio se define como `@Injectable`, lo que permite que Angular lo inyecte en cualquier componente que lo necesite.

1. **Crear el Servicio**

   Primero, creamos el servicio utilizando el Angular CLI:

   ```bash
   ng generate service services/nombreServicio
   ```

2. **Configurar el Servicio**

   En el archivo `nombre-servicio.service.ts`, configuramos el servicio para que utilice el módulo `HttpClient` de Angular, junto con `RXJS` para manejar los datos como *observables*.

   ```typescript
    import { HttpClient } from '@angular/common/http'; // Importa el servicio HttpClient para hacer peticiones HTTP.
    import { Injectable } from '@angular/core'; // Importa el decorador Injectable para que este servicio pueda ser inyectado en otros componentes o servicios.
    import { Observable } from 'rxjs'; // Importa Observable para trabajar con datos asíncronos. RxJS es una librería para programación reactiva que permite trabajar con flujos de datos asíncronos y eventos.
    import { IPais } from './pais.interface';

    @Injectable()
    providedIn: 'root'
    // El decorador @Injectable junto con 'providedIn: 'root'' indica que este servicio es un singleton y puede ser inyectado en cualquier parte de la aplicación.
    // Anteriormente se registraba en el módulo de Angular, pero ahora se hace con providedIn. 

    export class PaisesService {
    constructor(private http: HttpClient) {}
    // El constructor inyecta el HttpClient en el servicio para permitir hacer peticiones HTTP.

    get<T>(url: string): Observable<T> {
        return this.http.get<T>(url);
    }

    getPaises(): Observable<IPais[]> {
        return this.http.get<IPais[]>('https://restcountries.com/v3.1/all');
    }

    getPais(pais: string): Observable<IPais[]> {
        return this.http.get<IPais[]>('https://restcountries.com/v3.1/name/' + pais + '?fullText=true');
    }
    }
   ```

3. **Inyectar y Utilizar el Servicio en un Componente**

   Ahora, vamos a inyectar el servicio en un componente y usarlo para obtener y enviar datos.

   ```typescript
    import { Component } from '@angular/core';
    import { IPais } from '../pais.interface'; // Importamos interfaz para tipar los datos obtenidos de la API
    import { PaisesService } from '../paises.service'; // Importamos el servicio
    import { Router } from '@angular/router';

    @Component({
    selector: 'app-paises-mundo',
    templateUrl: './paises-mundo.component.html',
    styleUrls: ['./paises-mundo.component.css']
    })
    export class PaisesMundoComponent {
        paises: IPais[] = []; // Declaración de la propiedad paises, con tipo Array-IPais

        constructor(private paisesService: PaisesService, private router: Router) {}
        // Construcción con inyección de dependencias: inyecta el servicio PaisesService, (e inyecta el Router)

        ngOnInit() {
            this.getPaises();
        } // Llama al método getPaises del servicio una vez inicializado el componente

        getPaisesGenerico() { // Método getPaisesGenerico
            this.paisesService.get<IPais[]>('https://restcountries.com/v3.1/all').
            // llama al método get del servicio PaisesService
            subscribe({
            next: (data) => {
                console.log(data);
                this.paises = data;
            },
            error: (err) => console.log(err),
            complete: () => console.log('OK')
            });
        }

        getPaises() { // Método getPaises
            this.paisesService.getPaises()
            // llama al método getPaises del servicio PaisesService
            .subscribe({
            next: (data) => {
                console.log(data);
                this.paises = data;
            },
            error: (err) => console.log(err),
            complete: () => console.log('OK')
            });
        }

        verDetalles(pais: IPais) {
            this.router.navigateByUrl('/paises/pais-detalles/' + pais.name.common);
        }
    }
   ```
4. **Importar HttpClientModule en app.module**
Además, es necesario incluir siempre el HttpClientModule en el import de `app.module.ts`:

```ts
  imports: [
    BrowserModule,
    FormsModule,
    HttpClientModule
  ]
```

## Explicación

- **Servicio `PaisesService`**: Define métodos para obtener (`getData`) y enviar (`postData`) datos. Utiliza `HttpClient` para hacer las peticiones HTTP y RXJS para manejar los datos con observables. `catchError` se usa para manejar errores.
- **Componente `DataComponent`**: Inyecta `PaisesService` y llama a sus métodos. `ngOnInit` se usa para obtener datos al inicializar el componente.

Con esta configuración, tienes una base sólida para consumir y manejar datos de una API REST en Angular utilizando servicios, RXJS y observables.


------------

## Observable y RxJS

En RxJS, los flujos de datos se manejan mediante observables. Cuando un observable se suscribe, se pueden definir tres tipos de manejadores para los eventos del flujo de datos:

**next**: Manejador de datos emitidos (similar a try).
**error**: Manejador de errores (similar a catch).
**complete**: Manejador de finalización (similar a finally).

Analogía con try-catch-finally:
- **next**: Se ejecuta cada vez que el observable emite un valor. Es equivalente a la parte try donde se maneja la lógica principal.
- **error**: Se ejecuta si ocurre un error durante la emisión de datos. Es equivalente a catch, donde se maneja la lógica de error.
- **complete**: Se ejecuta cuando el observable completa su emisión de datos sin errores. Es equivalente a finally, que se ejecuta siempre al final, independientemente de si ocurrió un error o no.

```ts

getRecetas() {
  this.recetasService.getRecetas(this.categoriaSeleccionada).subscribe({
    next: (data: IMeals) => {
      console.log(data);
      this.recetario = data;

      this.mostrarError = false;
    },
    error: (err) => (this.mostrarError = true),
    complete: () => console.log('ok')
  });
}

```