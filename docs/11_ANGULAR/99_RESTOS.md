


## Crear una interfaz con `Paste JSON as Code`.
Explicar


En **binding.component.ts** se define el componente:
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-binding',
  templateUrl: './binding.component.html',
  styleUrls: ['./binding.component.css']
})
export class BindingComponent {

}
```

Para mostrar el componente en la página principal, en **app.component.html** añadimos lo siguiente:
```html
<app-binding></app-binding>
```

Esto renderizará el contenido de **binding.component.html**:
```html
<p>binding works!</p>
```






