# PRIMEROS PASOS - TRABAJANDO CON ANGULAR
## Crear PROYECTO
```bash
ng new NombreProyecto
```
---------------------------------------

## NG SERVE

Comando ng serve: este comando compila la aplicación Angular y la sirve en un servidor de desarrollo local. Por defecto, la aplicación estará disponible en la URL `http://localhost:4200/`.
No crea archivos en el disco, sino que mantiene la compilación en la memoria, lo que permite tiempos de compilación más rápidos y recompilaciones incrementales cuando se realizan cambios en el código.
```bash
ng serve -o
```

El flag `-o` ó `--open` le indica a Angular CLI que, una vez que la aplicación esté compilada y el servidor de desarrollo esté en funcionamiento, debe abrir automáticamente el navegador web predeterminado del sistema y cargar la aplicación en él.

Este comando es especialmente útil para el desarrollo, ya que acelera el proceso de ver los cambios en tu aplicación y proporciona una experiencia de desarrollo más fluida.

## Crear MODULO

Imagina un módulo en Angular como un contenedor para organizar las características y funcionalidades de tu aplicación.
```bash
ng generate module nombreModulo
```
```bash
ng g m nombreModulo
```
Este proceso genera la carpeta del móduo y dentro de la misma el fichero `nombre.module.ts`. Es en esa carpeta donde se almacenarán los componentes hijos.

### Importar módulo

Si queremos acceder desde un módulo a otro módulo, por ejemplo, desde `app` queremos acceder a `shared`, en el fichero `app.module.ts` debemos indicarlo, importando primero el módulo e indicándolo en NgModule después:
```ts
import { SharedModule } from "./shared/shared.module";
// ...
@NgModule({
    // ...
    imports: [
        // ...
        SharedModule
    ]
})
```

En ese punto, en `app.component.html` ya podremos introducir componentes de ese módulo externo.

---------------------------------------

## Crear COMPONENTE
Un componente en Angular es una unidad básica de la interfaz de usuario que incluye una plantilla HTML, una clase TypeScript para manejar la lógica, y, opcionalmente, un archivo de estilo `.css` y un archivo de pruebas unitarias `spec.ts`. Los componentes permiten estructurar y modularizar la aplicación.

```bash
ng generate component nombreModulo/nombreComponente
```

Esto ejecuta los siguientes pasos:
- crea carpeta `nombreComponente` con los siguientes archivos
    - CREATE `ejemplo.component.css`
    - CREATE `ejemplo.component.html`
    - CREATE `ejemplo.component.spec.ts`
    - CREATE `ejemplo.component.ts`
-  UPDATE `padre.module.ts`: añade el componente en la carpeta del módulo padre y lo indica en el código del módulo padre, en el apartado `declarations`, dentro de `@NgModule`.

Un componente siempre es una clase, siguiendo el enfoque de OOP de Angular y TypeScript.

Un componente no puede estar suelto. Tiene que estar dentro de un módulo. Si no especificamos, lo mete en el módulo por defecto que es app.

- Generar el componente **sin archivo de pruebas** (`.spec.ts`), añadir `--skip-tests`:

  ```bash
  ng generate component nombreModulo/nombreComponente --skip-tests
  ```

- Generar el componente **sin generar su propia carpeta**:
  ```bash
  ng generate component nombreModulo/nombreComponente --flat
  ```

- Generar el componente **sin archivo CSS**:
  ```bash
  ng generate component nombreModulo/nombreComponente --style=none
  ```

- Generar el componente **sin archivo HTML (inline template)**:
  ```bash
  ng generate component nombreModulo/nombreComponente -t
  ```

### Cómo usar Componentes
Uso común: Para definir elementos reutilizables y encapsular el comportamiento y la lógica en componentes independientes.

El componente define en su modelo (`nombre.component.ts`) en la propiedad de componente `selector` su nombre como **directiva de componente**.

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'AngularBasicos';
}
```

Luego cargamos dichos componentes en los distintos templates refiriendonos a ellos de la siguiente manera:
```ts
  <app-root></app-root>
```

Es posible integrar la plantilla HTML dentro del modelo del componente, manteniendo todo en un único archivo:

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

---------------------------------------

## Crear SERVICIO
Los servicios en Angular son clases que se utilizan para compartir datos y funcionalidades entre diferentes partes de la aplicación, como componentes. Ayudan a mantener el código organizado y promueven la reutilización al centralizar la lógica que no está directamente relacionada con la interfaz de usuario.

Los servicios en Angular también se utilizan para manejar la comunicación con APIs externas. Pueden encapsular la lógica para realizar peticiones HTTP, procesar respuestas y manejar datos provenientes de APIs RESTful u otros servicios externos. Esto ayuda a mantener la separación de preocupaciones y facilita la integración de datos externos en la aplicación Angular.

1. Se genera el servicio:

```bash
ng g s services/nombreServicio
```

2. Se configura el servicio: el servicio es un @Injectable, debemos configurarlo con los módulos `HttpClient` y `Observable` (RXJS) y con los correspondientes métodos GET y POST que consuman y envíen datos a la API Rest.

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class MiServicio {

  private apiUrl = 'https://api.example.com/data'; // URL de la API

  constructor(private http: HttpClient) {}

  obtenerDatos(): Observable<any> {
    return this.http.get<any>(this.apiUrl);
  }

  enviarDatos(datos: any): Observable<any> {
    return this.http.post<any>(this.apiUrl, datos);
  }
}
```

3. Se inyecta el servicio en el componente en el que queramos hacer uso de él.

```ts
import { Component } from '@angular/core';
import { MiServicio } from './mi-servicio.service';

@Component({
  selector: 'app-mi-componente',
  template: `
    <button (click)="getData()">Obtener Datos</button>
    <button (click)="sendData()">Enviar Datos</button>
  `
})
export class MiComponente {

  constructor(private miServicio: MiServicio) {}

  getData() {
    this.miServicio.obtenerDatos().subscribe(data => {
      console.log('Datos obtenidos:', data);
    });
  }

  sendData() {
    const datos = { mensaje: 'Hola desde Angular' };
    this.miServicio.enviarDatos(datos).subscribe(response => {
      console.log('Datos enviados:', response);
    });
  }
}

```

4. Importamos el módulo `HttpClientModule` en el módulo principal de la aplicación.

Más adelante, en el apartado [API Service](./09_API_SERVICE.md) se profundiza en los servicios, cómo configurarlos y como implantarlos.


------------------------------


