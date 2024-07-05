# DECORADORES

Cómo habrás podido observar durante los ejemplos de código anteriores, en Angular utilizamos una serie de anotaciones especiales precedidas por el símbolo @. Estas anotaciones, conocidas como decoradores, son una característica de TypeScript que Angular ha adoptado y adaptado para añadir metadatos a nuestras clases y métodos, facilitando la configuración y el comportamiento del código sin necesidad de modificar su lógica principal. Estos metadatos permiten que Angular entienda cómo las diferentes piezas de la aplicación deben comportarse y cómo deben interactuar entre sí.

Los decoradores son funciones que se aplican a clases, métodos, accesores, propiedades o parámetros, y permiten modificar su comportamiento de manera declarativa. En Angular, los decoradores son fundamentales para definir componentes, directivas, servicios y otros elementos del framework.

A continuación, exploraremos los decoradores más comunes y su uso en el desarrollo con Angular.

1. **@Component**: Define una clase como un componente de Angular y proporciona metadatos sobre el selector, la plantilla, los estilos, etc.
Un componente es una unidad fundamental de Angular que controla una porción de la interfaz de usuario a través de una clase y una plantilla HTML asociada.
   
   ```typescript
   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css']
   })
   export class AppComponent {
     title = 'mi-aplicacion';
   }
   ```

2. **@NgModule**: Define una clase como un módulo de Angular y proporciona metadatos sobre los componentes, directivas, pipes, y otros módulos que forman parte del módulo.
Un módulo es un contenedor que agrupa componentes, directivas, servicios y otros módulos, organizando el código y facilitando su reutilización y carga.

   ```typescript
   @NgModule({
     declarations: [
       AppComponent,
       OtroComponent
     ],
     imports: [
       BrowserModule,
       FormsModule
     ],
     providers: [],
     bootstrap: [AppComponent]
   })
   export class AppModule { }
   ```

3. **@Injectable**: Marca una clase como disponible para la inyección de dependencias.
Un inyectable es una clase que puede ser inyectada como dependencia en otros componentes o servicios, gestionando la creación y el ciclo de vida de los servicios. Por ejemplo, crearíamos un Inyectable para crear el servicio que se comunica con una API externa.

   ```typescript
   @Injectable({
     providedIn: 'root',
   })
   export class DataService {
     constructor(private http: HttpClient) { }
   }
   ```

4. **@Directive**: Define una clase como una directiva estructural o de atributo.
Una directiva es una clase que permite manipular el DOM o modificar el comportamiento de los elementos HTML, ya sea añadiendo nuevas funcionalidades o alterando las existentes.

   ```typescript
   @Directive({
     selector: '[appHighlight]'
   })
   export class HighlightDirective {
     constructor(private el: ElementRef) { }
   }
   ```

5. **@Pipe**: Define una clase como un pipe de transformación en Angular. Los pipes transforman datos en plantillas, como formatear fechas, números, textos, etc.
Es una función que transforma datos en plantillas, permitiendo formatear, modificar o filtrar la información mostrada en la interfaz de usuario.

    ```typescript
    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({
      name: 'capitalize'
    })
    export class CapitalizePipe implements PipeTransform {
      transform(value: string): string {
        if (!value) return value;
        return value.charAt(0).toUpperCase() + value.slice(1).toLowerCase();
      }
    }
    ```

   ```html
   <!-- Suponiendo que "nombre" es una propiedad con el valor "angular" -->
   <p>{{ nombre | capitalize }}</p> <!-- Salida: "Angular" -->
   ```

6. **@Input y @Output**: el decorador `@Input` permite a un componente recibir datos de su componente padre, mientras que el decorador `@Output` permite a un componente emitir eventos a su componente padre.
Estos decoradores son esenciales para la comunicación de datos entre componentes en Angular, facilitando el paso de información y la respuesta a eventos en una aplicación.

    ```typescript
    import { Component, Input, Output, EventEmitter } from '@angular/core';

    @Component({
      selector: 'app-child',
      template: `
        <div>
          <p>{{ childData }}</p>
          <button (click)="notifyParent()">Notify Parent</button>
        </div>
      `,
      styleUrls: ['./child.component.css']
    })
    export class ChildComponent {
      @Input() childData: string;
      @Output() childEvent = new EventEmitter<string>();

      notifyParent() {
        this.childEvent.emit('Message from Child');
      }
    }

    @Component({
      selector: 'app-parent',
      template: `
        <div>
          <app-child [childData]="parentData" (childEvent)="handleEvent($event)"></app-child>
        </div>
      `,
      styleUrls: ['./parent.component.css']
    })
    export class ParentComponent {
      parentData = 'Data from Parent';

      handleEvent(event: string) {
        console.log(event);
      }
    }
    ```
----------------

### Beneficios de los Decoradores

- **Claridad y legibilidad**: Los decoradores proporcionan una sintaxis clara y declarativa para definir metadatos y comportamientos adicionales.
- **Modularidad**: Facilitan la creación de componentes, directivas y servicios modulares y reutilizables.
- **Inyección de dependencias**: Simplifican la configuración y el uso de la inyección de dependencias en Angular.

En resumen, los decoradores en Angular son herramientas poderosas que permiten añadir metadatos y modificar el comportamiento de clases y sus miembros de manera declarativa, mejorando la modularidad y la claridad del código.