# ROUTER


## Módulo de enrutamiento:

En caso de que vayamos a utilizar Angular Routing, es altamente recomendable que al crear nuestro proyecto, durante el asistente, habilitemos con `yes` la opción de `add Angular Routing`. Esta opción configura automáticamente algunas partes esenciales del enrutamiento en Angular, lo cual simplifica el proceso inicial y asegura que todas las dependencias y configuraciones necesarias estén correctamente establecidas desde el principio.

Si decides no habilitar el enrutamiento al crear el proyecto, aún puedes configurarlo manualmente más tarde.

Es buena práctica crear un módulo específico para Routing, el cual luego importaremos en el módulo principal de la aplicación, app. Esto proporciona organización y separación de responsabilidades en el proyecto.

```bash
ng g m appRouting --flat
```

Este comando genera el módulo dentro de la carpeta app en una carpeta llamada igual que el módulo.
Por buenas prácticas, usamos el flag `--flat`, para generar `app-routing.module.ts` sin carpeta propia, dentro de la carpeta `app`.

Ejemplo de `app-routing.module.ts`, entre ello un array de rutas:

```ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { NotFoundComponent } from './not-found/not-found.component';
import { BindingComponent } from './introduccion/binding/binding.component';
import { DirectivasComponent } from './introduccion/directivas/directivas.component';
import { AngularPipesComponent } from './pipes/angular-pipes/angular-pipes.component';
import { PipesPersonalizadosComponent } from './pipes/pipes-personalizados/pipes-personalizados.component';

  
const appRoutes: Routes = [ // ARRAY DE RUTAS
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

Importamos el módulo `appRouting` en el módulo principal `app`:

```ts
// ...
import { AppRoutingModule } from './app-routing.module';

@NgModule({
  declarations: [
    // ...
  ],
  imports: [
    AppRoutingModule
  ],
  // ...
  bootstrap: [AppComponent]
})
export class AppModule { }
```

------------------------

## Aplicando RouterModule en el NavBar
Una vez listo el módulo de enrutamiento, procedemos a alimentar las rutas en la barra de navegación.

1. En el componente NavBar, en su plantilla HTML:
  ```html
  <nav>
    <div>
      <a [routerLink]="['/home']" routerLinkActive="active-link"
        >Home</a
      >
      <a [routerLink]="['/paises']" routerLinkActive="active-link"
        >Paises</a
      >
    </div>
  </nav>
  ```
  
  2. En la plantilla del componente app, alimentaremos el `<router-outlet>`:
  ```html
  <div class="container">
  <app-navbar></app-navbar>
  <div class="mt-4">
    <router-outlet></router-outlet>
  </div>
</div>
  ```

Si la ruta es de un componente en diferido, debe ser alimentada de una forma distinta. Lo veremos más adelante en el apartado de DIFERIDO.

----

[<-- Anterior](./03_DIRECTIVAS.md) | [Inicio Angular](../11_ANGULAR/) | [Siguiente -->](./05_PIPES.md)