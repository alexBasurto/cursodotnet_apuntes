# PRIMEROS PASOS - TRABAJANDO CON ANGULAR
## Crear PROYECTO
```bash
ng new NombreProyecto
```
---------------------------------------

## LIVE PREVIEW
Comando live preview:
```bash
ng serve -o
```

Por defecto, servirá la preview en la URL `http://localhost:4200/`.

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

Como generar el componente **sin archivo de pruebas** (`.spec.ts`), añadir `--skip-tests`:

```bash
ng generate component nombreModulo/nombreComponente --skip-tests
```

Como generar el componente **sin generar su propia carpeta**:
```bash
ng generate component nombreModulo/nombreComponente --flat
```

Como generar el componente **sin archivo CSS**:
```bash
ng generate component nombreModulo/nombreComponente --style=none
```

Como generar el componente **sin archivo HTML (inline template)**:
```bash
ng generate component nombreModulo/nombreComponente -t
```

---------------------------------------

## Crear SERVICIO
Los servicios en Angular son clases que se utilizan para compartir datos y funcionalidades entre diferentes partes de la aplicación, como componentes. Ayudan a mantener el código organizado y promueven la reutilización al centralizar la lógica que no está directamente relacionada con la interfaz de usuario.
Los servicios en Angular también se utilizan para manejar la comunicación con APIs externas. Pueden encapsular la lógica para realizar peticiones HTTP, procesar respuestas y manejar datos provenientes de APIs RESTful u otros servicios externos. Esto ayuda a mantener la separación de preocupaciones y facilita la integración de datos externos en la aplicación Angular.
```bash
ng g s services/nombreServicio
```

### Ejemplo de servicio consultando una API Rest por el método GET:
```ts
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { ICategoriasMeal, IMeals } from './meal.interface';

@Injectable({
  providedIn: 'root'
})
export class RecetasService {
  constructor(private http: HttpClient) {}

  getRecetas(categoria: string): Observable<IMeals> {
    return this.http.get<IMeals>(`https://www.themealdb.com/api/json/v1/1/filter.php?c=${categoria}`);
  }

  getCategorias(): Observable<ICategoriasMeal> {
    return this.http.get<ICategoriasMeal>(`https://www.themealdb.com/api/json/v1/1/list.php?c=list`);
  }
}
```


---------------------------------------


Creación de componente y servicio:
```bash
ng g c rest/recetas
ng g s services/recetas
```


rxjs: librería de MS para trabajar con llamadas a APIs (Tipo fetch pero mejor.). Angular lo incorpora desde hace tiempo.

```ts
import { HttpClient } from '@angular/common/http'; // Importa el servicio HttpClient para hacer peticiones HTTP.
import { Injectable } from '@angular/core'; // Importa el decorador Injectable para que este servicio pueda ser inyectado en otros componentes o servicios.
import { Observable } from 'rxjs'; // Importa Observable para trabajar con datos asíncronos.
import { IMeals } from '../interfaces/meal.interface'; // Importa la interfaz IMeals para tipar la respuesta de la API.

@Injectable({
  providedIn: 'root'
  // Indica que este servicio es un singleton y estará disponible en toda la aplicación.
  // Anteriormente se registraba en el módulo de Angular, pero ahora se hace con providedIn.
})
export class RecetasService {
  // El decorador @Injectable indica que este servicio puede ser inyectado en cualquier parte de la aplicación.
  // 'providedIn: 'root'' significa que el servicio es un singleton y estará disponible en toda la aplicación.

  constructor(private http: HttpClient) {}
  // El constructor inyecta el HttpClient en el servicio para permitir hacer peticiones HTTP.

  getRecetas(categoria: string): Observable<IMeals> {
    // Método que recibe una categoría como parámetro y devuelve un Observable de tipo IMeals.
    // Este método realiza una petición GET a la API de TheMealDB para obtener recetas filtradas por categoría.

    return this.http.get<IMeals>(`https://www.themealdb.com/api/json/v1/1/filter.php?c=${categoria}`);
    // Hace una petición GET a la URL de la API, incluyendo la categoría en la query string.
    // La respuesta de la API es tipada como IMeals.
  }
}

```

Incluir siempre el httpClientModule en el import de `app.module.ts`:

```ts
  imports: [
    BrowserModule,
    FormsModule,
    HttpClientModule
  ]
  ```

Dentro del método que llama a la API:
  - next - como try
  - error - como catch
  - continue - como finally

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

##### Módulo de enrutamiento:

Nuevo módulo para enrutar: generar módulo
```bash
ng g m appRouting
```
Me genera el módulo dentro de la carpeta app en una carpeta llamada igual que el módulo.
Por buenas prácticas, movemos el archivo `app-routing.module.ts` dentro de la carpeta `app`. Y borramos la carpeta que queda vacía.

En el archivo `app-routing.module.ts` agregamos lo siguiente, entre ello un array de rutas:
```ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { NotFoundComponent } from './not-found/not-found.component';
import { BindingComponent } from './introduccion/binding/binding.component';
import { DirectivasComponent } from './introduccion/directivas/directivas.component';
import { AngularPipesComponent } from './pipes/angular-pipes/angular-pipes.component';
import { PipesPersonalizadosComponent } from './pipes/pipes-personalizados/pipes-personalizados.component';

  
const appRoutes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'binding', component: BindingComponent},
  { path: 'directivas', component: DirectivasComponent},
  { path: 'pipes-angular', component: AngularPipesComponent},
  { path: 'pipes-personalizados', component: PipesPersonalizadosComponent},
  { path: '**', component: NotFoundComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
```


------------------------

