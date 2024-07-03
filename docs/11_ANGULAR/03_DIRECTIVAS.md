# DIRECTIVAS

En Angular, una directiva es una clase que puede modificar la estructura del DOM (Document Object Model) o el comportamiento de los elementos del DOM. Las directivas permiten añadir o modificar el comportamiento de los elementos en la vista de una aplicación Angular.

Las directivas en Angular son una herramienta poderosa que permite manipular el DOM de manera eficiente y flexible, lo que facilita la creación de interfaces de usuario dinámicas y responsivas.

Visto de otra forma, son una forma de establecer un enlace entre el código TypeScript (TS) y la plantilla HTML. Las directivas permiten aplicar lógica desde el código TypeScript directamente a los elementos del DOM, modificando su apariencia, comportamiento y estructura. Este enlace es una de las capacidades clave que hacen de Angular un framework poderoso para desarrollar aplicaciones web dinámicas y ricas en funcionalidades.

- [Directivas de Atributo](#directivas-de-atributo)
    - [ngClass](#ngclass)
    - [ngStyle](#ngstyle)
    - [ngModel](#ngmodel)
- [Directivas Estructurales](#directivas-estructurales)
    - [ngIf](#ngif)
    - [ngFor](#ngfor)
    - [ngSwitch](#ngswitch)
- [Directivas de Componente](#directivas-de-componente)
- [Directivas Personalizadas](#directivas-personalizadas)

----

## Directivas de Atributo

Modifican la apariencia o comportamiento de un elemento.
Enlazan propiedades y métodos del componente con el DOM.
Nos permite aplicar o eliminar clases CSS, modificar estilos en línea, y manipular propiedades del DOM.Ex

### ngClass

Aplica o elimina una o más clases CSS a un elemento.
Cuando deseas que la clase dependa de una condición en tu modelo:

```html
<div [ngClass]="{'highlight': isHighlighted}">Contenido</div>
```

En este caso, en el archivo TS del componente, existirá una variable booleana llamada highlight. Podríamos añadir lógica en el componente para establecer cuando debe ser true y cuando false.

```ts
// ...
export class ExampleComponent {
    // ...
    isHighlighted: boolean = true; // O false, dependiendo del estado inicial
    // ...
}
```

Cabe destacar que si queremos añadir una **clase estática**, sin hacer uso de la lógica de Angular, aplicamos la clase como se haría en un HTML plano:

```html
<div class="highlight">Contenido</div>
```

### ngStyle

Permite aplicar estilos en línea dinámicamente a un elemento del DOM. Funciona enlazando un objeto de estilos CSS al elemento y actualizando esos estilos en función de las expresiones evaluadas en el modelo (TypeScript).

La sintaxis básica de ngStyle utiliza corchetes [] para enlazar la propiedad ngStyle a un objeto de estilos en el modelo. El objeto puede contener cualquier propiedad CSS válida como claves y los valores correspondientes para esas propiedades.

#### Ejemplo básico de ngStyle

Supongamos que quieres cambiar el color de fondo y el tamaño de la fuente de un elemento basado en algunas propiedades del modelo:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styles: []
})
export class ExampleComponent {
  isImportant: boolean = true;
  fontSize: string = '20px';

  getStyles() {
    return {
      'background-color': this.isImportant ? 'yellow' : 'white',
      'font-size': this.fontSize
    };
  }
}
```

```html
<div [ngStyle]="getStyles()">Contenido importante</div>
<button (click)="isImportant = !isImportant">Toggle Importance</button>
<button (click)="fontSize = fontSize === '20px' ? '25px' : '20px'">Toggle Font Size</button>

```

### ngModel

Se utiliza en formularios para enlazar datos entre el modelo (TS) y la vista (HTML).

Es una directiva en Angular que facilita el enlace bidireccional de datos entre los elementos del formulario y las propiedades del modelo (clase del componente en TypeScript). Esto significa que cualquier cambio en el valor del elemento del formulario actualizará automáticamente la propiedad del modelo, y viceversa.

#### Cómo funciona ngModel

1. Enlace Bidireccional: Permite que los datos fluyan en ambas direcciones. Los cambios en el formulario actualizan el modelo y los cambios en el modelo actualizan el formulario.
2. Formulario Reactivo: Facilita la creación y manipulación de formularios reactivos y dinámicos.
3. Validación: Trabaja bien con las capacidades de validación de Angular para formularios.

#### Ejemplo básico de ngModel

La directiva ngModel se usa en el elemento `<input>` para enlazar la propiedad userName del componente.
`[(ngModel)]="userName"` crea un enlace bidireccional. Cualquier cambio en el input actualiza userName y cualquier cambio en userName actualiza el input.
El texto en el párrafo <p> se actualiza automáticamente cuando userName cambia.

```html
<!-- Asegúrate de que FormsModule esté importado en tu módulo (app.module.ts) -->
<input [(ngModel)]="userName" placeholder="Enter your name">
<p>Hello, {{ userName }}!</p>
```

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  userName: string = '';
}
```

#### Habilitando ngModel
Para usar ngModel, debes asegurarte de haber importado el módulo FormsModule en tu módulo principal (generalmente AppModule):

```ts
// ...
import { FormsModule } from '@angular/forms';

@NgModule({
    // ...
  imports: [
    // ...
    FormsModule
  ]
  // ...
})
export class AppModule { }
```
Más adelante, en el apartado de [FORMULARIOS](./08_FORMULARIOS.md) profundizaremos sobre el uso de **ngModel**.

----

## Directivas Estructurales

Las directivas estructurales en Angular son un tipo especial de directivas que permiten cambiar la estructura del DOM, es decir, añadir o quitar elementos del DOM basándose en condiciones específicas. Estas directivas afectan directamente a los elementos del DOM y su contenido. Las tres directivas estructurales más comunes en Angular son ngIf, ngFor y ngSwitch.

Estas directivas no solo simplifican la manipulación del DOM en respuesta a cambios de datos, sino que también promueven un código más claro y mantenible al separar la lógica de presentación de la lógica de negocio en las aplicaciones Angular.

### ngIf
Añade o quita un elemento del DOM basado en una condición booleana.

Ejemplo ngIf 1:

```html
<div *ngIf="isLoggedIn">Welcome, user!</div>
```

```ts
export class ExampleComponent {
  isLoggedIn: boolean = true;
}
```

Ejemplo ngIf 2:

```html
<button (click)="toggleDisplay()">Toggle Display</button>

<div *ngIf="isVisible">
  <p>Este texto se muestra si isVisible es true.</p>
</div>
```

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html',
  styleUrls: ['./example.component.css']
})
export class ExampleComponent {
  isVisible: boolean = true;

  toggleDisplay() {
    this.isVisible = !this.isVisible;
  }
}
```

### ngFor
Repite un elemento del DOM por cada ítem en una lista o colección.

Ejemplo ngFor:
Se itera sobre el array `users`, y en cada iteración se genera un elemento `li` con cada uno de los *name* del array.

```html
<ul>
  <li *ngFor="let user of users">{{ user.name }}</li>
</ul>
```

```ts
export class ExampleComponent {
  users = [
    { name: 'Alice' },
    { name: 'Bob' },
    { name: 'Charlie' }
  ];
}
```

### ngSwitch
Permite mostrar uno de varios elementos del DOM basándose en una expresión.
Útil para mostrar diferentes elementos dependiendo del valor de una expresión.

Ejemplo ngSwitch:

```html
<div [ngSwitch]="user.role">
  <div *ngSwitchCase="'admin'">Admin Content</div>
  <div *ngSwitchCase="'user'">User Content</div>
  <div *ngSwitchDefault>Guest Content</div>
</div>
```

```ts
export class ExampleComponent {
  user = { role: 'admin' };
}
```

----

## Directivas de Componente

Las directivas de componente en Angular son personalizaciones que se aplican a elementos del DOM para agregar comportamientos específicos. Son similares a las directivas estándar como `*ngIf` y `*ngFor`, pero personalizadas para necesidades específicas de la aplicación.

### Ejemplo breve de directivas de componente:

Supongamos que queremos crear una directiva que cambie el color de fondo de un elemento cuando se pasa el ratón por encima. Aquí está cómo podríamos definir y usar esa directiva:

```typescript
// Definición de la directiva
import { Directive, ElementRef, HostListener, Renderer2 } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {

  constructor(private el: ElementRef, private renderer: Renderer2) { }

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string | null) {
    this.renderer.setStyle(this.el.nativeElement, 'background-color', color);
  }
}
```

```html
<!-- Uso de la directiva -->
<p appHighlight>
  Pasa el ratón por encima de este párrafo para ver el efecto de resaltado.
</p>
```

- **Directiva (`HighlightDirective`):** Es una clase decorada con `@Directive` que define el comportamiento (`highlight`) que se aplicará a elementos del DOM con el atributo `appHighlight`.

- **`@HostListener`:** Escucha eventos del elemento DOM al que se aplica la directiva (`mouseenter` y `mouseleave`) y ejecuta métodos específicos (`onMouseEnter` y `onMouseLeave`) para cambiar el estilo del elemento.

- **`ElementRef` y `Renderer2`:** Permiten acceder al elemento del DOM de manera segura y modificar su estilo (`background-color`) basado en la lógica definida en la directiva.

Las directivas de componente en Angular te permiten añadir comportamientos personalizados a elementos del DOM de forma modular y reutilizable. Son útiles para encapsular lógica específica y mejorar la separación de responsabilidades en tu aplicación Angular.

----

## Directivas Personalizadas

Las directivas personalizadas en Angular son atributos que añades a elementos del DOM para extender su funcionalidad. Te permiten encapsular lógica y comportamiento reutilizable que puede aplicarse a múltiples partes de tu aplicación.

### Ejemplo Directiva Personalizada

Supongamos que queremos crear una directiva que resalte un elemento cuando se hace clic en él:

```ts
// Definición de la directiva
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlightOnClick]'
})
export class HighlightOnClickDirective {

  constructor(private el: ElementRef) { }

  @HostListener('click') onClick() {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }
}
```

```html
<!-- Uso de la directiva -->
<div appHighlightOnClick>
  Haz clic aquí para ver el efecto de resaltado.
</div>
```

- **Directiva (`HighlightOnClickDirective`):** Es una clase decorada con `@Directive` que define el comportamiento (`onClick`) que se aplicará a elementos del DOM con el atributo `appHighlightOnClick`.

- **`@HostListener('click')`:** Escucha el evento de clic en el elemento DOM al que se aplica la directiva y ejecuta el método `onClick`, que cambia el estilo de fondo del elemento a amarillo.

- **`ElementRef`:** Proporciona acceso al elemento DOM al que se aplica la directiva para modificar su estilo directamente en respuesta a eventos.

Las directivas personalizadas en Angular son una forma poderosa de añadir funcionalidad específica a elementos del DOM de manera modular y reutilizable. Permiten separar el comportamiento del componente principal y mejorar la organización y mantenibilidad del código en aplicaciones Angular más complejas.

----

## Diferencia entre Directivas de Componente y Directivas Personalizadas

Las directivas personalizadas y las directivas de componente en Angular son conceptos relacionados pero no son exactamente lo mismo.

1. **Directivas Personalizadas:**
   - Se utilizan para aplicar comportamientos específicos a elementos del DOM estándar (como `<p>`, `<div>`, `<span>`, etc.).
   - Permiten extender la funcionalidad de estos elementos sin necesidad de definir un componente completo con su propia plantilla HTML y estilos CSS.
   - Pueden escuchar eventos del DOM, modificar estilos, interactuar con el elemento directamente, entre otras funcionalidades.

2. **Directivas de Componente:**
   - Aunque técnicamente todos los componentes en Angular son directivas (`@Component` extiende `@Directive`), en el uso común se refiere a directivas de componente como aquellas que representan componentes completos.
   - Estos componentes completos pueden incluir una plantilla HTML, estilos CSS y la lógica necesaria para manejar su comportamiento y datos.
   - Se utilizan para crear piezas reutilizables y modulares de la interfaz de usuario que encapsulan no solo comportamientos sino también presentación y lógica de datos más complejas.

### Ejemplo de Aplicación

- **Directiva Personalizada:** Podrías crear una directiva que resalte un elemento cuando se pasa el ratón por encima.

- **Directiva de Componente:** Crearías un componente completo para representar, por ejemplo, un formulario de registro de usuarios que incluya campos de entrada, validaciones, botones de envío, etc.

En resumen, las directivas personalizadas se utilizan para aplicar comportamientos específicos a elementos HTML estándar, mientras que las directivas de componente se utilizan para encapsular componentes completos que pueden ser reutilizados y contienen tanto lógica como presentación en Angular.