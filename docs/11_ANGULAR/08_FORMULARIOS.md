Formularios

2 técnicas para hacer formularios:
- por template (ngModel)
- reactivos (más modernos y más completos) ngForm

Crear un formulario:
```bash
ng g c formularios/formularioUsuarios
```

De esta forma podríamos meter el HTML dentro del componente, sin un HTML aparte:

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-formulario-usuarios',
  // templateUrl: './formulario-usuarios.component.html',
  template: '<h1></h1>',
  styleUrls: ['./formulario-usuarios.component.css']
})
export class FormularioUsuariosComponent {

}
```

