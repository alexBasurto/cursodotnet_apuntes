# PIPES
Pipes:

```bash
ng g c pipes/angularPipes
```
Ejemplo pipes HTML:
```html
<div class="container">
    <h3><a href="https://angular.io/api?query=pipe" target="_blank">Pipes en Angular</a></h3>
    <p>Pipes básicos de texto</p>
    <ul>
      <li>Original: {{nombre}}</li>
      <li>Pipe uppercase: {{nombre | uppercase}}</li>
      <li>Pipe lowercase: {{nombre | lowercase}}</li>
      <li>Pipe titlecase {{nombre | titlecase}}</li>
    </ul>
  
    <p><a href="https://angular.io/api/common/DatePipe" target="_blank">Pipe date</a></p>
    <ul>
      <li>Original: {{fechaActual}}</li>
      <li>{{fechaActual | date: 'short'}}</li>
      <li>{{fechaActual | date: 'mediumDate'}}</li>
      <li>{{fechaActual | date: 'dd/MM/yyyy'}}</li>
      <li>{{fechaActual | date:'MMMM dd, yyyy'}}</li>
      <li>{{fechaActual | date:'long':'GMT-4'}}</li>
      <li>{{fechaActual | date:'long':'':'fr' }}</li>
    </ul>
  
    <p>Pipes numéricos</p>
    <ul>
      <li>Original: {{facturacion}}</li>
      <li>{{facturacion | number:'1.2-2' }}</li>
      <li>{{facturacion | currency:'EUR':'symbol-narrow':'1.4-4' }}</li>
      <li>{{porcentaje | percent:'2.2-2'}}</li>
    </ul>
  
  </div>
  
```

Ejemplo pipes TS:
```ts
export class AngularPipesComponent {
  nombre = 'Juan Luis ochoa';
  fechaActual = new Date();
  facturacion = 1099898.5454;
  porcentaje = 0.21;

}
```



Configurar idioma:
```ts
// app.module.ts
// Cambiar el locale de la app. Cambiamos el idioma de la aplicación a nivel global
// Importar los idiomas deseados (por lo general será solo el castellano (es))
import localeEs from '@angular/common/locales/es';
import localeFr from '@angular/common/locales/fr';
// Ahora los registramos
import { registerLocaleData } from '@angular/common';

...

registerLocaleData(localeEs);
registerLocaleData(localeFr);
...

 providers: [{ provide: LOCALE_ID, useValue: 'es' }], // Configuramos el idioma por defecto de la app
 ```

Comandos para pipes personalizados
```bash
ng g c pipes/pipesPersonalizados
ng g p pipes/almacenamientoArchivos
ng g p pipes/almacenamientoArchivosMultiple
ng g p pipes/distancias
```