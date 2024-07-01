
## Decoradores en Angular
En Angular, los decoradores son una característica de TypeScript que se utilizan para añadir metadatos a clases, métodos, propiedades o parámetros. Estos metadatos permiten que Angular entienda cómo las diferentes piezas de la aplicación deben comportarse y cómo deben interactuar entre sí.

1. **@Component**: Define una clase como un componente de Angular y proporciona metadatos sobre el selector, la plantilla, los estilos, etc.
   
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
   
   ```typescript
   @Injectable({
     providedIn: 'root',
   })
   export class DataService {
     constructor(private http: HttpClient) { }
   }
   ```

   Inyectable mete un servicio (mejorar frase).

4. **@Directive**: Define una clase como una directiva estructural o de atributo.
   
   ```typescript
   @Directive({
     selector: '[appHighlight]'
   })
   export class HighlightDirective {
     constructor(private el: ElementRef) { }
   }
   ```


## Decorador @Pipe en particular

El decorador `@Pipe` se utiliza para definir una clase como un pipe de transformación en Angular. Los pipes transforman datos en plantillas, como formatear fechas, números, textos, etc.

### Ejemplo de un pipe personalizado

1. **Definición del pipe**:
   
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

2. **Uso del pipe en una plantilla**:
   
   ```html
   <!-- Suponiendo que "nombre" es una propiedad con el valor "angular" -->
   <p>{{ nombre | capitalize }}</p> <!-- Salida: "Angular" -->
   ```

En resumen, los decoradores en Angular permiten definir cómo deben comportarse las distintas partes de una aplicación, mientras que el decorador `@Pipe` en particular se utiliza para crear pipes que transforman datos en las plantillas.

----------------

### Decoradores en Angular

Los decoradores en Angular son funciones especiales que se utilizan para modificar el comportamiento de clases, métodos, propiedades o parámetros. Proporcionan una forma declarativa de añadir metadatos y funcionalidades adicionales a los elementos de la aplicación.

### Tipos de Decoradores en Angular

1. **Decoradores de Clase**
   - **@Component**: Define un componente Angular.
   - **@Directive**: Define una directiva Angular.
   - **@Injectable**: Marca una clase como inyectable, permitiendo que se pueda utilizar en la inyección de dependencias.

   Ejemplo:
   ```typescript
   @Component({
     selector: 'app-ejemplo',
     templateUrl: './ejemplo.component.html',
     styleUrls: ['./ejemplo.component.css']
   })
   export class EjemploComponent {}
   ```

2. **Decoradores de Propiedad**
   - **@Input**: Marca una propiedad como una entrada, permitiendo que un componente padre pase datos a un componente hijo.
   - **@Output**: Marca una propiedad como una salida, permitiendo que un componente hijo emita eventos al componente padre.

   Ejemplo:
   ```typescript
   @Input() dato: string;
   @Output() eventoClick = new EventEmitter<string>();
   ```

3. **Decoradores de Método**
   - **@HostListener**: Escucha eventos en el elemento host del componente o directiva.
   - **@HostBinding**: Vincula una propiedad del host a una propiedad del componente o directiva.

   Ejemplo:
   ```typescript
   @HostListener('click') onClick() {
     // Manejar el evento click
   }
   
   @HostBinding('class.active') isActive = true;
   ```

4. **Decoradores de Parámetro**
   - **@Inject**: Inyecta una dependencia específica en el constructor de una clase.

   Ejemplo:
   ```typescript
   constructor(@Inject('ConfigService') private config) {}
   ```

### Beneficios de los Decoradores

- **Claridad y legibilidad**: Los decoradores proporcionan una sintaxis clara y declarativa para definir metadatos y comportamientos adicionales.
- **Modularidad**: Facilitan la creación de componentes, directivas y servicios modulares y reutilizables.
- **Inyección de dependencias**: Simplifican la configuración y el uso de la inyección de dependencias en Angular.

En resumen, los decoradores en Angular son herramientas poderosas que permiten añadir metadatos y modificar el comportamiento de clases y sus miembros de manera declarativa, mejorando la modularidad y la claridad del código.