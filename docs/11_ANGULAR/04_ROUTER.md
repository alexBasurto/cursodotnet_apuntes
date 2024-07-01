# ROUTER


## Módulo de enrutamiento:

Nuevo módulo para enrutar: primero generamos su módulo.

```bash
ng g m appRouting --flat
```

Generará el módulo dentro de la carpeta app en una carpeta llamada igual que el módulo.
Por buenas prácticas, generamos `app-routing.module.ts` sin carpeta propia, dentro de la carpeta `app`.

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
2. En el módulo APP (app.module.ts): importar `AppRouterModule` e importar `HttpClientModule`.

Si la ruta es de un componente en diferido, debe ser alimentada de una forma distinta. Lo veremos más adelante en el apartado de DIFERIDO.
