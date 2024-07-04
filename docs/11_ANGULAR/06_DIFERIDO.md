# CARGA DE COMPONENTES EN DIFERIDO (LAZYLOAD)
## Cargas en diferido y no diferido:
En Angular, la carga diferida (lazy loading) y la no diferida (eager loading, funcionamiento por defecto) son estrategias utilizadas para gestionar cómo y cuándo se cargan los módulos de la aplicación. Estas estrategias son fundamentales para optimizar el rendimiento y la velocidad de carga de la aplicación, especialmente en aplicaciones grandes y complejas.

----

## Carga Diferida (Lazy Loading)

La carga diferida implica cargar módulos únicamente cuando son necesarios. Esto se hace generalmente en combinación con el enrutador de Angular, de manera que los módulos se cargan solo cuando el usuario navega a una ruta específica que requiere ese módulo. Esta estrategia mejora el rendimiento inicial de la aplicación al reducir el tamaño del paquete que se carga inicialmente.

**Ventajas:**
- Reduce el tiempo de carga inicial de la aplicación.
- Mejora la experiencia del usuario, ya que solo se cargan los módulos necesarios cuando se necesitan.
- Optimiza el uso de los recursos del navegador.

### Implementando la Carga Diferida (Lazy Loading)

1. Generamos el nuevo módulo, llamado `paises.module.ts`.
2. En el enrutador principal, `app-routing.module.ts`, configuramos la ruta al nuevo módulo como carga en diferido con la propiedad `loadChildren`.

  ```ts
  // Array de rutas
  { path: 'paises', loadChildren: () => import('./paises/paises.module').then((m) => m.PaisesModule) }, // Ruta en diferido o Lazy
  { path: 'home', component: HomeComponent }, // Ruta en no-diferido o Eager
  ```

3. Creamos un enrutador para el módulo en diferido, `paises-routing.module.ts`, y configuramos las rutas de su componente principal y de sus componentes hijos:

  ```ts
  // Array de rutas del enrutador del módulo en diferido:
    {
    path: '', // ruta ppal
    component: PaisesComponent, // módulo cargado en ruta ppal
    children: [
      { path: '', redirectTo: '/paises/paises-mundo', pathMatch: 'full' }, // ruta hija
      { path: 'paises-mundo', component: PaisesMundoComponent }, // ruta hija
      {
        path: 'pais-detalles/:pais',
        component: PaisDetallesComponent,
      }, // ruta hija
    ],
    },
  ```

4. Configuramos el módulo nuevo que cargará en diferido, `paises.module.ts`.
5. Creamos los componentes que necesitemos en el nuevo módulo.
6. Ahora cuando carguemos la ruta en diferido `/`, Angular cargará el módulo de manera diferida y mostrará el componente ``.

**Importante:**

- No se tiene que importar ese módulo lazy en `app.module.ts`.
- Cada módulo en diferido tiene que tener su propio sistema de enrutamiento.
- Por cada módulo lazy, crearemos su router correspondiente.

Existe una forma de de generar el nuevo módulo con carga en diferido a través de un comando: este comando genera un nuevo módulo llamado moduleName y configura la ruta de lazy loading automáticamente.

```bash
ng generate module moduleName --route moduleName --module app.module
```

----

## Carga No Diferida (Eager Loading)

La carga no diferida, por otro lado, implica cargar todos los módulos de la aplicación de inmediato cuando se carga la aplicación. Todos los módulos especificados se importan y se inicializan al comienzo, lo cual puede aumentar el tiempo de carga inicial, pero garantiza que todos los recursos estén disponibles de inmediato.

**Ventajas:**
- Todos los módulos están disponibles inmediatamente después de cargar la aplicación.
- Simplifica la configuración del enrutador, ya que no es necesario gestionar la carga diferida.

**Cómo Implementar:**
Simplemente importa los módulos en el módulo raíz de la aplicación (usualmente `AppModule`). Podríamos decir que es el comportamiento por defecto.

Ejemplo:
```typescript
import { FeatureModule } from './feature/feature.module';

@NgModule({
  declarations: [
    // componentes
  ],
  imports: [
    BrowserModule,
    FeatureModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

----

## Comparación y Elección de Estrategia

La elección entre carga diferida y no diferida depende del tamaño y la complejidad de la aplicación, así como de las necesidades específicas de rendimiento.

- **Carga Diferida:** Ideal para aplicaciones grandes con muchas funcionalidades que no siempre son necesarias de inmediato. Mejora el rendimiento inicial y la experiencia del usuario.
- **Carga No Diferida:** Adecuada para aplicaciones más pequeñas o cuando la velocidad de respuesta inmediata de todos los módulos es más importante que la velocidad de carga inicial.

Implementar la estrategia adecuada puede hacer una gran diferencia en la usabilidad y eficiencia de tu aplicación Angular.

----
